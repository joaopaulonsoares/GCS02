# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  config.vm.provision "shell", inline: "echo Hello"

  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.hostname="db"

    db.vm.network "forwarded_port", guest: 5432, host: 5432

    db.vm.network "private_network", ip: "192.168.1.11"
    db.vm.synced_folder ".", "/vagrant_data"

    config.vm.provider "virtualbox" do |vb|
      vb.name = "testeUm"
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false

      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end

    # db.vm.provision "shell", inline: <<-SHELL
    #   # apt-get update
    #   sudo apt-get install -y postgresql
    #   sudo su - postgres -c "psql -f /vagrant/db-config.sql"
    #   sudo su - postgres -c 'echo "host all all 192.168.1.10/24 trust" >> /etc/postgresql/9.3/main/pg_hba.conf'
    #   sudo su - postgres -c "echo listen_addresses=\\'*\\' >> /etc/postgresql/9.3/main/postgresql.conf"
    #   sudo service postgresql restart
    # SHELL

  end

  config.vm.define "web" do |web|
    web.vm.box = "ubuntu/trusty64"
    web.vm.hostname="web"

    web.vm.network "forwarded_port", guest: 8000, host: 8000

    web.vm.network "private_network", ip: "192.168.1.10"
    web.vm.synced_folder ".", "/vagrant_data",type:"virtualbox"

    config.vm.provider "virtualbox" do |vm|
      vm.name = "testeDois"
      # Display the VirtualBox GUI when booting the machine
      vm.gui = false

      # Customize the amount of memory on the VM:
      vm.memory = "512"
    end

    # web.vm.provision "shell", inline: <<-SHELL
    #   sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
    #   sudo pip install django flake8 psycopg2
    #   # python3 manage.py migrate
    #   # python3 manage.py loaddata db.fixture.json
    #
    # SHELL

  end

end
