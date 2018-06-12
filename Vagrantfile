#
# Ayres.io 
#

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.hostname = "devel"
  config.vm.define "devel"
  #config.ssh.insert_key = false
  #config.vm.network :private_network, ip: "192.168.1.113"
  config.disksize.size = '10GB' # install the plugin!

  #for i in 5432..5460
  #  config.vm.network :forwarded_port, guest: i, host: 10000  + i
  #end

  config.vm.network :forwarded_port, guest: 80, host: 8090
  config.vm.network :forwarded_port, guest: 81, host: 8091
  config.vm.network :forwarded_port, guest: 82, host: 8092

  # https://www.percona.com/blog/2018/02/09/collect-postgresql-metrics-with-percona-monitoring-and-management-pmm/
  config.vm.network :forwarded_port, guest: 8080, host: 8093
  config.vm.network :forwarded_port, guest: 8081, host: 8094
  config.vm.network :forwarded_port, guest: 8082, host: 8095

  # netdata https://github.com/firehol/netdata/wiki/Installation
  # http://this.machine.ip:19999/
  config.vm.network :forwarded_port, guest: 19999, host: 19998

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = ENV['VAGRANT_PLAYBOOK'] || "provisioning/vagrant_playbook.yml" # Default
    #ansible.inventory_path = "provisioning/inventory"     # Default 
    ansible.sudo = true
    #ansible.limit = "all"
    ansible.tags = ENV['VAGRANT_TAGS']
    ansible.skip_tags = ENV['VAGRANT_SKIP_TAGS']
    ansible.extra_vars = JSON.load(ENV['VAGRANT_EXTRA_VARS'])
    ansible.start_at_task = ENV['VAGRANT_START_AT_TASK']

    ansible.vault_password_file = File.expand_path('~/.vagrant_vault')
    ansible.verbose = 'v'
    ansible.extra_vars = {
      ansible_python_interpreter: "/usr/bin/python3",
    }
  end

end

