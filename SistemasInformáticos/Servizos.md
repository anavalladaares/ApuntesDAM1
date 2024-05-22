# Servicios y Configuraciones en Ubuntu 

En este documento se describen diferentes servicios y configuraciones que se pueden realizar en sistemas basados en Ubuntu y otros sistemas operativos Linux. Estos servicios y configuraciones son comunes en entornos de red y servidores, y pueden ser útiles para administradores de sistemas y redes.

### 1. Configuración de la Red
#### 1.1 Configuración de Red Cableada e Inalámbrica
- **ifconfig**: Usado para configurar manualmente las interfaces de red. Ejemplo:
  ```bash
  ifconfig eth0 192.168.1.2 netmask 255.255.255.0 up
  ```
- **iwconfig**: Utilizado para la configuración de la red inalámbrica.
- **dhclient**: Para obtener una dirección IP de un servidor DHCP de forma dinámica.
- **/etc/network/interfaces**: Aquí puedes guardar la configuración de la red para que persista después de reiniciar el sistema.

#### 1.2 Configuración de iptables
- **iptables**: Se usa para configurar el firewall y establecer reglas de enrutamiento y permisos de red.
  Ejemplo para configurar NAT:
  ```bash
  iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -d 0/0 -j MASQUERADE
  ```

### 2. Servicio DHCP
- **Instalación y configuración de DHCP** para asignar direcciones IP automáticamente en la red.
  Instalación:
  ```bash
  apt-get install isc-dhcp-server
  ```
  Configuración básica en `/etc/dhcp/dhcpd.conf`:
  ```plaintext
  subnet 10.0.0.0 netmask 255.255.255.0 {
    range 10.0.0.100 10.0.0.200;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
    option routers 10.0.0.1;
  }
  ```

### 3. Compartir Archivos e Impresoras (Samba)
- **Samba**: Permite compartir archivos e impresoras con equipos Windows.
  Instalación:
  ```bash
  apt-get install samba smbclient
  ```
  Configuración básica en `/etc/samba/smb.conf` para compartir una carpeta:
  ```ini
  [shared]
  path = /path/to/shared
  writable = yes
  valid users = user1 user2
  ```

### 4. NFS - Sistema de Archivos de Red
- **NFS**: Configuración para compartir directorios entre sistemas Unix/Linux.
  Ejemplo de configuración en `/etc/exports`:
  ```plaintext
  /datos 192.168.20.9(rw) 192.168.20.8(ro)
  ```

### 5. Acceso Remoto
- **SSH (Secure Shell)**: Herramienta segura para acceso remoto.
  Instalación:
  ```bash
  apt-get install openssh-server
  ```
  Configuración básica en `/etc/ssh/sshd_config`.

- **VNC**: Para acceso gráfico remoto.
  Instalación:
  ```bash
  apt-get install tightvncserver
  ```

### 6. Servidor Web
- **Apache**: Uno de los servidores web más populares.
  Instalación:
  ```bash
  apt-get install apache2
  ```
  Configuración básica en `/etc/apache2/apache2.conf`.

### 7. Servidor FTP
- **VSFTP**: Servidor FTP seguro.
  Instalación:
  ```bash
  apt-get install vsftpd
  ```
  Configuración básica en `/etc/vsftpd.conf`.

## SSH
### Pasos para la instalar SSH en Ubuntu
1. Instalar el servidor SSH:
    ```bash
    sudo apt-get install ssh
    ```
2. Habilitar el servicio SSH para que se inicie automáticamente al arrancar el sistema:
    ```bash
    sudo systemctl enable ssh
    ```
3. Iniciar el servicio SSH: 
    ```bash
    sudo systemctl start ssh
    ```
4. Verificar el estado del servicio SSH:
    ```bash
    sudo systemctl status ssh
    ```

