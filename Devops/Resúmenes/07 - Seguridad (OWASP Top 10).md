---
tags: [devops, seguridad, security, owasp, devsecops]
unidad: 7
clase: material (Security.pdf)
---

# 7️⃣ Seguridad — Conceptos y OWASP Top 10

> [!abstract] Idea central
> La seguridad en DevOps se integra en **todo el ciclo** (**DevSecOps**, *"Shift Left Security"*: detectar vulnerabilidades **temprano**, no al final). Esta unidad cubre conceptos de **API/HTTP/vulnerabilidad** y el **OWASP Top 10 (2025)** con ejemplos, casos reales y contramedidas.

> [!warning] Fuente de esta unidad
> En las transcripciones de Clases 1–7 **no hay una clase dedicada a Seguridad**. El profesor la nombra como un módulo de la materia y toca seguridad de forma **transversal** (escaneos SAST/Checkov en [[03 - CI-CD]], gestión de secretos en [[04 - Deployment]], DevSecOps en [[01 - Introducción a DevOps]]). Este resumen se basa en el material **`Security.pdf`**.

[[06 - Terraform (Infraestructura como Código)|← Unidad 6]] · [[00 - Índice DevOps|Índice]]

---

## Conceptos base
- **API (Application Programming Interface):** interfaz que permite a un software usar otro. Una **API REST** incrusta la invocación (método + parámetros) en una **solicitud HTTP**.
- **HTTP Request:** Método (GET, POST, PUT, DELETE) · URI · versión HTTP · **Headers** · **Body**.
- **Endpoint:** la **URI** que identifica un recurso. Estructura típica: `Protocolo://dominio/versión/endpoint`.
- **Vulnerabilidad:** comportamiento **inesperado** de un programa que afecta la **confidencialidad, integridad o disponibilidad**. El atacante busca comportamientos que el programador no previó:
  - valores inesperados para un parámetro,
  - combinaciones inesperadas de parámetros,
  - **encadenamiento** inesperado de invocaciones,
  - endpoints o parámetros **ocultos**.

---

## OWASP Top 10 — evolución
OWASP publica listas de los riesgos más críticos. Entre 2019 y 2023 hubo un giro de **vulnerabilidades técnicas simples** hacia **fallas de lógica de negocio** (BOLA/BFLA unificadas, abuso de flujos de negocio, consumo inseguro de APIs).

## OWASP Top 10 (2025)
1. **A01 — Broken Access Control**
2. **A02 — Security Misconfiguration**
3. **A03 — Software Supply Chain Failures**
4. **A04 — Cryptographic Failures**
5. **A05 — Injection**
6. **A06 — Insecure Design**
7. **A07 — Authentication Failures**
8. **A08 — Software or Data Integrity Failures**
9. **A09 — Security Logging and Alerting Failures**
10. **A10 — Mishandling of Exceptional Conditions**

---

### 🔓 A01 — Broken Access Control (Control de acceso roto)
Usuarios actuando **fuera de sus permisos**: manipular tokens JWT/cookies/IDs en la URL (**IDOR/BOLA**), endpoints de admin que no verifican el rol, confiar en datos del cliente en vez del estado del servidor.
- **Casos:** First American Financial (2019, 885M registros por IDOR); Uber (2019, fallas BOLA).
- **Ejemplo:** Alice cambia `/recibos?id=80010` → `80011` y ve el recibo de Bob; itera con un script y baja miles de recibos.
- **Contramedidas:** **denegar por defecto**, **validar pertenencia (ownership)** en el backend, loggear fallos, deshabilitar *listing* y usar **UUIDs** en vez de IDs predecibles, **rate limits**.

### ⚙️ A02 — Security Misconfiguration
Configuraciones inseguras de fábrica: buckets S3 públicos, puertos/credenciales por defecto expuestos, falta de cabeceras de seguridad (HSTS, CSP), **modo Debug en producción**.
- **Casos:** Capital One (2019, WAF mal configurado en AWS, 100M clientes); Twitch (2021, código fuente expuesto).
- **Contramedidas:** **IaC** ([[06 - Terraform (Infraestructura como Código)|Terraform]]) para auditar/versionar config, **menor privilegio**, *hardening* (CIS Benchmarks), parches automáticos.

### 📦 A03 — Software Supply Chain Failures
Las apps modernas ensamblan cientos de componentes open-source. Riesgo: dependencias (`npm`, `PyPI`, `Maven`) comprometidas, CI/CD sin controles fuertes, no verificar firmas/hashes de imágenes Docker.
- **Casos:** SolarWinds (2020, actualización troyanizada); Kaseya (2021, ransomware).
- **Ejemplo:** "paquete envenenado" — atacantes toman control de una librería abandonada, publican una versión que en el **CI/CD** roba tokens de AWS.
- **Contramedidas:** verificar **firmas/hashes** (¡**evitar `latest`** en imágenes Docker!), **SCA** (analizar dependencias, generar **SBOM**), **Zero Trust en CI/CD**, repositorios internos auditados.

### 🔑 A04 — Cryptographic Failures
Debilidades en proteger datos **en reposo** y **en tránsito**: HTTP en vez de HTTPS, hashing débil (MD5/SHA1 sin *salt*), llaves *hardcodeadas*, modos de cifrado anticuados (ECB).
- **Casos:** Equifax (2017, datos en texto plano); Adobe (2013, cifrado reversible sin salt).
- **Contramedidas:** **TLS/HTTPS** + HSTS, cifrado en reposo (AES-256), **gestión de secretos** (Vault, Secrets Manager — nunca *hardcodear*), hashing **lento** (bcrypt, Argon2) con **salt** único.

