# qa
QA tools for OpenStack

## Modules
### Tempest

The OpenStack Integration Test Suite

https://github.com/openstack/tempest

### Patrole

RBAC Integration Tempest Plugin

https://github.com/openstack/patrole

### Neutron Tempest Plugin

Tempest plugin for Neutron project

https://github.com/openstack/neutron-tempest-plugin

### Tungsten

Tempest Integration of Tungsten Fabric (Contrail)

https://github.com/tungstenfabric/tungsten-tempest

P.S.: It contains only RBAC (Patrole) tests now.

### Stestr

Test runner

https://github.com/mtreinish/stestr

## Vagrantfile

Makes it possible to run Devstack in a VM and execute any tests from the 
host.

## Usage

```bash
vagrant up
pipenv install -d
pipenv shell
tempest run --workspace tests --list-tests | wc -l
    3138
```