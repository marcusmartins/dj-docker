# -*- mode: ruby -*-
# vi: set ft=ruby :

# Override these values with a local config defined in vagrant.yaml
CONF = {
    'box_name' => 'ubuntu/trusty64',
    'box_url' => 'https://cloud-images.ubuntu.com/vagrant/trusty/20140820/trusty-server-cloudimg-amd64-vagrant-disk1.box',
    'allocate_memory' => 512,
    'num_cpus' => 2,
    'ip_address' => '192.168.33.80',
}

Vagrant.configure("2") do |config|

    # Every Vagrant virtual environment requires a box to build off of.
    config.vm.box = CONF['box_name']

    # The url from where the 'config.vm.box' box will be fetched if it
    # doesn't already exist on the user's system.
    config.vm.box_url = CONF['box_url']

    # SSH settings
    config.ssh.forward_agent = true

    config.vm.network :private_network, :ip => CONF['ip_address']
    config.vm.network :forwarded_port, guest: 4243, host: 4243

    config.vm.synced_folder ".", "/vagrant", type: "nfs"

    config.vm.provider :virtualbox do |vb, override|
        # Use VBoxManage to customize the VM. For example to change memory:
        memory = CONF['allocate_memory'].to_s()
        vb.customize ["modifyvm", :id, "--memory", memory]

        # modify number of cpus
        n_cpus = CONF['num_cpus']
        if ! n_cpus.nil?
         vb.customize ["modifyvm", :id, "--cpus", n_cpus.to_s()]
        end
    end

    config.vm.provision "ansible" do |ansible|
        # Set the verbosity of the ansible log - 'v', 'vv' or 'vvv'. 'v' is the default
        ansible.verbose = "v"
        ansible.playbook = "provisioning/playbook.yml"
        ansible.inventory_path = "provisioning/inventory"
        ansible.sudo = true
    end

    # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
    config.vm.define :docker do |docker|
        docker.vm.hostname = "docker"
    end

end
