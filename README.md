# Course Alura - Vargrant

- Install Virtual Box 6  
   <https://www.linuxtechi.com/install-virtualbox-6-centos-8-rhel-8/>

  - Enable VirtualBox and EPEL Repository

    - Add Repository
      dnf config-manager --add-repo=<https://download.virtualbox.org/virtualbox/rpm/el/virtualbox.repo>

    - Use below rpm command to import Oracle VirtualBox Public Key
      rpm --import <https://www.virtualbox.org/download/oracle_vbox.asc>

    - Enable EPEL repo using following dnf command
      dnf install <https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm> -y

  - Install VirtualBox Build tools and dependencies

    - Run the following command to install all VirtualBox build tools and dependencies
      dnf install binutils kernel-devel kernel-headers libgomp make patch gcc glibc-headers glibc-devel dkms -y

  - Install VirtualBox 6.0 on CentOS 8 / RHEL 8

    - If wish to list available versions of VirtualBox before installing it , then execute the following dnf command
      dnf search virtualbox

    - Let’s install latest version of VirtualBox 6.0 using following dnf command
      dnf install VirtualBox-6.0 -y

    - If any local user want to attach usb device to VirtualBox VMs then he/she should be part “vboxusers ” group, use the beneath usermod command to add local user to “vboxusers” group.
      usermod -aG vboxusers user

    - Enable AMD-V in host or provider(vmware, for example)

    - Disable Hyper-V in case windows
      Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All

  - Install VirtualBox 6.0 Extension Pack
    cd Downloads/
    wget <https://download.virtualbox.org/virtualbox/6.0.0/Oracle_VM_VirtualBox_Extension_Pack-6.0.0.vbox-extpack>

  - Access VirtualBox on CentOS 8 / RHEL 8

    - Configure home with permissions
      chown user:user /home/user
    - Configure .Xauthority
      <https://www.osradar.com/configure-x11-forwarding-in-centos-rhel-6-7-8-and-fedora-28-29/>

    init 6
    virtualbox
    ctr+z
    bg virtualbox

- Install Vargrant in Rhel Centos 8

  - Download Vargrant
    <https://releases.hashicorp.com/vagrant/2.2.3/vagrant_2.2.3_x86_64.rpm>
    sudo wget <https://releases.hashicorp.com/vagrant/2.2.3/vagrant_2.2.3_x86_64.rpm>
  - Install
    sudo yum localinstall vagrant_2.2.3_x86_64.rpm -y
    vagrant ––version

  - Install plugins
    vagrant plugin install vagrant-vsphere

- Init vargrant Project box precise
  cd ~
  mkdir vagrabt
  cd vagrabt
  mkdir dev
  cd dev
  vagrant init hashicorp/precise64
  vagrant up

- Validate Vagrantfile
  vagrant validate

- Vagrant ssh

  - Connect host
    vagrant ssh or
    ssh -p 2222 vagrant@127.0.0.1 / pass:vagrant
  - View configs
    vagrant ssh-config

- Power off VM
  vagrant halt

- Init vargrant Project box Ubuntu bionic and nginx
  vim Vagrantfile
  add content in Vagrantfile:

  Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  end

  vagrant up
  vagrant status
  vagrant ssh
  sudo apt-get update -y
  sudo apt-get install -y nginx
  curl <http://localhost>

- Configure Forwarded Ports
  vim Vagrantfile
  add line:
  config.vm.network "forwarded_port", guest: 80, host: 8080
  vagrant halt
  vagrant up
  vagrant ssh
  curl <http://localhost:8089>

- Configure Private Network
  vim Vagrantfile

  - Static,add line:
    config.vm.network "private_network", ip: "192.168.1.21"
  - DHCP,add line:
    config.vm.network "private_network", type: "dhcp"

  vagrant reload or vagrant halt, vagrant up

- Configure Public Network
  vim Vagrantfile

  - Static,add line:
    config.vm.network "public_network" , ip: "192.168.1.21"
  - DHCP,add line:
    config.vm.network "public_network"

  vagrant reload or vagrant halt, vagrant up

- Delete VM
  vagrant destroy -f

- SSH

  - list configs ssh
    vagrant ssh-config
  - connect with private key
    ssh -i /home/user/vagrant/dev/bionic/.vagrant/machines/default/virtualbox/private_key vagrant@192.168.1.132
  - connect with public key
    ssh -i id_rsa vagrant@192.168.1.132

- PROVISIONING

  - Shell

    - Hello World
      config.vm.provision "shell", inline: "echo Hello, World"
      config.vm.provision "shell", inline: "echo Hello, World >>hello.txt"
    - Set SSH
      config.vm.provision "shell",
      inline: "cat /configs/id_rsa.pub >> .ssh/authorized_keys"

  - Execute Providers(Case modify Vagrantfile with new code)
    vagrant provision

- MOUNTS \ SHARED FOLDERS

  - Add mounts
    config.vm.synced_folder "src/", "/srv/website"
    config.vm.synced_folder "./configs/", "/configs"
  - Disable Mounts
