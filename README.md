# vagrant-openshift
This is a starter Vagrantfile deploying an Ubuntu sandbox environment containing Docker and OpenShift Origin on Ubuntu.  It's a work in progress, with more to do to enable logging in to the Openshift Web UI for example.  (Probably this is easiest from the guest machine, cf [this link](https://stackoverflow.com/questions/18878117/using-vagrant-to-run-virtual-machines-with-desktop-environment) on a Vagrant Desktop environment.)  

Already as it stands, however, OpenShift Origin cluster is installed and configured for development work.

## Usage

With Virtualbox and Vagrant installed:

```
git clone git@github.com:CodeSolid/vagrant-openshift.git
cd vagrant-openshift
vagrant up
# Wait a bit for machine to provision
vagrant ssh
oc cluster up
```
