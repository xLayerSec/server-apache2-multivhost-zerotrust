# 📙 Guia paso a paso - Apache2 + Multiples dominios + Cloudflared

En esta guia se documenta el proceso completo para desplegar un servidor apache2
en Debian 12 capaz de servir multiples dominios utilizando VirtualHost, exponiendolos
a Internet mediante un solo tunnel de Cloudflared Zero Trust 

----

### ⚙️ Requisitos 

- Servidor dedicado o VM
- Debian 12 minimal
- Acceso root
- Dominios activos
- Cuenta cloudflare

----

## 📟 Comandos

 
### 1️⃣ **Primer acceso como root**

```bash
su -
```

**Actualizamos el sistema** 

```bash
apt update | apt upgrade -y
```

### 2️⃣ **Instalacion de sudo**

```bash
apt install sudo -y
```

**Agregamos nuestro usuario al grupo sudo**

```bash
usermod -aG sudo TU_USUARIO
```
**⚠️Cerrar sesion y volver a entrar para que los permisos se apliquen**

---

### 3️⃣ **Acceso por SSH**

**Desde otra terminal, ingresamos por SSH con nuestro usuario (ya no root)**

```bash
ssh TU_USUARIO@IP_DEL_SERVIDOR 
```

### 4️⃣ **Instalacion de dependencias basicas**

**Sin ser root solo usaremos sudo con nuestro usuario normal**

```bash
sudo apt install -y curl wget gnupg2 ca-certificates lsb-release 
```

### 5️⃣ **Instalacion de Apache2**

```bash
sudo apt install apache2 -y
```

**Iniciamos y habilitamos Apache2**

```bash
sudo service apache2 start
sudo systemctl enable apache2
```

**Verificamos que este corriendo**

```bash
sudo service apache2 status
```

###  8️⃣ **Cerrar puertos y escuchar solo localhost**

```bash
sudo nano /etc/apache2/ports.conf
```

**Cambiar a**

```bash
Liten 127.0.0.1:80
```

**Reiniciamos apache2**

```bash
sudo service apache2 restart 
```

### 9️⃣ **Crear Virtual Host**

```bash
sudo nano /etc/apache2/sites-available/MI_DOMINIO.conf 
```

**Ejemplo:**

```bash
<VirtualHost *:80>
ServerName MI_DOMINIO.com
ServerAlias www.MI_DOMINIO.com
DocumentRoot /var/www/MI_DOMINIO


<Directory /var/www/MI_DOMINIO>
AllowOverride All
Require all granted
</Directory>


ErrorLog ${APACHE_LOG_DIR}/MI_DOMINIO_error.log
CustomLog ${APACHE_LOG_DIR}/MI_DOMINIO_access.log combined
</VirtualHost> 
```

### 🔟 **Crear carpeta del sitio**

```bash
sudo mkdir -p /var/www/MI_DOMINIO

sudo chown -R www-data:www-data /var/www/MI DOMINIO

sudo chmod -R 755 /var/www/MI_DOMINIO
```

**Crear un Index de prueba**

```bash
echo "MI_DOMINIO Funcionando" | sudo tee /var/www/MI_DOMINIO/index.html
```
### 1️⃣1️⃣ **Habilitar el sitio**

```bash
sudo a2ensite  MI_DOMINIO.conf
```

```bash
sudo a2dissite  000-default.conf
sudo systemctl reload apache2
```

**Verificar VirtualHost con curl**

```bash
curl -H "Host: MI_DOMINIO" http://127.0.0.1
```

### 1️⃣2️⃣ **Instalar Cloudflared desde github**


```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64

chmod +x cloudflared-linux-amd64

sudo mv cloudflared-linux-amd64 /usr/local/bin/cloudflared
```

**Verificar instalacion correcta**

```bash
cloudflared --version
```

### 1️⃣3️⃣ **Login en Cloudflare**

```bash
cloudflared tunnel login
```
Se abrirá una URL → seleccionar dominio → se descargará el certificado .pem.

### 1️⃣4️⃣ **Crear Tunnel**

```bash
cloudflared tunnel create MI-TUNEL 
```
**Guardar el Tunnel UUID.**

### 1️⃣5️⃣ **Configurar cloudflared**

**Crear directorio**

```bash
mkdir -p ~/.cloudflared
```

**Crear config**

```bash
nano ~/.cloudflared/config.yml
```

**Ejemplo:**

```bash
tunnel: TUNNEL_UUID
credentials-file: /home/user/.cloudflared/TUNNEL_UUID.json


ingress:
- hostname: MI_DOMINIO
service: http://127.0.0.1:80
- hostname: www.MI_DOMINIO
service: http://127.0.0.1:80

- hostname: MI_DOMINIO_2
service: http://127.0.0.1:80
- hostname: www.MI_DOMINIO_2
service: http://127.0.0.1:80


- service: http_status:404
```

**Recargamos Apache2**

```bash
sudo service apache2 reload
```
### 1️⃣6️⃣ **Ejecutar Tunnel**

```bash
cloudflared tunnel run MI-TUNEL 
```
---

## ✅ Resultado final

✔ Apache2 escucha solo en 127.0.0.1 ✔ Puertos cerrados al exterior ✔ Cloudflare Zero Trust como único punto de acceso ✔ Múltiples dominios sirviéndose desde un solo servidor ✔ Esta arquitectura es ideal para entornos detras de CGNAT

## 🔐 Arquitectura

Usuario → Cloudflare → Tunnel → Apache (127.0.0.1)



📌 Autor: xLayerSec

📌 Sistema: Debian 12

📌 Modelo: Zero Trust Architecture












































