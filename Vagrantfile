#
# roborio-skel
#
Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provider 'virtualbox' do |v|
    v.memory = 2048
    v.cpus = 3
  end

  config.vm.network 'public_network'

  # We provision using ansible_local, bootstrap our ansible roles by
  # downloading them from our repo.
  config.vm.provision 'shell', inline: <<-SCRIPT
  GIT=/usr/bin/git
  ANSIBLE_REPO=https://github.com/strykeforce/ansible.git
  ANSIBLE_VERSION=v16.0.1
  ANSIBLE_DIR=/opt/ansible

  [[ ! -x $GIT ]] && apt-get install -y git

  [[ ! -d $ANSIBLE_DIR ]] && $GIT clone -q $ANSIBLE_REPO $ANSIBLE_DIR

  cd $ANSIBLE_DIR
  $GIT fetch -q
  $GIT checkout -q $ANSIBLE_VERSION
  SCRIPT

  config.vm.provision 'ansible_local' do |ansible|
    ansible.provisioning_path = '/opt/ansible'
    ansible.playbook = 'vagrant.yml'
    ansible.groups = {
      'roborio' => ['default']
    }
    ansible.sudo = true
    ansible.verbose = false
    ansible.raw_arguments = ['--vault-password-file=/vagrant/.vault_pw']
  end
end
