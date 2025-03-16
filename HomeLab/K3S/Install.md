### Base OS for Templating:
1. Install Rocky Minimal
2. Set root password
3. Create user `kube` (This is the user used for SSH)
4. Disable FirewallD (Temporary until K3S is installed and working. I.E. not a firewall issue if the install isn't working.)
5. Setup networking. If using Ceph, remember 10G networking.

### K3S Master
First Master:
```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
    --cluster-init \
    --tls-san=<FIXED_IP> # Optional, needed if using a fixed registration address
    --disable=traefik \
    --disable=servicelb
```

Every other master:
```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - server \
	--server https://<ip or hostname of server1>:6443 \
	--tls-san=<FIXED_IP> # Optional, needed if using a fixed registration address
	--disable=traefik \
    --disable=servicelb
```

### K3S LoadBalancer
Loadbalancer uses the same base template but without the 10G networking.

1. Install HAProxy and Keepalived
```bash
sudo dnf install -y haproxy keepalived
```

2. Add the following to `/etc/haproxy/haproxy.cfg` on lb-1 and lb-2:
```bash
frontend k3s-frontend
    bind *:6443
    mode tcp
    option tcplog
    default_backend k3s-backend

backend k3s-backend
    mode tcp
    option tcp-check
    balance roundrobin
    default-server inter 10s downinter 5s
    server master-1 <ip of master1>:6443 check
    server master-2 <ip of master2>:6443 check
    server master-3 <ip of master3>:6443 check
```

3. Add the following to `/etc/keepalived/keepalived.conf` on lb-1 and lb-2:
```bash
global_defs {
  enable_script_security
  script_user root
}

vrrp_script chk_haproxy {
    script 'killall -0 haproxy' # faster than pidof
    interval 2
}

vrrp_instance haproxy-vip {
    interface eth1
    state <STATE> # MASTER on lb-1, BACKUP on lb-2
    priority <PRIORITY> # 200 on lb-1, 100 on lb-2

    virtual_router_id 51

    virtual_ipaddress {
        10.10.10.100/24
    }

    track_script {
        chk_haproxy
    }
}
```

Since we are installing on Rocky Linux, which uses SELinux, we have to allow for HAProxy to bind to any port and allow Keepalived to kill HAProxy once there is a chance in the VIP.
Make sure to start both HAProxy and Keepalived, if they fail to start, don't worry, that would be due to SELinux.

To solve SELinux errors, run the following:
```bash
dnf install -y policycoreutils-python-utils

audit2why -a
```

`audit2why -a` will show all SELinux errors and possible solutions.
For HAProxy the solution should be a one liner: `sudo setsebool -P haproxy_connect_any 1`
This allows HAProxy to connect to any port and thereby proxy port 6443.

For Keepalived, there isn't a one liner available. To resolve those issues we have to first trigger the security violation before we can create our custom policy.
You can trigger it simply by starting Keepalived on both loadbalancers and then stopping the service on the master, forcing the backup to trigger the violation. Repeat on the master simply by turning it back on.
Once the violation has been triggered, run the following:
```bash
ausearch -c 'keepalived' --raw | audit2allow -M my-keepalived
semodule -i my-keepalived.pp
```

This creates a custom policy that allows Keepalived to kill HAProxy. This is needed since HAProxy doesn't bind to the VIP on the backup loadbalancer, since it's not broadcasting the VIP until it becomes the "master".
HAProxy will automatically start back up as long as it has been enabled.

### K3S Worker
```bash
curl -sfL https://get.k3s.io | K3S_TOKEN=SECRET sh -s - agent \
	--server https://<ip or hostname of loadbalancer>:6443
```
