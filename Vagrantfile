
Vagrant.configure("2") do |config|

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  config.vm.define "pcf-vagrant-client" do |clivm|

    clivm.vm.box = "ubuntu/bionic64"
    clivm.vm.provider :virtualbox do |v, override|
      v.memory = 1024
      v.cpus = 1
    end

    # modify based on your host vm folders that you want to map
    clivm.vm.synced_folder "~/Workspace", "/home/vagrant/workspace"

    # install jmespath, needs to be in separate provisioned to ansible_local
    # to ensure it is available when the ansible_local provisioner runs
    config.vm.provision "shell", inline: "sudo apt update; sudo apt install -y python-jmespath"

    # run the pcf-ops-playbook, update the playbook path to match the location in your setup
    clivm.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "/home/vagrant/workspace/work/pcf-ops-playbook/local.yml"
      ansible.verbose = false
    end

  end

end