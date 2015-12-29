# -*- mode: ruby -*-
# vi: set ft=ruby :

# For OS detection
module OS
    def OS.windows?
        (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end

    def OS.mac?
        (/darwin/ =~ RUBY_PLATFORM) != nil
    end

    def OS.unix?
        !OS.windows?
    end

    def OS.linux?
        OS.unix? and not OS.mac?
    end
end

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |vconfig|
    vconfig.vm.box = "ubuntu/trusty64"
    # vconfig.vm.box_url = ""
    # vconfig.vm.box_check_update = true

    vconfig.vm.define :devbox do |config|
        config.vm.provider :virtualbox do |v|
            # set memory to 1024MB
            v.customize [ "modifyvm", :id, "--memory", "1024" ]
            # debugging
            #v.gui = true
            v.name = "devbox-14-04"
            v.cpus = 1
        end

        config.vm.host_name = "devbox-14-04"
        config.vm.network :private_network, ip: "192.168.56.14"

        if OS.windows?
            # provision on windows host: login on guest vm and provison via ansible locally
            puts "Vagrant launched from windows"
            config.vm.provision :shell, path: "windows.sh"
        elsif OS.mac?
            # provision this way on an old mac (it's the same way like under windows)
            puts "Vagrant launched from mac os"
            config.vm.provision :shell, path: "mac.sh"
        else
            # do it this way on linux systems (provision by ansible via SSH)
            puts "Vagrant launched from linux or unix"
            config.vm.provision :ansible do |ansible|
                ansible.playbook = "ansible/development.yml"
                ansible.inventory_path = "ansible/development"
            end
        end
    end
end
