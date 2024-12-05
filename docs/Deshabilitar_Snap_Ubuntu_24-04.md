
## Veure llista de Snaps
```bash
snap list
```

## Eliminar els Snaps
Per cada Snap de la llista
```bash
snap remove --purge «snap»

#Una vegada has eliminat tots els snaps toca 
sudo apt remove --purge snapd
sudo apt-mark hold snapd
sudo rm -R /home/*/snap
find / -type d -name snap
```