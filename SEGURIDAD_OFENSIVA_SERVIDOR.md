## 🔐 Documentación de Seguridad del Servidor

**Este documento registra hallazgos obtenidos mediante tecnicas de reconociemiento ofensivo y las medidas de mitigacion aplicadas.**

---


### 1. Informacion General

**Servidor**

- Sistema Operativo: Debian 12

- Servicio Principal:  Apache2 

- Exposición: Cloudflare Tunnel

- Dominio: https://c8x1v93ka92mlq07qv3h.site/
  
- Vector de acceso: Externo (WLAN)

- Uso: Testing

**Objetivo del servidor**

Servidor configurado exclusivamente para pruebas controladas de seguridad y validación de arquitectura Zero Trust mediante Cloudflare Tunnel.

---

### 2. Metodología Utilizada

- Se aplicaron técnicas de reconocimiento ofensivo sin explotación activa

**Tipo de Analisis**

- Reconocimiento pasivo
- Reconocimiento activo controlado
  
**Herrramientas utilizadas**

- ReconLayer (herramienta modular)
- 
  modulos pasivos:
  
  - Nslookup
  - Whois dns
  - Whois ip
  - Subfinder
  - Crt.sh
  - Wayback

  modulos activos :

  - WafW00f
  - Nmap
  - Whatweb
  - Nucei
  - Wpscan
  - Openssl
  - FUZZ / Dirb


- Curl


### 3. Hallazgos de Seguridad

**🔴 Hallazgo #1: Exposición de Server Status**




