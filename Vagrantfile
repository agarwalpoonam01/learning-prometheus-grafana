$script = <<-'SCRIPT'
echo "Provisioning the VM"
sudo yum update -y
sudo yum install -y telnet bash-completion wget curl vim ntp
swapoff -a
sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo systemctl enable ntpd
sudo systemctl start ntpd
sudo sed -i s/^SELINUX=.*$/SELINUX=disabled/ /etc/selinux/config
sudo timedatectl set-timezone "Asia/Kolkata"
date > /etc/vagrant_provisioned_at
SCRIPT

# ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

Vagrant.configure("2") do |config|
config.vm.provision "shell", inline: $script
config.vm.synced_folder "./", "/home/vagrant/synced_folder", :mount_options => ["dmode=777", "fmode=666"]
  config.vm.define "prom" do |prometheus|
    prometheus.vm.box = "hellodk/centos7"
    prometheus.vm.hostname = 'prometheus'
    prometheus.vm.network :private_network, bridge: "wlp5s0", ip: "192.168.56.50"
    prometheus.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 4096]
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--name", "prom"]
  end
end
end
