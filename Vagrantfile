Vagrant.configure("2") do |config|

  config.vm.define "master01" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname = "master01"
  end

  config.vm.define "node01" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname = "node01"
  end

  config.vm.define "node02" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname = "node02"
  end

  config.vm.provision "shell", inline: <<-SHELL
      yum -y install lvm2 yum-utils device-mapper-persistent-data
      yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      yum update
      yum -y install docker-ce
      systemctl enable docker && systemctl start docker && systemctl status docker
      usermod -aG docker vagrant
    SHELL

#only root users will have access to docker.
#we can verify by running the command
#"ls -l /var/run/docker.sock"
#add the user to the docker group for access.
#usermod -aG docker USERNAME
#now login as user and run the docker commands.
end
