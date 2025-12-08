### Creating Experimental windows node. 
Experimental windows node for proxmox for testing/dev purposes only. Proccess uses the same imagebuild process though in this case the docker container is not built so we will be using 
make to complete these tasks. Guide assumes you have already created caprox user/token from quickstart. Current implementation uses EVAL verision of windwos 2022 future release will include
instructions on setting up with LTSC, or Volume License ISOs

#### Setup:
Add the following ISO for virtio tools to ISO store: 
https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win.iso
- future release will auto pull from url, but for now install manually
- build assumes using iso with name of : virtio-win-0.1.285.iso. Future builds will include options to repalce this. 
Clone github:
```
git clone https://github.com/plouton24/image-builder.git
git pull --tags origin add-proxmox-windows
```
Set env vars: 
```
export PROXMOX_URL="https://<ip/fqdn>:8006/api2/json"
export PROXMOX_USERNAME='caprox@pve!capi'
export PROXMOX_TOKEN=<>
export PROXMOX_NODE="<nodename | "pve">
export PROXMOX_ISO_POOL=<iso_store | "local">
export PROXMOX_BRIDGE=< bridge | "vmbr0">
export PROXMOX_STORAGE_POOL=<"local-lvm">
export PATH=./local/.bin:$PATH
#T H I S assumes we are using the same iso pool for all isos. 
export ISO_FILE="${PROXMOX_ISO_POOL}:iso/SERVER_EVAL_x64FRE_en-us.iso"
```
Run Builder: 
```
cd image-builder/images/capi/
make build-proxmox-windows-2022
```
- You may need to load VM console manually start cd boot:
  - There are kinks needing to be worked on first boot command, as some systems boot up faster then others and console input may be delayed or fail to register
  - ATM boot commands sends 3x <space> commands to console 1 for each ISO(Windows,Scripts, and virtio)
- process should take around 15-40 minutes depending on host speed.
  - You can watch VM console for any issues:
    - If WINRM is still waiting, but console boots to server SCONFIG panel(Page showing option 1-15) there was an error with unattend.xml parsing
      - Troubleshoot: C:\Logs\*.logs, C:\Windows\panther\setup*.logs
  -   Terminal console will also show any errors. Issues could range from URL change for kube* depenecies to permission issues.
 
#### Post Install: 
Follow quick-start Skipping node setup and until 'Create our First Workload Cluster'
TODO:<Steps to create windows cluster as currenlty requries Cilium metadata label>
