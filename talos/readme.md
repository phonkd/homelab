# Creating Cluster

## Creating the vm

1. Get a talos no cloud amd64 image from github releases onto proxmox
2. run `unxz DOWNLOADED-FILE`
3. Create vm `qm craete 900`
4. Import disk into vm `qm importdisk 900 DOWNLOADED-FILE PROXMOX-STORAGE-NAME`
5. Add missing tings (network-device, Cloudinit device)
6. Add and potentially resize disk
 ```
talosctl gen config talos-sucks https://TALOS-VM-IP:6443
```

```bash
talosctl apply-config --insecure --nodes TALOS-VM-IP --file controlplane.yaml
```  

Wait about 5 minute and then run:
```bash
talosctl bootstrap  --nodes TALOS-VM-IP --endpoints TALOS-VM-IP --talosconfig talosconfig
```

## Troubleshooting

>[!warning] Disable IPv6 to get it to work


## Joining worker

1. create another talos vm
2. run this command:  
```bash
talosctl kubeconfig --nodes IP-OF-WORKER --endpoints IP-OF-WORKER --talosconfig talosconfig
```

