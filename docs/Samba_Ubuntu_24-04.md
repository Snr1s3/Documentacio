# Instal·lació i configuració de Samba a Ubuntu 24.04

## 1. Actualitzar el sistema
```bash
sudo apt update
sudo apt upgrade
```

## 2. Instal·lar Samba i verificar la instal·lació
```bash
sudo apt install samba
samba --version
```

## 3. Crear el directori compartit
```bash
sudo mkdir -p /mnt/sdc1/sambashare
sudo chmod -R 777 /mnt/sdc1/sambashare
```

## 4. Configurar Samba

Edita el fitxer de configuració:
```bash
sudo nano /etc/samba/smb.conf
```

Afegeix al final del fitxer:
```ini
[sambashare]
path = /mnt/sdc1/sambashare
browsable = yes
read only = no
guest ok = no
```

## 5. Reiniciar el servei Samba
```bash
sudo systemctl restart smbd
sudo systemctl status smbd
```

## 6. Crear un usuari Samba
```bash
sudo smbpasswd -a yourusername
sudo systemctl restart smbd
```

## Configurar el Firewall
Consulta la [guia de configuració d'iptables](Iptables_Ubuntu_24-04.md) per permetre els ports necessaris per Samba (139 i 445).

```bash
# Exemple ràpid:
sudo iptables -A INPUT -p tcp --dport 139 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 445 -j ACCEPT
sudo iptables-save | sudo tee /etc/iptables/rules.v4
```

---

**Notes:**
- Assegura't que el directori i el nom del recurs compartit siguin correctes (`sambashare`).
- Pots consultar l'estat de les regles del firewall amb:
  ```bash
  sudo iptables -L -v --line-numbers
  ```
- Per més opcions de configuració, consulta la documentació oficial de Samba.