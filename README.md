# Projeto de Configuração de Servidor Debian

## Descrição

Este repositório contém as configurações e scripts utilizados para a criação e configuração de um servidor Debian com os seguintes serviços:

- **WireGuard** (VPN)
- **OpenVPN** (VPN)
- **UFW** (Firewall)
- **Samba** (Compartilhamento de Arquivos)
- **Zabbix** (Monitoramento de Servidor)

## Passos Realizados

### 1. **Instalação do Debian com Interface Gráfica**
O sistema Debian foi instalado com interface gráfica para facilitar a configuração e administração dos serviços. A interface gráfica permite que a configuração de redes, firewalls e compartilhamento de arquivos seja feita de forma mais intuitiva.

### 2. **Configuração do WireGuard (VPN)**
O WireGuard foi instalado para configurar uma **VPN** segura e eficiente. O arquivo de configuração `wg0.conf` foi preparado, com a chave privada do servidor e a chave pública do peer, além do endereço de rede configurado.

**Comando de instalação do WireGuard:**
```bash
sudo apt install wireguard -y
```
### 3. **Intalação e Configuração do OpenVPN**

**Comando de instalação do OpenVPN:**
```bash
sudo apt install openvpn easy-rsa -y
```
### 4. **Instalação do Samba**

**Comando de Instalação do Samba:**
```bash
sudo apt install samba samba-common-bin -y
```
### 5. **Instalçao e Configuração do Zabbix**

#### a) Instalar o repositório Zabbix

```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
sudo dpkg -i zabbix-release_latest_7.2+debian12_all.deb
sudo apt update

```

#### b) Instalar o servidor Zabbix, o frontend e o agente
```bash
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent -y
```

#### c) Criar o banco de dados inicial
Certifique-se de que o servidor de banco de dados (MariaDB/MySQL) esteja instalado e funcionando.

Acesse o MySQL como root:
```bash
mysql -u root -p
```
Dentro do prompt do MySQL, execute os seguintes comandos:

```bash
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```
Depois, importe o esquema inicial de dados
```bash
zcat /usr/share/zabbix/sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -u zabbix -p zabbix
```

(Será solicitada a senha do usuário zabbix criada anteriormente.)

Após importar o esquema, desative a configuração log_bin_trust_function_creators:

```bash
mysql -u root -p
```
Dentro do MySQL:
```bash
set global log_bin_trust_function_creators = 0;
quit;
```
### d) Configurar o banco de dados para o servidor Zabbix
Edite o arquivo /etc/zabbix/zabbix_server.conf:
```bash
sudo nano /etc/zabbix/zabbix_server.conf
```
Localize e configure a linha:
```bash
DBPassword=password
```
Salve e feche o arquivo.

Após essas configurações:
Reinicie os serviços do Zabbix e do Apache:
```bash
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2
```
