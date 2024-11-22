# Proyecto NGIX con Vagrant

Este Vagrantfile configura una máquina virtual utilizando Vagrant con una base Debian (bookworm64). Configura una red privada, instala Nginx, Git y VSFTPD, y establece dos sitios web: uno basado en Git y otro basado en FTP. La configuración también incluye la creación de un certificado SSL y la gestión de servicios de Nginx y VSFTPD.

## Estructura de Archivos

### Vagrantfile
Este archivo es la configuración principal para Vagrant:

  - Nombre de la VM: ismael
  - IP de red privada: `192.168.57.102`
  - Distribucion: `debian/bookworm64`

  **Primer Sitio Web**
  - Crea un directorio para el contenido web en /var/www/ismael/html.
  - Clona un ejemplo de sitio web estático desde GitHub en este directorio.
  - Establece la propiedad para www-data y permisos.
  - Copia el archivo de configuración del sitio Nginx desde /vagrant/ismael-sites a /etc/nginx/sites-available/.
  - Habilita el sitio mediante un enlace simbólico en /etc/nginx/sites-enabled/.

  **Login Sitio Web**
  - Crea un directorio para un segundo servicio web en /var/www/login/.
  - Realizamos un cp para subir nuestra carpeta html para la segunda web
  - Realizamos los mismos pasos que en la primera

### ismael-sites
```
server {
    listen 80;
     listen [::]:80;
    root /var/www/ismael/html;
    server_name ismael www.ismael.com;
    index index.html index.htm index.nginx-debian.html;

    location / {
        deny 192.168.57.1;
        allow all;
        try_files $uri $uri/ =404;
    }
}

```

- Raíz del documento: El contenido del servidor está ubicado en el directorio /var/www/ismael/html.
- Nombre del servidor: El nombre del servidor es ismael.
- Denegamos nuestra ip y permitimos todas las demas

### segunda-sites
```
server {
    listen 80;
    listen [::]:80;
    root /var/www/login/html;
    index index.html index.htm index.nginx-debian.html;
    server_name login;
    location / {
        allow 192.168.57.1;
        deny all;
        try_files $uri $uri/ =404;
    }
    location /contact.html {
        auth_basic "Área restringida";
        auth_basic_user_file /etc/nginx/.htpasswd;
        try_files $uri $uri/ =404;
    }
    
}
```

- Raíz del documento: El contenido del servidor está ubicado en el directorio /var/www/segunda/html.
- Nombre del servidor: El nombre del servidor es segunda.
- Permitimos la ip 192.168.57.1
- No permitimos ninguna ip mas
- Le hacemos que introduzca usuario y contraseña para el contact

## access.log error.log
![imagen1](img/Captura%20de%20pantalla%202024-11-22%20202331.png)
![imagen1](img/Captura%20de%20pantalla%202024-11-22%20202512.png)


