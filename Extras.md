--En la máquina Linux Mint (controladora Ansible):

Instalar Ansible:


sudo apt update
sudo apt install -y ansible
Crear playbook practica_c.yml:

yaml
---
- name: Configurar VM en DigitalOcean
  hosts: all
  become: yes
  tasks:
    - name: Actualizar repositorios
      apt:
        update_cache: yes

    - name: Crear usuario adrian123
      user:
        name: adrian123
        groups: sudo
        append: yes
        shell: /bin/bash

    - name: Instalar bashtop
      apt:
        name: bashtop
        state: present
      
Ejecutar playbook:


ansible-playbook -i "IP_de_tu_VM_DigitalOcean," -u root -k practica_c.yml







--Snort para detectar puertos y bloquear con UFW
En la máquina Linux Mint principal:

Instalar Snort y UFW:


sudo apt update
sudo apt install -y snort ufw
Configurar Snort para monitorear puertos 22, 80 e ICMP:
Editar /etc/snort/snort.conf y agregar:


alert tcp any any -> any 22 (msg:"SSH traffic detected"; sid:1000001;)
alert tcp any any -> any 80 (msg:"HTTP traffic detected"; sid:1000002;)
alert icmp any any -> any any (msg:"ICMP traffic detected"; sid:1000003;)
Iniciar Snort:


sudo snort -A console -q -c /etc/snort/snort.conf -i eth0
Bloquear puertos con UFW:


sudo ufw enable
sudo ufw deny 22
sudo ufw deny 80
sudo ufw deny icmp





--Keepalived con contenedores NGINX
En ambas máquinas Linux Mint (principal y clon):

Instalar Docker y Keepalived:


sudo apt update
sudo apt install -y docker.io keepalived
En el servidor 1 (maestro), crear /etc/keepalived/keepalived.conf:


vrrp_instance VI_1 {
    state MASTER
    interface eth0
    virtual_router_id 51
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass 1234
    }
    virtual_ipaddress {
        192.168.1.100/24
    }
}
En el servidor 2 (backup), mismo archivo pero con:


state BACKUP
priority 50
Crear contenedores NGINX en ambos servidores:


sudo docker run -d -p 80:80 --name nginx1 -e CONTENT="Servidor 1 - Tu Nombre y Matrícula" nginx
sudo docker exec nginx1 bash -c 'echo "$CONTENT" > /usr/share/nginx/html/index.html'
En el segundo servidor usar nginx2 y Servidor 2 - Tu Nombre y Matrícula.





--Playbook Ansible para usuario y Notepad++
En la máquina Linux Mint (controladora Ansible):

Instalar dependencias para Windows:


sudo apt install -y python3-winrm
Crear playbook windows_playbook.yml:

yaml
---
- name: Configurar Windows
  hosts: windows
  gather_facts: no
  tasks:
    - name: Crear usuario administrador
      win_user:
        name: usuario_admin
        password: Password123
        groups:
          - Administrators
        state: present

    - name: Instalar Notepad++
      win_chocolatey:
        name: notepadplusplus
        state: present
Ejecutar playbook:


ansible-playbook -i "IP_de_Windows," -u usuario_admin -k windows_playbook.yml





-- Windows en dominio Samba con carpeta compartida
En la máquina Linux Mint (servidor Samba):

Instalar Samba:


sudo apt update
sudo apt install -y samba
Configurar Samba (/etc/samba/smb.conf):


[global]
   workgroup = SAMBADOMAIN
   security = user
   domain logons = yes
   domain master = yes

[carpeta_compartida]
   path = /srv/compartida
   browsable = yes
   read only = no
   guest ok = no
Crear usuario Samba:


sudo useradd usuario_samba
sudo smbpasswd -a usuario_samba
sudo mkdir -p /srv/compartida
sudo chmod 777 /srv/compartida
En Windows 10:

Unirse al dominio desde "Sistema" > "Configuración avanzada"

Mapear unidad de red con credenciales Samba





--WordPress con Docker Compose
En la máquina Linux Mint principal:

Crear docker-compose.yml:

yaml
version: '3'
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: teletubies123
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8888:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
Ejecutar:


sudo docker-compose up -d






--Terraform en DigitalOcean
En la máquina Linux Mint principal:

Instalar Terraform:


sudo apt update
sudo apt install -y terraform
Crear digitalocean.tf:

hcl
provider "digitalocean" {
  token = "API_TOKEN_DEL_PROFESOR"
}

resource "digitalocean_project" "examen_final" {
  name        = "examen final"
  description = "Proyecto para el examen final"
}

resource "digitalocean_droplet" "servidor" {
  image    = "ubuntu-20-04-x64"
  name     = "servidor-examen"
  region   = "nyc3"
  size     = "s-1vcpu-1gb"
  ssh_keys = ["HASH_LLAVE_PUBLICA_PROFESOR"]
}

resource "digitalocean_firewall" "reglas" {
  name = "reglas-examen"
  droplet_ids = [digitalocean_droplet.servidor.id]

  inbound_rule {
    protocol         = "tcp"
    port_range       = "22"
    source_addresses = ["0.0.0.0/0"]
  }

  inbound_rule {
    protocol         = "tcp"
    port_range       = "80"
    source_addresses = ["0.0.0.0/0"]
  }

  inbound_rule {
    protocol         = "tcp"
    port_range       = "443"
    source_addresses = ["0.0.0.0/0"]
  }

  inbound_rule {
    protocol         = "tcp"
    port_range       = "445"
    source_addresses = ["0.0.0.0/0"]
  }
}
Ejecutar:


terraform init
terraform apply





--CUPS (servidor de impresión)
En la máquina Linux Mint principal:

Instalar CUPS:


sudo apt update
sudo apt install -y cups
Configurar CUPS:


sudo usermod -a -G lpadmin $USER
sudo cupsctl --remote-any
sudo systemctl restart cups
Acceder a la interfaz web en http://localhost:631 y agregar impresora virtual.

En Windows 10:

Agregar impresora de red usando la IP de la máquina Linux con CUPS.
