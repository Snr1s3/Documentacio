
Eliminar programari inncecessari i Instalar Programari Util
```
sudo dnf update
sudo dnf remove gnome-games libreoffice* gnome-weather gnome-boxes gnome-contacts gnome-maps gnome-calculator gnome-calendar gnome-tour gnome-connections
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\nautorefresh=1\ntype=rpm-md\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" | sudo tee /etc/yum.repos.d/vscode.repo > /dev/null
dnf check-update
sudo dnf install code keepassxc golang gnome-tweaks fastfetch

```


Instalar Jetbrains Toolbox
```
tar -xvf jetbrains-toolbox-*
cd jetbrains-toolbox-*
cd /bin
./jetbrains-toolbox
```

Canviar el hostname
```
sudo echo Bondia >> /etc/hostname
```

Generar key ssh
```
ssh-keygen -t ed25519
```

Crear directoris importats
```
cd ~/
mkdir ./Docs ./Proj
```