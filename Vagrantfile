# -*- mode: ruby -*-
# vi: set ft=ruby :

MACHINES = {
  :nginx => {
    :box => 'generic/centos9s',
    :cpus => 2,
    :memory => 1024,
    :networks => [
      [
        :forwarded_port, {
          :guest => 8080,
          :host_ip => '127.0.0.1',
          :host => 8080
        }
      ]
    ],
    :playbook => 'playbook.yml'
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |host_name, host_config|
    config.vm.define host_name do |host|
      host.vm.box = host_config[:box]
      host.vm.host_name = host_name.to_s

      host.vm.provider :virtualbox do |vb|
        vb.cpus = host_config[:cpus]
        vb.memory = host_config[:memory]
      end

      host_config[:networks].each do |network|
        host.vm.network(network[0], **network[1])
      end

      if host_config.key?(:playbook)
        host.vm.provision :ansible do |ansible|
          ansible.playbook = host_config[:playbook]
          ansible.compatibility_mode = '2.0'
          ansible.raw_arguments = ['--diff']
        end
      end
    end
  end
end
