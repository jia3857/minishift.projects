# === Install Necessary Packages ===

## Optional, clean up previous installation
```bash

$ minishift delete --force --clear-cache
$ brew uninstall docker-machine-driver-xhyve # if hypervision xhyve installed
$ sudo rm -rf /usr/local/bin/docker-machine-driver-xhyve)
$ sudo rm -rf ~/.docker ~/.kube ~/.minishift ~/.minikube
```

## Install minishift with Homebrew on mac
```bash
$ brew cask install minishift         
$ brew cask install --force minishift # force reinstall
```

# === Set up Environment Variables ===
```bash
export MINISHIFT_HOME=${HOME}/minishift.projects/cml-pvc;
export PATH=$MINISHIFT_HOME/bin:$PATH
export KUBECONFIG=$MINISHIFT_HOME/.kube/config
export KUBE_EDITOR="code -w"

$ mkdir -p ${MINISHIFT_HOME}/bin
$ cd $MINISHIFT_HOME/bin
$ curl -qsOL https://github.com/minishift/minishift/releases/download/v1.34.3/minishift-1.34.3-darwin-amd64.tgz
$ tar xfz *.tgz -C .

$ minishift config set vm-driver virtualbox # Use VirtualBox Permanently
$ minishift config set memory 4096          # Set memory 4096 MB for profile
$ minishift config set --global memory 8192 # Set memory 8192 MB globally
$ minishift config view
$ minishift config view --global

```

## create profile "cml-pvc"

```bash
$ source <(minishift completion bash) # activate bash completion
$ minishift profile list
- minishift	Does Not Exist

$ minishift profile set cml-pvc
$ minishift profile list
- cml-pvc Does Not Exist    (Active)
- minishift	Does Not Exist

$ minishift config set memory 8GB
No Minishift instance exists. New 'memory' setting will be applied on next 'minishift start'
$ minishift config set cpus 4
No Minishift instance exists. New 'cpus' setting will be applied on next 'minishift start'

```

## Start minishift with hypervisor virtualbox

```bash
$ minishift start --vm-driver virtualbox
-- Starting profile 'cml-pvc'
-- Check if deprecated options are used ... OK
-- Checking if https://github.com is reachable ... OK
-- Checking if requested OpenShift version 'v3.11.0' is valid ... OK
-- Checking if requested OpenShift version 'v3.11.0' is supported ... OK
-- Checking if requested hypervisor 'xhyve' is supported on this platform ... OK
-- Checking if xhyve driver is installed ...
   Driver is available at /usr/local/bin/docker-machine-driver-xhyve
   Checking for setuid bit ... OK
-- Checking the ISO URL ... OK
-- Checking if provided oc flags are supported ... OK
-- Starting the OpenShift cluster using 'xhyve' hypervisor ...
-- Minishift VM will be configured with ...
   Memory:    8 GB
   vCPUs :    4
   Disk size: 20 GB
-- Starting Minishift VM ..........................
$ minishift profile list
- cml-pvc	Running		(Active)
- minishift	Does Not Exist

$ minishift ip --profile minishift
Running this command requires an existing 'minishift' VM, but no VM is defined.
$ minishift ip --profile cml-pvc
192.168.99.108

$ minishift oc-env
export PATH="/home/john/.minishift/cache/oc/v3.11.0:$PATH"
# Run this command to configure your shell:
$ eval $(minishift oc-env)
$ source <(oc completion bash)

OpenShift server started.

The server is accessible via web console at:
    https://192.168.99.109:8443/console

You are logged in as:
    User:     developer
    Password: <any value>

To login as administrator:
    oc login -u system:admin


-- Exporting of OpenShift images is occuring in background process with pid 97913.

## Navigate Openshift UI

```bash
$ minishift console --url
https://192.168.99.108:8443/console
$ minishift console
Opening the OpenShift Web console in the default browser...

```