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
2. Iniciar el servicio SSH:
    ```bash
    service ssh start
    ```
3. Habilitar el servicio SSH para que se inicie automáticamente al arrancar el sistema:
    ```bash
    sudo systemctl enable ssh
    ```
4. Iniciar el servicio SSH: 
    ```bash
    sudo systemctl start ssh
    ```
5. Verificar el estado del servicio SSH:
    ```bash
    sudo systemctl status ssh
    ```

### Configuración básica de SSH
- **Puerto SSH**: Por defecto, el puerto SSH es el 22. Puedes cambiarlo en `/etc/ssh/sshd_config`.
- **PermitRootLogin**: Puedes habilitar o deshabilitar el acceso de root a través de SSH: `PermitRootLogin yes/no`.
- **AllowUsers**: Puedes especificar qué usuarios tienen permiso para acceder a través de SSH: `AllowUsers user1 user2`.

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
```bash
ssh usuario@direccion_ip
```