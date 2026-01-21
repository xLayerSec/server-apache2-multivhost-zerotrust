# 📙 Guia paso a paso - Apache2 + Multiples dominios + Cloudflared

En esta guia se documenta el proceso completo para desplegar un servidor apache2
en Debian 12 capaz de servir multiples dominios utilizando VirtualHost, exponiendolos
a Internet mediante un solo tunnel de Cloudflared Zero Trust 

### ⚙️ Requisitos 

- Servidor dedicado o VM
- Debian 12 minimal
- Acceso root
- Dominios activos
- Cuenta cloudflare

## 📟 Comandos

 
1️⃣ **Primero acceso como root**

```bash
su -
```

**Actualizamos el sistema** 

```bash
apt update && apt upgrade -y
```




