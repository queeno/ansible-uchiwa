machines = {
  'debian'  =>  {
                  'ip'          => '192.168.33.2',
                  'box'         => 'ubuntu/trusty64',
                  'local_port'  => '8080',
                },
  'redhat'  =>  {
                  'ip'          => '192.168.33.3',
                  'box'         => 'bento/centos-7.1',
                  'local_port'  => '8081',
                }
}

Vagrant.configure('2') do |config|
  machines.each do |vm, specs|
    config.vm.define vm do |machine|
      machine.vm.hostname = "#{vm}-standalone"
      machine.vm.network :private_network, :ip => specs['ip']
      machine.vm.network "forwarded_port", guest: 3000, host: specs['local_port']
      machine.vm.box = specs['box']

      machine.vm.provision 'ansible' do |ansible|
        ansible.playbook = 'vagrant/site.yml'
        ansible.groups = {
          'debian' => ['debian-standalone'],
          'redhat' => ['redhat-standalone']
        }
        ansible.limit = "#{vm}"
        ansible.sudo = true
        ansible.host_key_checking = false
      end
    end
  end
end
