# Code Ready Containers Cheat Sheet

## Setup 
```
## Setup prerequisites
crc setup

## Check only 
crc setup --check-only

## Start Openshift cluster with default settings memory=9216, cpus=4 disk-size=31
crc start

## Start Openshift cluster with customize settings
crc start --memory=16384 --cpus 6 --disk-size=50

## Start Openshift cluster with custom file location of image pull secret
crc start --pull-secret-file <location-of-file>
```

## Accessing Openshift cluster with OC
```
# Run this command to configure your shell:
eval $(crc oc-env)

## Open console (you can replace crc console with crc dashboard also)
crc console

# Obtain credentials for developer and admin user
crc console --credentials

## Print URL
crc console --url

## Output in json format with ca cert and all user crendentials
crc console --output json

# Login to console using developer
oc login -u developer -p developer https://api.crc.testing:6443

# Login to admin using kubeadmin 
oc login -u kubeadmin -p <admin-password> https://api.crc.testing:6443
```


## Using local container registry

### Install and setup podman in Mac
```
## Use Homebrew to install
brew install podman

## To start the Podman-managed VM
podman machine init
podman machine start

## Verify 
podman info
```

### Podman Cleanup
```
```

Reference: https://podman.io/getting-started/installation

### Access local registry

```
## Check logged in user
oc whoami

## Switch the user if necessary (follow "Accessing Openshift cluster with OC" section)


## set podman env otherwise podman will not able to push image
```
eval $(crc podman-env)
```

## Login to local container registry
podman login -u kubeadmin -p $(oc whoami -t) default-route-openshift-image-registry.apps-crc.testing --tls-verify=false

## Create new project 
oc new-project demo

## Build or pull container image
podman pull quay.io/libpod/alpine

## Tag the image with local registry information
podman tag alpine:latest default-route-openshift-image-registry.apps-crc.testing/demo/alpine:latest

## Push the container image to local registry
podman push default-route-openshift-image-registry.apps-crc.testing/demo/alpine:latest --tls-verify=false

## Get imagestreams and verify that the pushed image is listed:
oc get is

## Enable image lookup in the imagestream:
oc set image-lookup alpine

## Create a pod using the recently pushed image
oc run demo --image=alpine --command -- sleep 600s

## Verify pod is using imagestream
oc describe po demo
```

## Upgrade
[Download the latest release of CodeReady Containers](https://cloud.redhat.com/openshift/create/local)
```
crc delete
crc setup
crc start
```
