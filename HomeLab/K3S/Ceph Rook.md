On Proxmox:
```bash
curl -s https://raw.githubusercontent.com/rook/rook/release-1.16/deploy/examples/create-external-cluster-resources.py > create-external-cluster-resources.py

# Enable Prometheus
ceph mgr module enable prometheus

python3 create-external-cluster-resources.py --rbd-data-pool-name k3s --namespace rook-ceph-external --format bash
```
Copy and paste the output to your ~/.bashrc file and source it.

```bash
curl -s https://raw.githubusercontent.com/rook/rook/release-1.16/deploy/examples/import-external-cluster.sh > import-external-cluster.sh
./import-external-cluster.sh
```
The import script will read the environment variables and create the necessary resources.

Install Rook:
```bash
export operatorNamespace="rook-ceph"
export clusterNamespace="rook-ceph-external"
curl -s https://raw.githubusercontent.com/rook/rook/release-1.16/deploy/charts/rook-ceph/values.yaml > values.yaml
curl -s https://raw.githubusercontent.com/rook/rook/release-1.16/deploy/charts/rook-ceph-cluster/values-external.yaml > values-external.yaml
helm install --create-namespace --namespace $operatorNamespace rook-ceph rook-release/rook-ceph -f values.yaml
helm install --create-namespace --namespace $clusterNamespace rook-ceph-cluster \
	--set operatorNamespace=$operatorNamespace rook-release/rook-ceph-cluster -f values-external.yaml
```

Setup takes a couple minutes. You can check the status with the following commands:
```bash
kubectl -n rook-ceph-external get cephcluster
kubectl get sc
```