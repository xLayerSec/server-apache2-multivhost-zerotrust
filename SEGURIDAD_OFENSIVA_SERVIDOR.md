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
 
  modulos pasivos:
  
  - Nslookup
  - Whois dns
  - Whois ip
  - Subfinder
  - Crt.sh
  - Wayback

  modulos activos:

  - WafW00f
  - Nmap
  - Whatweb
  - Nucei
  - Wpscan
  - Openssl
  - FUZZ / Dirb

 - Curl

---


### 3. Hallazgos de Seguridad

**🟠 Hallazgo #1: Exposición de Server Status**

/server-status

este endpoint permite visualizar informacion del servidor apache2 como:

- Conexiones activas
- Rutas solicitadas
- Estado de servicio
- Enumeracion de recursos

**Impacto**
- Filtracion de informacion sensible
- Facilita fingerprinting del servidor
- Aumenta superficie de ataque
- Ataques dirigidos

**⚠️Principal factor de filtracion de datos es el "Modo Debug" exponiendo Logs y Errores**

Un error que incluya fragmentos de la memoria o de las ultimas contultas realizadas donde podrian ir usuarios y 
contraseñas


**⚠️Mala implementacion de server-status**

manda todo el objeto del servidor sin filtracion de datos, si  por algun error arrastra datos de sessiones 
activas o usuarios conectados los datos quedarian expuestos 

**Nivel de riesgo**

🟠 Medio, con riesgo de escalada dependiendo del contexto

---

### 4. Evidencia de hallazgos

**Herramienta:** Reconlayer

**Modulo:** FUFF/DIBR

**Tecnica:** Enumeracion de rutas comunes

**Resultados observados**

- El servidor respondio con 200 OK al acceder a /server-status
- El servidor devolvio un json con los datos del servidor mediante "curl" a /server-status















