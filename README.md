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

**Instalação do repositório do Zabbix:**
```bash
wget https://repo.zabbix.com/zabbix/7.2/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.2+debian12_all.deb
dpkg -i zabbix-release_latest_7.2+debian12_all.deb
apt update
```
**Instalação do Servidor, FrontEnd e Agente:**
```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```
