# Bootcamp-Cybersecurity

## Simulando um Ataque de Brute Force de senhas com Medusa e Kali Linux 

#### Objetivo compreender como funciona ataques de força bruta em serviços de FTP, Web, SMB utilizando o kali linux e medusa, documentando todo o processo.

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
- abrir o prompt de comando (Linux)
- ```bash
   sudo apt install virtualbox-7.1 -y
  
#### 1.1 Download imagem iso Kali Linux 

- acesse o site oficial: https://www.kali.org/get-kali/
- abrir o prompt de comando (Linux)
- ```bash
   wget https://cdimage.kali.org/current/kali-linux-2025.3-installer-amd64.iso
- instalar a iso no virtualbox.

#### 1.2 Download Metasploitable 2

- acessar site oficial: https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
- abrir o prompt de comando (Linux)
- ```bash
   wget https://downloads.sourceforge.net/project/metasploitable/Metasploitable2/metasploitable-linux-2.0.0.zip
  
- breve explicação sobre Metasploidtable 2 é uma máquina virtual propositalmente vulnerável para prática de testes de penetração e aprendizado de segurança.
- instalar no VirtualBox.
- 
#### 1.3 Medusa
- Medusa é uma ferramenta de brute force que normalmente vem instalada na distribuição Kali Linux uma recomendação:
- **Atualizar repositórios**
- ```bash
   sudo apt update
  
- **Atualizar o Medusa**
- ```bash
  sudo apt upgrade medusa

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
 ip a
```
- Vai retornar o ip alvo que usaremos nas proximas etapas.

#### 2.3.1
- abrir o Kali Linux
- ctrl + alt + t (abrir prompt comando/terminal)
- Esse comando reliza um ping para ver se tem conectividade com o ip fornecido.
  
- ```bash
   ping IP_ALVO -c 3
  
- deve retornar 3 pacotes como a imagem abaixo:
- ![pingkali](https://github.com/user-attachments/assets/e145aa40-6078-4917-a9d9-5809b28f7e86)
- Tudo certo ate aqui.

 ## Parte 3 -Enumeração de serviços e portas abertas

 #### 3. Nmap
- Começando pelo Nmap para enumerar portas abertas de serviços disponiveis

- ```bash
   nmap -sV -p 21,22,80,445,139 IP_ALVO

