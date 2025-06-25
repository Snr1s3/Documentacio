# Instal·lació de Docker a Ubuntu 24.04

## 1. Afegir el repositori oficial de Docker

```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

## 2. Instal·lar Docker

```bash
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 3. Verificar la instal·lació

```bash
sudo docker run hello-world
```

> **Consell:** Si vols executar Docker sense `sudo`, afegeix el teu usuari al grup `docker`:
> ```> bash
> sudo usermod -aG docker $USER
> ```
> Després, tanca la sessió i torna a entrar.