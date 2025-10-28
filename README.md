# Bootcamp-Cybersecurity

## Simulando um Ataque de Brute Force de senhas com Medusa e Kali Linux 

#### Objetivo compreender como funciona ateques de força bruta em serviços de FTP, Web, SMB utilizando o kali linux e medusa, documentando todo o processo.

## Ferramentas utilizadas 
- **Oracle VirtualBox** (Software de virtualização das maquina atacante e maquina alvo) 
- **Kali Linux** (maquina atacante)
- **Metasploitable 2 / DVWA** (maquina alvo)
- **Medusa** (ferramenta de brute force)
- **Wordlist** (user.txt, password.txt personalizadas)
- **Nmap / enum4linux** (Enumerção de serviços e portas)

## Parte 1 -Instalação (passo a passo)

#### 1- Vamos começar com a instalações das ferramentas e softwares 

- site oficial: https://oracle.com
- abrir o prompt de comando (Linux) e digitar: "sudo apt install virtualbox-7.1 -y"
  
#### 1.1 Download imagem iso Kali Linux 

- acesse o site oficial: https://www.kali.org/get-kali/
- abrir o prompt de comando (Linux) e digitar: wget https://cdimage.kali.org/current/kali-linux-2025.3-installer-amd64.iso
- instalar a iso no virtualbox.

#### 1.2 Download Metasploitable 2

- acessar site oficial: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
- abrir o prompt de comando (Linux) e digitar: wget https://downloads.sourceforge.net/project/metasploitable/Metasploitable2/metasploitable-linux-2.0.0.zip
- breve exlicação sobre Metasploidtable 2 é uma máquina virtual propositalmente vulnerável para prática de testes de penetração e aprendizado de segurança.
- instalar no VirtualBox.
- 
#### 1.3 Medusa
- Medusa é uma ferramenta de brute force que normalmente vem instalada na distribuição Kali Linux uma recomendação:
- **Atualizar repositórios**
- sudo apt update
- **Atualizar o Medusa**
- sudo apt upgrade medusa

#### 1.4 Wordlist
- será criada com os usuarios e senhas mais utilizados e ou de vazamentos de dados publicos. 

## Parte 2 -configuração de rede(VirtualBox)

#### 2.Iniciando maquinas vituais e configurando a rede (rede interna host-only)
- Ao iniciar o VirtualBox com as maquinas já instaladas (Kali Linux e Metasploitable 2) vamos iniciar a configuração.
- Selecionar uma delas e ir em 'Configurações" um simbolo de engrenagem na parte superior do VirtualBox.
- Irá abrir outra janela, selecionar "Rede" > "Adaptador 1" > "Conectado a:" > mudar a opção "NAT" para (host-only)
- Repetir na outra maquina (abaixo uma imagem de referencia) 
- ![Configuação de rede](https://github.com/user-attachments/assets/cb0b95f9-45fc-4abf-81ab-a5032759bec9)

#### 2.1 iniciando o Metasploitable 
- Iniciar o metasploitable pelo VirtualBox
- usuario = msfadmin
- senha = msfadmin
#### 2.2 iniciando Kali Linux 
- Iniciar Kali Linux pelo virtualBox
- usuario = kali
- senha = kali
- (normalmente a padrão são essas se voce não escolheu algum usuario e senha)

#### 2.3 Confirmar se esta a rede esta configurado
- No metasploitable rodar o comando abaixo para saber o ip:
```bash
# ip a
```
- Vai retornar o ip alvo que usaremos nas proximas etapas.

#### 2.3.1
- abrir o Kali Linux
- ctrl + alt + t (abrir prompt comando/terminal)
- ```bash
  # ping (IP_ALVO) -c 3
  
- deve retornar 3 pacotes como a imagem abaixo:
- ![pingkali](https://github.com/user-attachments/assets/e145aa40-6078-4917-a9d9-5809b28f7e86)
- Tudo certo ate aqui.

 ## Parte 3 -Enumeração de serviços e portas abertas

 #### 3. Nmap
- Começando pelo Nmap para enumerar portas abertas de serviços disponiveis 
- ```bash
  # nmap -sV -p 21,22,80,445,139 (IP_ALVO)

- -sV = Detectar versão de serviços
- -p = especificação de portas
- Retorno esperado na imagem abaixo:
- ![nmapscan](https://github.com/user-attachments/assets/c88982b9-42ce-4666-9502-49de74a9d758)

#### 3.1 FTP (porta 21)
- Vamos simular o primeiro brute force no serviço FTP como da para ver na imagem acima
  ```bash
  # ftp (IP_ALVO)
  
- verificação se esta ativo, se retornar "Connected to (IP_ALVO)" e pedir "Name" e "password" esta ativo.

## 4 -Criando Wordlist (users, password)

- ```bash
  # echo -e 'user\nmsfadmin\nadmin\nroot\ > users.txt
  
- esta criado a wordlist de users mais usados no matasploitable 2 que usaremos logo.
- ```bash
  # echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt

- ![wordlist](https://github.com/user-attachments/assets/952d3837-fd46-4375-8d0b-d1370436e4e2)
- obs: é possivel encontrar wordlists de usuarios e senhas mais usadas e baixa-las.

## 5 -Medusa (brute force user, password)















