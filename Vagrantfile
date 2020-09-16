CPU=2
MEM=2048
DISK="30GB"
LOCAL_DNS="192.168.0.202"

SYNC_HOST="~/Workspace"
SYNC_CLIENT="/home/vagrant/workspace"

GIT_USERNAME="Simon O'Brien"
GIT_EMAIL="simonobrien@vmware.com"

PROVISION_PLAYBOOK="/home/vagrant/workspace/work/tanzu-ops-playbook/local.yml"

PIVNET_API_TOKEN="CHANGEME"

Vagrant.configure("2") do |config|

  config.vagrant.plugins = ["vagrant-disksize"]
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # forward ssh from host, ensure keys are added to ssh-agent via ssh-add
  config.ssh.forward_agent = true

  config.disksize.size = DISK

  config.vm.define "vagrant-workstation" do |vagrant|
    vagrant.vm.box = "ubuntu/bionic64"
    vagrant.vm.provider :virtualbox do |v, override|
      v.memory = MEM
      v.cpus = CPU
    end

    # modify based on your host vm folders that you want to map
    vagrant.vm.synced_folder SYNC_HOST, SYNC_CLIENT

    # ensure DNS is pointing at Untangle
    vagrant.vm.provision "shell" do |shell|
      shell.path = "set-dns.sh"
      shell.args = [LOCAL_DNS]
    end

    # install jmespath, needs to be in separate provisioned to ansible_local
    # to ensure it is available when the ansible_local provisioner runs
    vagrant.vm.provision "shell", inline: "sudo apt update; sudo apt install -y python-jmespath"

    # run the tanzu-ops-playbook
    vagrant.vm.provision "ansible_local" do |ansible|
      ansible.playbook = PROVISION_PLAYBOOK
      ansible.verbose = false
      ansible.extra_vars = {
        pivnet_api_token: PIVNET_API_TOKEN
      }
    end

    # set git username and email
    vagrant.vm.provision "shell" do |shell|
      shell.path = "git-config.sh"
      shell.args = [GIT_USERNAME, GIT_EMAIL]
    end

  end

end
