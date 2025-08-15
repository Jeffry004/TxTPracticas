1. Ansible

sudo apt update
sudo apt install -y ansible

2. Docker y Docker Compose

sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
sudo usermod -aG docker $USER

3. CUPS (Servidor de impresión)

sudo apt update
sudo apt install -y cups
sudo systemctl enable --now cups
sudo usermod -a -G lpadmin $USER

4. Snort (IDS)

sudo apt update
sudo apt install -y snort

5. Keepalived (HA)

sudo apt update
sudo apt install -y keepalived

7. Samba (Compartir archivos)

sudo apt update
sudo apt install -y samba
sudo systemctl enable --now smbd

8. UFW (Firewall)

sudo apt update
sudo apt install -y ufw

9. SSH Server

sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
Configuración de SSH Key
Generar clave SSH

ssh-keygen -t rsa -b 4096 -C "tu_email@ejemplo.com"
Copiar clave pública a servidor remoto

ssh-copy-id usuario@direccion_ip
Gestión de Firewall (UFW)
Habilitar UFW

sudo ufw enable
Permitir puertos

sudo ufw allow 22/tcp   # SSH
sudo ufw allow 80/tcp   # HTTP
sudo ufw allow 443/tcp  # HTTPS

Denegar puertos

sudo ufw deny 22/tcp
sudo ufw deny 80/tcp

Ver estado
sudo ufw status numbered

Eliminar regla
sudo ufw delete [NÚMERO_DE_REGLAS]
Gestión de Puertos con iptables
Bloquear puerto

sudo iptables -A INPUT -p tcp --dport 22 -j DROP
Permitir puerto

sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
Guardar reglas iptables

sudo apt install -y iptables-persistent
sudo netfilter-persistent save
Configuración Básica de Red
Ver interfaces de red

ip a
Reiniciar red

sudo systemctl restart networking
Ver rutas

ip route
Configuración de Samba
Editar configuración

sudo nano /etc/samba/smb.conf
Reiniciar servicio

sudo systemctl restart smbd
Añadir usuario Samba

sudo smbpasswd -a nombre_usuario
Configuración de CUPS
Habilitar acceso remoto

sudo cupsctl --remote-any
sudo systemctl restart cups
Ver impresoras instaladas

lpstat -a
Comandos Básicos de Docker
Listar contenedores

docker ps -a
Iniciar/Detener contenedor

docker start nombre_contenedor
docker stop nombre_contenedor
Eliminar contenedor

docker rm nombre_contenedor
Ver imágenes

docker images

Comandos Básicos de Ansible
Probar conexión

ansible all -m ping -i inventario.ini

Ejecutar playbook

ansible-playbook -i inventario.ini playbook.yml
Actualización del Sistema
Actualizar lista de paquetes

sudo apt update
Actualizar paquetes instalados

sudo apt upgrade -y

Limpiar paquetes innecesarios

sudo apt autoremove -y


