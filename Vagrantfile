# -*- mode: ruby -*-
# vi: set ft=ruby :

vagrant_cache_server = "172.23.238.253"


# Detect the current OS. node on windows needs some symlink magic to work.
module OS
  def OS.windows?
    (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
  end
end

Vagrant.configure(2) do |config|

  config.vm.box = "stackroute/barebones-node"
  config.vm.box_url = "http://#{vagrant_cache_server}/vagrant/boxes/stackroute-barebones-node.box"
  config.vm.hostname = 'stackroute-node'

  # increse the memory to 1GB
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  
  
  # Map the guest os port 3000 to host os port 3000, the default port of
  # express.js is 3000
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  if OS.windows?
    # enable symlinks between the host/gust filesystems
    config.vm.provider "virtualbox" do |vb|
      vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
    end

    # Setup the softlinks
    config.vm.provision :puppet do |puppet|
      puppet.manifest_file = "windows.pp"
    end

  end
end
