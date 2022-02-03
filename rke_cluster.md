# How to deploy a RKE cluster on CentOS:

## Step 1 - Install RKE on the VM:

```
wget -O rke https://github.com/rancher/rke/releases/download/v0.3.1/rke_linux-amd64
chmod +x rke
sudo mv rke /usr/local/bin

# Checking Rancher RKE version
rke --version
```

## Step 2 - Docker Installation:

Check which docker version is installed:
`docker --version`

if the version is above 19.03, remove and reinstall the specific relevant version, as the requrirements of RKE.

### 

