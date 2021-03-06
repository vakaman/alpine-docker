# Carrega as dependencias
Dir[File.join((Dir.pwd) + "/lib/", "**/*.rb")].each do |f|
      require f
end

# Plugins locais que o projeto requer | Dependencia "dependency-manager" para verificar se os plugins estão instalados
check_plugins ["vagrant-env", "vagrant-disksize", "vagrant-reload", "vagrant-vbguest"]

Vagrant.configure("2") do |config|

    # Seta a não atualização do VBoxGuestAdditions
    config.vbguest.auto_update = false

	# Habilita .env | Requer: "vagrant plugin install vagrant-env" | ENV['ENV_NAME']
	config.env.enable

	# Imagem base | https://app.vagrantup.com/boxes/search
	config.vm.box = "generic/alpine" + ENV['VAGRANT_ALPINE_VERSION']

	# Seta o tamanho do disco | Requer: "vagrant plugin install vagrant-disksize"
	config.disksize.size = ENV['VAGRANT_DISKSIZE_GB'] + "GB"

	# Configura a network
	config.vm.network "private_network", ip: ENV['IP_HOST']

	# Configura Hostname
    config.vm.hostname = ENV['VAGRANT_HOSTNAME']

    # Dependencia "env-to-server" para copiar as variaveis do arquivo .env para os scripts bash
    GLOBAL_ENV = set_env Dir.pwd
    config.vm.provision "shell", inline: GLOBAL_ENV, run: "always"

	# Script de atualização do sistema
 	config.vm.provision "shell", path: "./scripts/updatesystem.sh", privileged: true

 	# Reiniciar servidor apos updatesystem.sh
    config.vm.provision :reload

    # Script para instalações de pacotes
    config.vm.provision "shell", path: "./scripts/firstboot.sh", privileged: true

    # Copia a chave SSH | Dependencia "ssh-copy" para copiar chave ssh para o servidor
    if ENV['SSH_COPY'] == 'true'
       SSH_COPY_PRIVATE_KEY = copy_ssh_keys ENV['SSH_COPY_PRIVATE_KEY']
       SSH_COPY_PUBLIC_KEY = copy_ssh_keys ENV['SSH_COPY_PUBLIC_KEY']
       if SSH_COPY_PRIVATE_KEY != 0
            config.vm.provision "shell", path: "./scripts/sshcopy.sh", :args => [SSH_COPY_PRIVATE_KEY, SSH_COPY_PUBLIC_KEY]
       end
    end

	# Pastas mapeadas
	config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder "./shared", "/home/vagrant/shared"
    config.vm.synced_folder "./shared/html", "/var/www/html"

    config.vm.provider "virtualbox" do |v|
        v.name =  ENV['VAGRANT_HOSTNAME']
        v.customize ["modifyvm", :id, "--memory", ENV['VAGRANT_MEMORY_MB']]
    end

end