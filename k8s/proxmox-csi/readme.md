# Adding proxmox-csi to talos

1. open and edit config
```bash
talosctl -n 10.0.0.91,10.0.0.92 edit machineconfig --talosconfig talosconfig-ext
```
(make sure that the endpoint is set to the external ip instead of localhost)

2. Add:  

```
cluster:
  externalCloudProvider:
    enabled: true
    manifests:
      - https://raw.githubusercontent.com/sergelogvinov/proxmox-csi-plugin/main/docs/deploy/proxmox-csi-plugin-talos.yml
```

c: https://github.com/sergelogvinov/proxmox-csi-plugin

3. Create secret:

```bash
pveum role add CSI -privs "VM.Audit VM.Config.Disk Datastore.Allocate Datastore.AllocateSpace Datastore.Audit"
```  

```
pveum user add kubernetes-csi@pve
pveum aclmod / -user kubernetes-csi@pve -role CSI
pveum user token add kubernetes-csi@pve csi -privsep 0
```

4. Create the secret from the config.yaml (make sure to replace "secret" with the token from pve and the url of course)

```bash
kubectl -n csi-proxmox create secret generic proxmox-csi-plugin --from-file=config.yaml
```