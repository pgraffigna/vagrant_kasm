Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
 
IMAGEN = "geerlingguy/ubuntu2004"

  config.vm.define "kasm" do |t|
    t.vm.box = IMAGEN
    t.vm.hostname = "kasm"
    t.vm.network "public_network"
    t.vm.network "forwarded_port", guest: 443, host: 8443
  
    t.vm.provision "shell", inline: <<-SHELL 
    export KASM="https://kasm-static-content.s3.amazonaws.com/kasm_release_1.9.0.077388.tar.gz"
    export GZ="kasm_release_1.9.0.077388.tar.gz"
    export TAR="kasm_release_1.9.0.077388.tar"
    
    sudo fallocate -l 4g /mnt/4GiB.swap
    sudo chmod 600 /mnt/4GiB.swap
    sudo mkswap /mnt/4GiB.swap
    sudo swapon /mnt/4GiB.swap
    echo '/mnt/4GiB.swap swap swap defaults 0 0' | sudo tee -a /etc/fstab

    cd /tmp && wget -q --show-progress --progress=bar:force 2>&1 $KASM 
    gunzip /tmp/$GZ
    tar -xf /tmp/$TAR 
    SHELL
  
    t.vm.post_up_message = "Falta correr el instalador..sudo bash /tmp/kasm_release/install.sh"
  end

  config.vm.provider "virtualbox" do |vb|
    vb.name = "kasm"
    vb.memory = "4096"
    vb.cpus = "2"
  end  
end


