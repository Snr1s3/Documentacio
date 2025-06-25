## Veure la llista de paquets Snap instal·lats

```bash
snap list
```

## Eliminar els paquets Snap

Per eliminar tots els paquets Snap instal·lats, executa per cada Snap de la llista:

```bash
sudo snap remove --purge <nom-del-snap>
```

Un cop eliminats tots els paquets Snap, pots desinstal·lar Snapd i netejar els fitxers residuals:

```bash
sudo apt remove --purge snapd
sudo apt-mark hold snapd
sudo rm -rf /home/*/snap
sudo find / -type d -name snap
```

> **Nota:** La comanda `sudo apt-mark hold snapd` impedeix que Snapd es torni a instal·lar automàticament en futures actualitzacions.