### 💉 A05 — Injection
Datos del usuario no sanitizados que se ejecutan como comandos (SQL, NoSQL, OS, LDAP, XML).
- **Casos:** TalkTalk (2015, SQLi); Panama Papers / Mossack Fonseca (2016).
- **Ejemplo:** login con usuario `admin' --` → el `--` comenta el chequeo de contraseña → acceso de superusuario.
- **Contramedidas:** **consultas parametrizadas** (Prepared Statements), **ORMs seguros**, **validación positiva** (allow-listing), escapar variables dinámicas.

### 🏗️ A06 — Insecure Design (Diseño inseguro)
Vulnerabilidades introducidas **desde la arquitectura** (no es un bug de implementación): flujos lícitos pero **abusables**, falta de *threat modeling* y de *rate limits*, recuperación de contraseña demasiado "cómoda".
- **Casos:** Zoom (2020, "Zoombombing" — reuniones sin contraseña por defecto, IDs cortos); Tesla (2020, sistema sin llave).
- **Ejemplo:** bots que reservan todo el inventario en milisegundos al abrir una venta.
- **Contramedidas:** **Threat Modeling** en la arquitectura, fricción por seguridad (rate limiting, CAPTCHA), controles de lógica de negocio, **seguridad por diseño (Shift-Left)**.

### 👤 A07 — Authentication Failures
Debilidades en confirmar la identidad: sin *account lockout* (fuerza bruta, **Credential Stuffing**), sin **MFA**, contraseñas débiles, mal manejo de sesiones (tokens de larga vida, *logout* que no invalida el token en el servidor).
- **Casos:** Nintendo (2020, 300k cuentas por credential stuffing); 23andMe (2023, MFA opcional + reúso de contraseñas).
- **Contramedidas:** **MFA obligatorio**, defensas anti-fuerza-bruta (bloqueos, rate limit), cotejar contraseñas contra bases filtradas, **gestión estricta de sesiones**.

### 🧩 A08 — Software or Data Integrity Failures
Confiar ciegamente en componentes/actualizaciones. Incluye **deserialización insegura** (reconstruir objetos del usuario → **RCE**), CDNs sin **SRI**, parches sin verificar firma.
- **Casos:** Asus (2019, ShadowHammer — actualización oficial troyanizada con firma válida); CCleaner (2017).
- **Ejemplo:** cookie con objeto Java serializado → atacante forja un payload con `ysoserial` → *reverse shell*.
- **Contramedidas:** evitar deserializar objetos nativos (usar **JSON** + validación de esquema), **SRI** (hash de scripts externos), **firmar digitalmente** los artefactos en CI/CD (PGP) y validarlos en producción.

### 📋 A09 — Security Logging and Alerting Failures
Sin logging disciplinado ni alertas, las brechas son **invisibles**: incidentes que no se registran, logs locales dispersos (sin **SIEM**), logs sin metadatos forenses (IP, timestamp, usuario), sin umbrales de alerta.
- **Casos:** Marriott/Starwood (2018, atacantes 4 años sin detección); Equifax (2017, 76 días).
- **Contramedidas:** **registrar acciones críticas** (logins, cambios de permisos), **centralizar en SIEM** (evitar borrado local), metadatos de calidad (UTC, IP, usuario), **alertas proactivas** ante anomalías. *(Enlaza con [[05 - Monitoreo y Observabilidad|logs y alertas]].)*

### 💥 A10 — Mishandling of Exceptional Conditions
Nuevo en 2025. Mal manejo de fallos: mostrar **stack traces** completos al público, **Fail-Open** (si una validación falla por timeout, deja pasar asumiendo que "todo está bien"), tolerancia débil a payloads desbordantes → *crash*/DoS.
- **Casos:** Cloudflare "Cloudbleed" (2017, fuga de memoria); T-Mobile (2021, APIs que filtraban config interna al colapsar).
- **Contramedidas:** **mensajes de error genéricos** (nunca stack traces en prod; devolver "error con ID X"), registrar el detalle **internamente**, **Fail-Secure** (ante fallo, **negar** no saltar la validación), límites de tamaño de payload.

---

## DevSecOps — atar todo con DevOps
> [!quote] CAMS aplicado a seguridad (del material de la [[01 - Introducción a DevOps|Unidad 1]])
> - **Cultura:** la seguridad es **responsabilidad de todos** (dev, sec, ops), no de *gatekeepers* finales.
> - **Automatización:** *scanning* automático en **cada etapa** (SAST, dependencias, contenedores, IaC, runtime).
> - **Medición:** MTTR de vulnerabilidades, cobertura de scanning, *policy compliance*.
> - **Compartir:** *threat hunting*, post-mortems *blameless*, *training* continuo.
>
> Principios: **Shift Left** (detectar antes de producción), *Automated Compliance* (políticas sin fricción), *Speed ≠ Insecurity* (la automatización lo permite), seguridad como **habilitador**, no bloqueante.

> [!summary] Cierre de la materia
> La seguridad no es una etapa final sino una **práctica transversal** que se apoya en todo lo visto: escaneos en el [[03 - CI-CD|pipeline]], imágenes versionadas y secretos bien gestionados en el [[04 - Deployment|deployment]], logging/alertas para detección en [[05 - Monitoreo y Observabilidad|monitoreo]], y configuración auditable vía [[06 - Terraform (Infraestructura como Código)|IaC]]. Volver al [[00 - Índice DevOps|índice]].
