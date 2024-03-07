Vagrant.configure("2") do |config|

config.vm.define "server" do |server|

    server.vm.box = "centos"

    server.vm.network "private_network", ip: "192.168.40.22"

    server.vm.hostname = "borgserver"

    server.vm.provider :virtualbox do |vb|

      vb.customize ["modifyvm", :id, "--memory", "512"]

      vb.customize ["modifyvm", :id, "--cpus", "2"]

      end

    server.vm.provision "shell", inline: <<-SHELL

       mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh

       sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config

       systemctl restart sshd

       timedatectl set-timezone Europe/Moscow

       yum install epel-release -y

       yum install ntp -y && systemctl start ntpd && systemctl enable ntpd

       yum install borgbackup -y     
  
       useradd -m borg

     SHELL

    end

 

    config.vm.define "client" do |client|

      client.vm.box = "centos"

      client.vm.network "private_network", ip: "192.168.40.23"

      client.vm.hostname = "borgclient"

      client.vm.provider :virtualbox do |vb|

      vb.customize ["modifyvm", :id, "--memory", "512"]

      vb.customize ["modifyvm", :id, "--cpus", "2"]

      end


      client.vm.provision "shell", inline: <<-SHELL

       mkdir -p ~root/.ssh; cp ~vagrant/.ssh/auth* ~root/.ssh

       sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config

       systemctl restart sshd

       timedatectl set-timezone Europe/Moscow

       yum install epel-release -y
       
       yum install ntp -y && systemctl start ntpd && systemctl enable ntpd

       yum install borgbackup -y

       useradd -m borg

    SHELL

  end
end