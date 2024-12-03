Instal·lació de Samba

## Actualitzar el sistema
```bash
sudo apt update sudo apt upgrade
```
## Instal·lar Samba i verificar la instal·lació
```bash
sudo apt install samba
samba --version
```
## Crear el directori compartit
```bash
sudo mkdir /mnt/sdc1/sambasahre
sudo chmod -R 777 /mnt/sdc1/sambasahre
```
## Configurar Samba
```bash
sudo nano /etc/samba/smb.conf
```
Contingut del document smb.conf
```txt
[sambashare]
path = /mnt/sdc1/sambasahre
browsable = yes
read only = no
guest ok = no
```
## Reiniciar el servei
```bash
sudo systemctl restart smbd
sudo systemctl status smbd
```
## Crear Usuari
```bash
sudo smbpasswd -a yourusername
sudo systemctl restart smbd
```

## Configurar Firewall
```bash
sudo iptables -L
sudo iptables -A INPUT -p tcp --dport 139 -j ACCEP
sudo iptables -A INPUT -p tcp --dport 445 -j ACCEP
sudo iptables-save > /etc/iptables/rules.v4
```