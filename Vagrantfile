# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  # Web
  config.vm.define "web" do |apache|

    apache.vm.hostname = "apache.hacklab"

    apache.vm.box = "ubuntu/trusty64"

    apache.vm.network "forwarded_port", guest: 8000, host: 8000

    apache.vm.network "private_network", ip: "192.168.1.10"

    apache.vm.synced_folder ".", "/django-basico-postgree", type: "rsync"
    config.vm.synced_folder ".", "/django-basico-postgree", type: "rsync"



    apache.vm.provider "virtualbox" do |v|
      v.name = "dj"
      # Display the VirtualBox GUI when booting the machine
      v.gui = false

      # Customize the amount of memory on the VM:
      v.memory = "512"
    end

    apache.vm.provision "shell", inline: <<-SHELL
      sudo apt-get install -y python-pip python-dev libpq-dev postgresql postgresql-contrib
      sudo pip install django flake8 psycopg2
      # python3 manage.py migrate
      # python3 manage.py loaddata db.fixture.json

    SHELL

  end
  # End Web

  # DB
  config.vm.define "db" do |mysql|

    mysql.vm.hostname = "db"

    mysql.vm.box = "ubuntu/trusty64"

    mysql.vm.network "forwarded_port", guest: 5432, host: 5432

    mysql.vm.network "private_network", ip: "192.168.1.11"

    mysql.vm.provider "virtualbox" do |vb|
      vb.name = "dj-database"
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false

      # Customize the amount of memory on the VM:
      vb.memory = "512"
    end

    mysql.vm.provision "shell", inline: <<-SHELL
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
