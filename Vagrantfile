require 'yaml'
require 'fileutils'

unless Vagrant.has_plugin?("vagrant-hostmanager")
  puts 'The "vagrant-hostmanager" plugin is required!  Run "vagrant plugin install vagrant-hosts" and try again.'
  exit!
end

VAGRANTFILE_API_VERSION = 2
hosts_file = File.join(Dir.pwd, "vagrant-hosts.yml")
hosts = YAML.load_file(hosts_file)["hosts"]
# box_name = "centos/7"
# box_name = "debian/jessie64"
box_name = "ubuntu/trusty64"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.ssh.insert_key = false
  config.ssh.username = 'vagrant'
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = true
  hosts.each do |hostname, host|
    config.vm.define hostname do |h|
      h.vm.hostname = host['hostname']
      h.vm.box = box_name
      h.vm.network :private_network, :ip => host['ip']
      unless host['forwards'] == nil
        host['forwards'].each do |f|
          h.vm.network :forwarded_port, guest: f['guest'],
                                        host: f['host'],
                                        auto_correct: false
        end
      end
      h.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", host['mem'], '--cpus', host['cpus']]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
        vb.customize ["modifyvm", :id, "--nictype2", "virtio"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      end
      h.vm.provision "ansible" do |ansible|
        ansible.groups = {
          "sensu_backend" => [ "sensu-server-01" ],
          "sensu_agent" => [ "sensu-agent-01", "sensu-server-01" ]
        }
        ansible.playbook = "./tests/provision.yml"
      end
    end
  end
end