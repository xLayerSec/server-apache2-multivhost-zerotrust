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
sudo service apache2 enable
```

**Verificamos que este corriendo**

```bash
sudo service apache2 status
```





























