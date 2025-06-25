# Configuració inicial de Fedora Gnome

## 1. Eliminar programari innecessari i instal·lar programari útil

```bash
sudo dnf update
sudo dnf remove gnome-games libreoffice* gnome-weather gnome-boxes gnome-contacts gnome-maps gnome-calculator gnome-calendar gnome-tour gnome-connections
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
sudo dnf check-update
sudo dnf install code keepassxc golang gnome-tweaks fastfetch
```

## 2. Instal·lar JetBrains Toolbox

Descarrega l'última versió des de la [pàgina oficial](https://www.jetbrains.com/toolbox-app/), després executa:

```bash
tar -xvf jetbrains-toolbox-*.tar.gz
cd jetbrains-toolbox-*/
./jetbrains-toolbox
```

## 3. Canviar el hostname

```bash
sudo hostnamectl set-hostname Bondia
```

## 4. Generar una clau SSH

```bash
ssh-keygen -t ed25519
```

## 5. Crear directoris importants i enllaços

```bash
cd ~
mkdir -p ~/Docs ~/Proj
git clone git@github.com:Snr1s3/Documentacio.git ~/Proj/Documentacio
ln -s ~/Proj/Documentacio/docs ~/Docs
```

---

**Consells addicionals:**
- Revisa la configuració de GNOME Tweaks per personalitzar l'entorn.
- Utilitza `fastfetch` per mostrar informació del sistema al terminal.