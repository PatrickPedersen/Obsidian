---
tags:
  - H2
  - Linux
  - DNS
  - Skole
---
Bruger [[Master]] som grundlag

### Technical
IPv4 Config:
Manual
Addresse: 10.0.0.2
Gateway: 10.0.0.1
DNS Server: 127.0.0.1


## Opsætning
denlange.local.db
```SOA
@ IN SOA dns patrick (
						0     ; serial
						3H    ; refresh
						15M   ; retry
						2W    ; expire
						12H ) ; minimum
        NS dns
     IN MX 10 mail
dns  IN A  10.0.0.2
www  IN A  10.0.0.5
dgz  IN A  10.0.0.1
dhcp IN A  10.0.1.2
dg   IN A  10.0.1.1
ftp  IN A  10.0.0.5
sql  IN A  10.0.0.3
mail IN A  10.0.0.5
@    IN A  10.0.0.5
```

0.0.10.db
```SOA
@ IN SOA dns.denlange.local. patrick.denlange.local. (
						0     ; serial
						3H    ; refresh
						15M   ; retry
						2W    ; expire
						12H ) ; minimum
	 IN NS  dns.denlange.local.
5    IN PTR mail.denlange.local.
2    IN PTR dns.denlange.local.
5    IN PTR www.denlange.local.
1    IN PTR dgz.denlange.local.
3    IN PTR sql.denlange.local.
```

1.0.10.db
```SOA
@ IN SOA dns.denlange.local. patrick.denlange.local. (
						0     ; serial
						3H    ; refresh
						15M   ; retry
						2W    ; expire
						12H ) ; minimum
	 IN NS  dns.denlange.local.
1    IN PTR dg.denlange.local.
2    IN PTR dhcp.denlange.local.
```

named.conf
Ændrer følgende linjer efter de andre zoner:
- listen-on port 53 { 127.0.0.1; 10.0.0.2 };
- allow-query { localhost; 10.0.0.0/29 };
- `#dnssec-enable yes;`
- dnssec-validation no;
- forwarders { 10.2.0.9; 10.2.0.10; };
- forward only;
Tilføj følgende linjer:
```
zone "denlange.local" IN {
	type master;
	file "denlange.local.db";
	allow-update    {none;};
};

zone "0.0.10.in-addr.arpa" IN {
	type master;
	file "0.0.10.db";
	allow-update    {none;};
};

zone "1.0.10.local" IN {
	type master;
	file "1.0.10.db";
	allow-update    {none;};
};
```
