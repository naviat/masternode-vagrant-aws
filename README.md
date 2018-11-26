# Vagrant + AWS Masternode Setup - WORK IN PROGRESS (May include Ansible in the future)

This project aims to `vagrant up` a multi-node/multi-region Node/Masternode ready-to-use production-grade cluster within Amazon's EC2 cloud. Configure and `vagrant up --provider=aws` to get going.

## Configuration

### AWS side

You need to have AWS setup so that

- there is a `default` security group (you can change other name edit `security_groups` parameter)
- there is a security group that allows inbound traffic to all ports used by Masternode (configured defaults are for Masternode: 30303,30301,22,9160)
- your instances receive a public DNS entry
- there is a key-pair for which you have the private key file available

Note that these settings are region specific in AWS, if you want to deploy to multiple regions you need to do this setup on each region. Also note that in case of multi-region deployments, you need to have *one* key-pair setup in all regions as the Vagrant combination does not support multiple keys for multiple regions.

### Vagrant side

- Install Vagrant (https://vagrantup.com)
- Install the `vagrant-aws` plugin: `vagrant plugin install vagrant-aws`
- Add the `dummy` aws box, the concrete config is done in the `Vagrantfile` and config: `vagrant box add dummy https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box`

Configure your desired cluster topology as well as AWS credentials in the `aws-config.yml.template` and rename it to `aws-config.yml`. All configuration is done is this file.

This has at the moment only been tested under `Ubuntu 16.04.LTS`, so you should choose your AMI accordingly.

## Deploy

`vagrant up --provider=aws` and you are good to go.

## Terminate the whole cluster

`vagrant destroy` (`-f` if you don't want to ack every node.)

## What do you get

- Multi-Region/Multi-Node setup, preconfigured for...
  - AWS Multi-Region usage (EC2-Multi-Region-Snitch)
  - Settings optimized for SSD data storage (make sure to use an appropriate AMI)

## Concerns

## TODO
