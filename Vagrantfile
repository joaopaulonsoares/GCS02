# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  # Web
  config.vm.define "web" do |web|

    web.vm.hostname = "web"
    web.vm.box = "ubuntu/trusty64"
    web.vm.network "forwarded_port", guest: 8000, host: 8000
    web.vm.network "private_network", ip: "192.168.1.10"

    web.vm.synced_folder ".", "/vagrant", type: "rsync"
    config.vm.synced_folder ".", "/vagrant", type: "rsync"

    web.vm.provider "virtualbox" do |v|
      v.name = "dj"
      # Display the VirtualBox GUI when booting the machine
      v.gui = false

      # Customize the amount of memory on the VM:
      v.memory = "512"
    end

    web.vm.provision "shell", inline: <<-SHELL
      # apt-get update
      sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      sudo pip install django flake8 psycopg2

    SHELL

  end
  # End Web

  # DB
  config.vm.define "db" do |db|

    db.vm.hostname = "db"
    db.vm.box = "ubuntu/trusty64"
    db.vm.network "forwarded_port", guest: 5432, host: 5432
    db.vm.network "private_network", ip: "192.168.1.11"

    db.vm.provider "virtualbox" do |vb|
      vb.name = "dj-database"
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false

      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end

    db.vm.provision "shell", inline: <<-SHELL
      # apt-get update
      sudo apt-get install -y postgresql
      sudo su - postgres -c "psql -f /vagrant/db-config.sql"
      sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
      sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
      sudo service postgresql restart

    SHELL

  end
  # End DB

end
