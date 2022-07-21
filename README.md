# Uol

## Atividade: 6 - individual.
+ Subir uma segunda VM (seguindo as mesmas regras da atividade anterior);
+ Mudar a VLAN das 2 para BRIGDE e fazer os ajustes necessarios;
+ Criar uma apresentação de slides com 1 slide para cada um dos topicos:
  - O que faz o comando chmod 777 arquivo. sh?
  - Explique a diferença entre: "yum install -y docker" e "yum install docker";
  - O que é o MAC ADDRESS?
  - Qual o comando usado para manipular discos no linux?
+ Configurar a relação de confiança entre as duas VMs;
+ Fazer o versionamento da atividade;
+ Fazer a documentação explicando o processo de instalação do Linux;

## Comandos Utilizados:

### GIT
+ 	sudo yum -y install git (instalação do git)
  - Configuração do git para definir nome e email:
    - git config --global user.name "Nome"
    - git config --global user.email "email@gmail.com"
  - Configurando Chave ssh no linux:
    - Gerar um chave ssh:
      - ssh-keygen -t ed25519 -C "email@gmail.com"
    - Adicionar sua chave SSH ao ssh-agent:
      - eval "$(ssh-agent -s)"
      - ssh-add ~/.ssh/id_ed25519
   - Adicionar uma nova chave SSH à sua conta do GitHub:
	    - cat ~/.ssh/id_ed25519.pub	(mostra a sua chave ssh publica, copia ela)
	    - https://github.com/settings/keys (cole a chave ssh aqui)
	    - ssh -T git@github.com (testa a conexão com o github)
   - Clonando o repositorio Servidor(Exemplo)
	    - git clone git@github.com:Usuario/projeto.git
### Bloquear acesso do root via SSH:
  - Edite o arquivo sshd_config com o comando:
    - sudo vi /etc/ssh/sshd_config
	  - 43 + shift + g => vai para a linha especifica do root
	  - Troque o yes (na frente do PermitRootLogin), para no;
	  - Feche o arquivo salvando com o comando:
      - esc + : + x + enter
	  - Reinicie o serviço do SSHD com o comando:
      - sudo systemctl restart sshd
    - Use o proximo comando para copiar o arquivo editado para a pasta versionada:
	    - sudo cp /etc/ssh/sshd_config /home/usuario/projeto 
### Configurando o IP fixo:
  - Ierei usar o nmtui:
    - Utlize o seguinte comando para iniciar o programa:
      - sudo nmtui
        - Primeiro entre em: Edit a connection.
        - Selecione a placa de rede: enp0s3.
        - Implemente as seguintes configurações:
          - IPv4 CONFIGURATION <Manual>                               
          - Addresses 192.168.100.56/24              
          - Gateway 192.168.100.1
          - DNS servers 8.8.8.8  
     - Confirme as modificações e volte ao menu principal e entre na opção: Set system hostname
        - Apage o hostename e coloque: servidor
        
        - Use o proximo comando para copiar o arquivo editado para a pasta versionada:
          - cp /etc/sysconfig/network-scripts/ifcfg-enp0s3 /home/usuario/projeto 
          - sudo cp /etc/hostname /home/usuario/projeto
          
### Configurando o arquivo host no linux servidor:
  - Utilize o seguinte comando para iniciar:
    - sudo vi /etc/hosts
		    - i + end + seta para baixo + enter 
	- adicione as linhas:
	  - 192.168.100.56 rabbitlabmqcontainerdocker
	  - 192.168.100.57 cliente
  - Use o proximo comando para copiar o arquivo editado para a pasta versionada:
    - cp /etc/hosts /home/usuario/projeto  

### docker:
  - Comandos para instalação do Docker:
	  - sudo yum install -y yum-utils (instala o yum-utils)
	  - sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo (adiciona o repositorio)
	  - sudo yum install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin --nobest --allowerasing (instala o Docker e suas ferramentas)
	  - sudo systemctl start docker (inicia o docker)
	  - sudo groupadd docker (cria o grupo docker)
	  - sudo usermod -aG docker $USER (adiciona o usuário atual ao grupo docker)
	  - sudo systemctl enable docker.service (deixa o docker rodandando sempre)
	  - sudo systemctl enable containerd.service (deixa os containers rodandando sempre)
    - sudo reboot (reseta a máquina para aceitar todas as modificções)

### Download e execução de container RABBITMQ:
  - docker pull rabbitmq (baixa a imagem do rabbit)
	- docker run -d --hostname my-rabbit --name usuario rabbitmq:3 (roda a imagem rabbit em backgroud)
	- docker update --restart always usuario (sempre reinica o container junto com a VM)


## VM Cliente:

### Configurações adicionais:
  - configurar o host no linux cliente:
	  - sudo vi /etc/hosts
		- i + end + seta para baixo + enter 
	  - adicione as linhas:
		  - 192.168.100.56 rabbitlabmqcontainerdocker
	  - Para salvar:
	    - esc + : + x + enter
      
  - Estabelecer relação de confiança entre máquinas via SSH:
    - Cria a chave:	
      - ssh-keygen -t rsa -f ~/.ssh/id_rsa
	  - Copia e envia a chave para a VM Servidor: 	
      - ssh-copy-id 192.168.100.56
 ### Configurando o IP fixo:
  - Ierei usar o nmtui:
    - Utlize o seguinte comando para iniciar o programa:
      - sudo nmtui
        - Primeiro entre em: Edit a connection.
        - Selecione a placa de rede: enp0s3.
        - Implemente as seguintes configurações:
          - IPv4 CONFIGURATION <Manual>                               
          - Addresses 192.168.100.57/24              
          - Gateway 192.168.100.1
          - DNS servers 8.8.8.8  
     - Confirme as modificações e volte ao menu principal e entre na opção: Set system hostname
        - Apage o hostename e coloque: cliente
	- sudo reboot (reseta a máquina para aceitar todas as modificções) 
