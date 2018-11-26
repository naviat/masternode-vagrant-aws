# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'securerandom'

#Require aws credentials
require 'yaml'
aws_config = YAML::load_file("#{File.dirname(File.expand_path(__FILE__))}/aws-config.yml")

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  
  all_created_nodes = []
  datacenter_and_nodes = {}
  
  aws_config["nodes"].each do |datacenter_info|
    
    #create a hash like {us-west: 2, eu-central: 1}
    datacenter_and_nodes[datacenter_info["region"][0..-3]] = datacenter_info["number_of_nodes"]
    
    datacenter_info["number_of_nodes"].times do |node_number|
      
      node_name = ( aws_config["node_name_prefix"] ? aws_config["node_name_prefix"] + "-" : "" ) + "node-" + datacenter_info["region"] + "-" + node_number.to_s
      
      all_created_nodes << node_name
      
      config.vm.define(node_name) do |node|  
      
          node.vm.box = "ubuntu/trusty64"
          node.vm.hostname = node_name
          # node.vm.network "private_network", ip: "192.168.33." + (node_number + 2).to_s
          
          node.vm.provider :virtualbox do |v|
            v.name = node_name
          end

          node.vm.provider :aws do |aws, override|
            aws.access_key_id = aws_config["key"]
            aws.secret_access_key = aws_config["secret"]
            
            aws.keypair_name = aws_config["keyname"]
        
            aws.ami = datacenter_info["ami"]
            aws.region = datacenter_info["region"]
            aws.instance_type = datacenter_info["instance_type"]
            aws.security_groups = ["default", datacenter_info["security_group"]]

            override.vm.box = "dummy"
            override.ssh.username = "ubuntu"
            override.ssh.private_key_path = aws_config["keypath"]
          end
        end
    
    end
  end  
  # also disable deploying unique ssh keys to each machine
  # which will break ansible in the current config
  # this is not used when using aws anyhow
  config.ssh.insert_key = false
end
