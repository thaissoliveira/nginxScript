# Do que trata este trabalho:
Trabalho de criação e automatização de um script em shell que faça a verificação de funcionamento do Web Server Nginx  em um sistema Ubuntu 20.04. O presente trabalho foi realizado em prol da **Atividade de Linux Devsecops - Outubro** do Programa de Estágio Compass UOL de 2025.

# Instalação do WSL no Windows 

```bash
npm install
```
# Subir Servidor Web Nginx

## Pré-requistos:

1) Acesso ao terminal como usuário root. Rode o seguinte comando para ter acesso como superusuário:
```bahs
sudo su
```
Lembre-se que, se você já tiver criado uma conta root anteriormente, terá de fazer login com sua senha de root.

## Passo 1: Atualizar o Sistema

Antes de instalar qualquer novo pacote, é uma boa prática atualizar a lista de pacotes e os pacotes existentes para a versão mais recente. Abra o terminal e execute:
```bahs
sudo apt update
sudo apt upgrade -y
```
## Passo 2: Instalar o Nginx no Ubuntu 20.04

Para instalar o Nginx, execute o seguinte comando:
```bahs
apt-get install nginx -y
```
Após a instalação, o Nginx será iniciado automaticamente. Para verificar o status do serviço, use o seguinte comando:
```bahs
systemctl status nginx
```
## Passo 3: Verificando o Funcionamento

Para testar o funcionamento do servidor Nginx, podemos realizar algumas ações. Primeiro, vamos verificar se a porta 80 (padrão HTTP) está aguardando conexões, para ver as conexões ativas, vamos usar o comando:

```bahs
netstat -tupan
```
O comando netstat mostra informações sobre as conexões de rede e tabelas de roteamento. O **-t** mostra as conexões tcp/ip. O **-u** mostra as conexões udp. O **-p** mostra o programa que está controlando aquela porta. O **-a** mostra as portas conectadas e as que estão aguardando conexão.

O resultado deste comando será o seguinte:

![image](https://github.com/user-attachments/assets/49c9d0e0-79de-468d-961d-af987bf5c5e6)

Note que a porta 80 já está associada ao Nginx, está ouvindo e esperando (LISTEN).

Outra opção para testar se o Nginx está funcionando é usar o comando systemctl da seguinte forma:

```bahs
systemctl status nginx
```
Se o servidor estiver ativo e funcionando, a seguinte mensagem de status deve aparecer:

![image](https://github.com/user-attachments/assets/7a817699-db12-488a-83e2-954f817981a6)

Para um teste final, precisaremos saber nosso endereço IP, pois testaremos no navegador. Para saber seu endereço, podes usar o comando:

```bahs
ipconfig
```
Caso você esteja em uma máquina virtual, use o enderço público dela.

Por fim, coloque "https://" mais o número do seu endereço ip. A página que deverá aparecer é a seguinte:

![Captura de tela 2024-10-28 110615](https://github.com/user-attachments/assets/9088a3a3-ce6d-46ad-b4b4-8a6e25627b99)

Agora sabemos que o servidor está funcionando!

# Criação de um Script

Para criar um script no Ubuntu 20.04 você precisa acessar o diretório /bin como um administrador e, após isso escolher um editor de texto para escrever um script. Para este exemplo, usarei o editor nano. Escolha um nome para seu arquivo de script e digite nano + o nome do arquivo. Após isso, o arquivo será criado e aparecerá aberto para você escrever nele.

```bahs
cd /bin #comando para acessar o diretório /bin
```

```bahs
nano nome_do_arquivo #comando para criar arquivo de script
```

Esta é a tela após este último comando:

![image](https://github.com/user-attachments/assets/5d5374ea-8f40-4119-8875-5ee9a37506c4)

Perceba que ainda está vazio, você deve escrever seu script neste arquivo.

Para este trabalho, será criado um script que deve atender os seguintes critérios:

1) Criar um script que valide se o serviço do servidor nginx está online e envie o resultado da validação para um diretório de sua escolha;
2) O script deve conter - Data HORA + Nome do serviço + Status + Mensagem personalizada de ONLINE ou Offline;
3) O script deve gerar 2 arquivos de saída: Um para o serviço online e outro para o serviço OFFLINE;
4) Preparar a execução automatizada do script a cada 5 minutos.

Veja como ficou o script:

![Captura de tela 2024-10-28 112825](https://github.com/user-attachments/assets/dca7c37d-6d56-4e58-a6db-dc9f7fe186dc)

Depois de criar o script, é importante verificar no diretório de logs se os arquivos para onde os resultados foram enchaminhados já foram criados.

![image](https://github.com/user-attachments/assets/2f5407c5-b244-4fa8-9d2b-0eaaa59ed350)

Podemos ver que os arquivos de log foram criados.

Agora, vamos configurar o cron para que o script seja executado automaticamente a cada 5 minutos.

Digite o seguinte comando para abrir o arquivo de cron:

```bahs
crontab -u root -e
```
Dentro do arquivo, vamos digitar:

```bahs
*/5 * * * * /bin/valid_nginx
```
Note que colocamos o caminho para o script que criamos dentro do diretório /bin

![image](https://github.com/user-attachments/assets/7f481579-910a-4c68-9695-515dafa666fe)

Pronto, agora nosso script está automatizado! Para vermos os resultados das execuções, basta lermos os arquivos de log.

```bahs
cat debug.log
```
![image](https://github.com/user-attachments/assets/d6d8120d-e091-46d3-906f-62f53e2cdbea)

Sabemos que o script está sendo executado a cada 5 minutos observando as datas e horas registradas no arquivo de log.