- -sV = Detectar versão de serviços
- -p = especificação de portas
- Retorno esperado na imagem abaixo:
- ![nmapscan](https://github.com/user-attachments/assets/c88982b9-42ce-4666-9502-49de74a9d758)

#### 3.1 FTP (porta 21)

- Vamos simular o primeiro brute force no serviço FTP como da para ver na imagem acima
- Esse comando e para teste se esta ativo e para login com serviço FTP.
  
  ```bash
   ftp IP_ALVO
  
- verificação se esta ativo, se retornar "Connected to (IP_ALVO)" e pedir "Name" e "Password" esta ativo.

## 4 -Criando Wordlist (users, password)

- Esses comados irá criar as pasta com as wordlist que usaremos para realiar o brute force.

- ```bash
   echo -e 'user\nmsfadmin\nadmin\nroot\ > users.txt
  
- esta criado a wordlist de users mais usados no matasploitable 2 que usaremos logo.
- ```bash
   echo -e '123456\npassword\nqwerty\nmsfadmin' > pass.txt

- ![wordlist](https://github.com/user-attachments/assets/952d3837-fd46-4375-8d0b-d1370436e4e2)
- obs: é possivel encontrar wordlists de usuarios e senhas mais usadas e baixa-las.

## 5 -Medusa (brute force - FTP)
- Com as etapas todas prontas e com resutados esperados vamso realizar primeira tentativa de brute force.

#### 5.1 executando Medusa

- Na sua maquina atacante rodar esse comando abaixo:
- Esse comando vai realizar testes com de usuarios e senhas fornecidos pelas wordlists.
- 
- ```bash
    medusa -h IP_ALVO -U wordlist_usuarios -P wordlist_senhas -M ftp -t 6
- -h > maquina alvo
- -U > usuarios
- -P > senha
- -M > o serviço que deseja atacar
- -t > tentativas em paralelo (como no comando acima é 6, será 6 threads simultâneas)

- o retorno esperado é algo como a imagem abaixo:
  
-![ftpmedusa](https://github.com/user-attachments/assets/fe424ae8-1939-4b98-8fab-a6d7317377a6)

- Procurar por um "SUCCESS" e testar o usuario e senha no **FTP** como nos fez na etapas **3.1**.

- ![loginftp](https://github.com/user-attachments/assets/69216f29-3fa1-4b21-8495-5d1c7fd27c34)

## 6 -Medusa (brute force - Sistema Web "DVWA") 

- Ataque de forca bruta em formularios de login em sistemas web

#### 6.1 DVWA
- Abrir o navegador e na barra de url digitar;

- ```bash
    IP_ALVO/dvwa/login.php

- Nessa etapa o usuario precisa de um conhecimento previo porque vamos ter que filtrar os parametros que o sistema wb esta usando
- Os parametros que aplicação esta usando são:
- username, password, login

#### 6.2 Executado Medusa DVWA
- Abre o terminal e digitar esse comando:

- ```bash
  medusa -h IP_ALVO -U wordlist_usuarios -P wordlist_senhas -M http -m FORM:'/dvwa/login.php:username=^USER^&password=^PASS^:F=Username and/or password incorrect' -t 6

- Como resposta para nossa simulação com DVWA será retornado:
- user: admin
- password: password
- Agora com resultado é só ir na pagina que abrimos na etapa **6.1** e você tem acesso ao site.

## 7 - SMB + password spraying
- Vamos simular um ataque no serviço em funcionamento SMB, começando pea enumeração.

#### 7.1 Enumeração com enum4linux 
- Abre o terminal e digitar esse comando:
- ```bash
  enunm4inux -a IP_ALVO | tree enum4_output.txt
  
- -a > enumeração
- tree > salvar a saida do comando em um arquivo .txt
- Abrindo o arquivo .txt vamos ter acesso aos possiveis 'user' que são usados para ter acesso ao serviço, essa é nossa base de usuarios.
- ![userSMB](https://github.com/user-attachments/assets/427c205e-3cdc-4ad1-a5b9-49279347cef1)

- Agora precisamos criar uma wordlist com esses usuarios e de senhas como ja fizemos na etapa **4**
- Recomendado criar a wordlist de senhas com base na politicas de senhas do alvo

#### 7.2 password spraying
- Ataque com medusa, com terminal aberto digite:
- ```bash
  medusa -h IP_ALVO -U wordlist_users -P wordlist_password -M smbnt -t 2 -T 50

- ![smpFound](https://github.com/user-attachments/assets/16f3110c-c864-4730-86ed-0bc9013462f4)

- A mensagem 'ACCONT FOUND' significa que conseguimos um acesso.
  
#### 7.3 smbclient
- Para validar se conseguimos os usuarios e senhas corretos vamos executar o smbclient.
- No terminar:
- ```bash
  smbclient -L //IP_ALVO -U usuario

- Após ira pedir a senha, digite a senha e o resultado esperado é algo assim:

- ![smblogin](https://github.com/user-attachments/assets/c76b0007-9ad8-47b2-bc34-d52991d5a2c5)

## 8 - Medidas de mitigação 
- **Checklist basico**
- Habilitar MFA para contas críticas.
- Substituir FTP por SFTP/FTPS desabilitar FTP anônimo.
- Configurar bloqueio/atraso progressivo e fail2ban.
- Aplicar rate limiting e WAF na camada web
- Desabilitar SMBv1 e exigir NTLMv2/Kerberos
- Monitorar e configurar alertas para tentativas de login em massa
- Revisar contas administrativas e remover privilégios desnecessários

## 9 - Documentação das Ferramentas utilizadas 
- Kali Linux > https://www.kali.org/docs/
- VirtualBox > https://www.virtualbox.org/wiki/Documentation
- Metasploitable 2 > https://docs.rapid7.com/metasploit/metasploitable-2/
- DVWA > https://www.studocu.com/in/document/dr-apj-abdul-kalam-technical-university/btech/dvwa-official-documentation-v1/98470062
- Nmap > https://nmap.org/docs.html
- Medusa > https://docs.medusajs.com/

