### Configuración básica de SSH
1. **PrintMotd**: Permite mensajes de entrada o conexión:
    ```bash
    sudo sed -i 's/^#*PrintMotd.*$/PrintMotd yes/' /etc/ssh/sshd_config

    sudo sed -i 's/^#*Banner.*$/Banner \/etc\/issue.net/' /etc/ssh/sshd_config
    ```
2. **PermitRootLogin**: Habilita o deshabilita el acceso de root a través de SSH:
    ```bash
    sudo sed -i 's/^PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config
    ```

3. **IgnoreUserKnownHosts**: Configuración de seguridade e control de acceso.
    ```bash
    sudo sed -i 's/^#*IgnoreUserKnownHosts.*$/IgnoreUserKnownHosts no/' /etc/ssh/sshd_config

    sudo sed -i 's/^#*GatewayPorts.*$/GatewayPorts no/' /etc/ssh/sshd_config

    sudo sed -i 's/^#*AllowTcpForwarding.*$/AllowTcpForwarding yes/' /etc/ssh/sshd_config
    ```
4. **Subsistemas**: Uso de subsistemas para otras aplicaciones, como por ejemplo, FTP.
    ```bash
    sudo sed -i 's/^#*Subsystem\s\+sftp\s\+\/usr\/lib\/openssh\/sftp-server/Subsystem sftp \/usr\/lib\/openssh\/sftp-server/' /etc/ssh/sshd_config
    ```

### Como saber si esta instalado SSH
```bash
sudo systemctl status ssh
```
Debería salir lo siguiente:
  ```bash
  ● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2021-10-12 15:00:00 -03; 1h 30min ago
  ```

### Como usar SSH
- Como saber dirección IP
```bash
ip a
```
- Conectarse a un servidor SSH:
```bash
ssh usuario@direccion_ip
ej. ssh ana@10.0.2.15
```

- Salir de la sesión SSH:
```bash
exit
```

## Servidor Web
### Pasos para instalar el servidor Apache en Ubuntu
1. Instalar el servidor Apache
   ```bash
   sudo apt-get install apache2
   ```
2. Comprobar que Apache se ha instalado correctamente
   ```bash
   apache2 -v
   ```
   Debería mostrar la versión de Apache instalada: `Server version: Apache/2.4.41 (Ubuntu)`
2. Iniciar el servicio automaticamente
   ```bash
   sudo systemctl enable apache2
   ```
3. Iniciar ahora el servicio:
   ```bash
   service apache2 start
   ```
### Pasos para instalar PHP en Ubuntu
1. Instalar PHP
   ```bash
   sudo apt-get install php
   ```
2. Comprobar que PHP se ha instalado correctamente
   ```bash
   php -v
   ```
   Debería mostrar la versión de PHP instalada: `PHP 8.1.2 (cli) (built: Oct  6 2021 08:47:56) ( NTS )`
3. Reiniciar el servidor Apache
   ```bash
   service apache2 restart
   ```

## Servidor FTP
### Pasos para instalar un servidor FTP en Ubuntu
1. Instalar el servidor FTP
   ```bash
   sudo apt-get install vsftpd
   ```
2. Comprobar que vsftpd se ha instalado correctamente
   ```bash
    vsftpd -v
    ```
    Debería mostrar la versión de vsftpd instalada: `vsftpd: version 3.0.3`
3. Iniciar el servicio vsftpd
    ```bash
    sudo systemctl start vsftpd
    ```

## Servidor NFS
### Pasos para instalar un servidor NFS en Ubuntu
1. Instalar el servidor NFS
   ```bash
   sudo apt-get install nfs-kernel-server
   ```
2. Comprobar que nfs-kernel-server se ha instalado correctamente
   ```bash
    nfs-kernel-server -v
    ```
    Debería mostrar la versión de nfs-kernel-server instalada: `nfs-kernel-server: version 1.3.4`
3. Iniciar el servicio nfs-kernel-server
    ```bash
    sudo systemctl start nfs-kernel-server
    ```
