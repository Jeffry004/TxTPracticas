                             -------------------- Practica 2 - Configuración de parametros de red. ----------------------------------

clear
ip a
ip addr del 10.0.0.200/24 dev enp0s3
ip addr del 10.0.0.154/24 dev enp0s3
nano /etc/netplan/01-netcfg.yaml



network:
  version: 2
  ethernets:
    enp0s3:
      addresses:
      - "10.0.0.150/24"
      routes:
      - to: "default"
        via: "10.0.0.1"

^x
nano /etc/netplan/50-cloud-init.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
^O
^X
ip a
nano /etc/netplan/50-cloud-init.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: false
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
^o
^x
nano /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: false
      addresses:
        - 192.168.1.100/24   
      gateway4: 192.168.1.1  
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
^o
^x
netplan apply
ip a


                              -----------------------  Practica 3 -----------------------------

# Cambiar a superusuario para tener todos los permisos
sudo su

# Añadir el usuario 'jefry' al grupo 'sudo' para que pueda ejecutar comandos como administrador
usermod -aG sudo jefry

# Verificar UID, GID y grupos a los que pertenece 'michael'
id michael

# Crear un nuevo grupo llamado 'guest'
groupadd guest

# Crear el usuario 'adrian' asignándolo directamente al grupo principal 'guest'
adduser --ingroup guest adrian

# Verificar UID, GID y grupos de 'adrian'
id adrian

# Eliminar el usuario 'adrian' junto con su carpeta personal
deluser --remove-home adrian

# Eliminar el grupo 'guest' (solo si no hay usuarios que lo estén usando como grupo principal)
groupdel guest

# Eliminar el usuario 'michael' junto con su carpeta personal
deluser --remove-home michael


# Listar todos los usuarios registrados en el sistema
getent passwd

# Listar todos los grupos del sistema
getent group


# Verificar si 'jefry' puede usar sudo
su - jefry
sudo whoami



                        ------------------------ --------- PRACTICA 4 ----------------------------------
                         
# -----------------------------------------
# CHMOD Y PERMISOS EN LINUX
# -----------------------------------------
# Un archivo o carpeta tiene permisos para:
#  - Propietario (usuario dueño)
#  - Grupo (usuarios en su mismo grupo)
#  - Otros (resto de usuarios)
#
# Cada permiso tiene un valor:
#  r = 4  (lectura)
#  w = 2  (escritura)
#  x = 1  (ejecución/acceso)
#
# Se suman los valores para cada bloque:
#  rwx = 4+2+1 = 7
#  rw- = 4+2+0 = 6
#  r-- = 4+0+0 = 4
#
# Ejemplo: chmod 750 archivo
#  7 = rwx dueño
#  5 = r-x grupo
#  0 = --- otros
#
# Para ver permisos: ls -l
# Ej: -rwxr-x---
#      |  |  └─ otros
#      |  └──── grupo
#      └─────── dueño
# -----------------------------------------

# Crear carpeta materia
mkdir -p ~/materia

# Editar archivo con vi (i → escribir → ESC → :wq → Enter)
vi ~/materia/estudiante.txt

# Permisos: rwx solo dueño
chmod 700 ~/materia/estudiante.txt

# Crear carpeta materia2
mkdir -p ~/materia2

# Copiar archivo a materia2
cp ~/materia/estudiante.txt ~/materia2/

# Quitar permisos al dueño, dar rwx a grupo
chmod 070 ~/materia/estudiante.txt

# Borrar carpeta materia y su contenido
rm -rf ~/materia

# Ver info de carpeta materia2
ls -ld ~/materia2
                         
