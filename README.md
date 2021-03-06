# Alpine Docker | Server Vagrant Box

Está é uma Vagrant Box completa para começar a desenvolver com Docker!

Ela gera uma VM com linux Alpine de 100mb com todos os recursos essenciais instalados (Git, Composer, PHP7, Docker, Docker Compose e utilitarios do unix).

##  Pré-requisitos

* [VirtualBox da Oracle](http://www.virtualbox.org/)
* [Vagrant](http://downloads.vagrantup.com/)

## Subindo o Servidor

 1. Crie um arquivo _.env_ e popule as seguintes variaveis:
 
``` 
#ALL
IP_HOST=192.168.70.7
TIME_ZONE=America/Sao_Paulo

#Vagrantfile
VAGRANT_ALPINE_VERSION=38
VAGRANT_HOSTNAME=alpine-docker
VAGRANT_DISKSIZE_GB=50
VAGRANT_MEMORY_MB=2048

# Versão dos modulos do VirtualBox
# NOTA 01/2020: Versões mais novas desses modulos quebram a sincronização das pastas
VB_GUEST_FIX=true
VB_GUEST_ADD_VERSION=5.1.30-r0
VB_GUEST_MOD_VIRT_VERSION=4.14.167-r0

############
# OPCIONAL #
############

# SSH
# Essa configuração serve para copiar a chave SSH da sua maquina local para o servidor, desta forma você consegue
# usar o git sem usuario e senha e sem ter que cadastrar uma nova chave ssh no repositorio.
# NOTA: Não recomendado usar essa opção em uma maquina de produção, use local.
SSH_COPY=true
SSH_COPY_PRIVATE_KEY="~/.ssh/id_rsa"
SSH_COPY_PUBLIC_KEY="~/.ssh/id_rsa.pub"
 ```
 2. Execute o comando:

> vagrant up

 3. Apos a inntalação, acesse a maquina com:
 
> vagrant ssh
