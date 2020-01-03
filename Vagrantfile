Vagrant.configure('2') do |config|
    # Disable /vagrant synced folder
    config.vm.synced_folder '.', '/vagrant', disabled: true

    config.vm.define :centos7 do |centos|
        centos.vm.box = 'bento/centos-7'
    end
    config.vm.define :centos8 do |centos|
        centos.vm.box = 'bento/centos-8'
    end
    config.vm.define :ubuntu18 do |ubuntu|
        ubuntu.vm.box = 'bento/ubuntu-18.04'
    end

end
