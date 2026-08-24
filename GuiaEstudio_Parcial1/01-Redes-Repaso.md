# Presentación 1 — Repaso de Redes

> **La idea en una frase:** antes de hablar de "la nube" hay que entender cómo dos computadores se hablan entre sí. Esta clase es ese repaso.

---

## 1. ¿Para qué sirve una red?

Solo por dos razones, y todo lo demás sale de ahí:

- **Compartir datos** (un archivo, una base de datos, un correo)
- **Compartir hardware** (una impresora, un servidor, capacidad de cómputo)

> 🧠 **Analogía:** una red es como el sistema de tuberías de un edificio. No importa si por ahí va agua o gas: lo importante es que conecta apartamentos que antes estaban aislados.

---

## 2. Tipos de redes

### Por alcance geográfico

| Tipo | Qué tan grande es | Ejemplo cotidiano |
|---|---|---|
| **PAN** (Personal Area Network) | Unos pocos metros | Tu celular con tus audífonos Bluetooth |
| **LAN** (Local Area Network) | Un edificio | El WiFi de tu casa o de la universidad |
| **WAN** (Wide Area Network) | Ciudades, países | Internet |

### Por el rol del host

- **Cliente** → el que *pide* un servicio (tu navegador)
- **Servidor** → el que *responde* con el servicio (el computador de Google)

> ⚠️ Ojo: "cliente" y "servidor" son **roles**, no máquinas. Un mismo computador puede ser cliente de uno y servidor de otro al mismo tiempo.

---

## 3. Modelos de capas (OSI y DoD/TCP-IP)

Para no volver un caos la comunicación, se divide en **capas**. Cada capa solo se preocupa por su trabajo y le entrega el resultado a la siguiente.

> 🧠 **Analogía del correo postal:**
> - Tú escribes la carta (**Aplicación**)
> - La metes en un sobre y decides si quieres confirmación de entrega (**Transporte**)
> - Escribes la dirección de destino (**Internet / Red**)
> - El cartero decide por qué calles ir (**Acceso a la red**)
>
> Tú nunca hablas con el cartero. Cada capa confía en la de abajo.

### Modelo DoD / TCP-IP (el que usó el profe)

| Capa DoD | Qué hace | Ejemplos |
|---|---|---|
| **Aplicación** | Lo que el usuario ve y usa | HTTP, DNS, SMTP, FTP |
| **Transporte** | Entrega confiable o rápida, puertos | TCP, UDP |
| **Internet** | Direccionamiento y ruteo | IP, ICMP |
| **Acceso a la red** | El cable, el WiFi, las tarjetas | Ethernet, WiFi |

**El modelo OSI** tiene 7 capas: Física, Enlace, Red, Transporte, **Sesión**, **Presentación**, Aplicación.
👉 Guarda este dato: **el middleware vive en las capas 5 y 6 (Sesión y Presentación)**. Va a salir en la presentación 5.

---

## 4. ¿Qué necesito para enviar un paquete de un host a otro?

No basta con "tener la IP". La lista completa desde el **cliente**:

1. Un **ISP** (proveedor de internet), un **NOS** (sistema operativo de red) y una **interfaz de conexión** (tarjeta de red / WiFi)
2. Una **dirección IPv4 privada + máscara**, saliendo por **NAT** hacia una IP pública (normalmente dinámica)
3. Una **pasarela / gateway** (la puerta de salida de tu red)
4. Un **servidor DNS** (para traducir nombres a IPs)

> 💡 Normalmente todo esto lo configura solo el **DHCP** que da tu router. Por eso conectas el celular al WiFi y "simplemente funciona".

### Y si yo quiero *ofrecer* un servicio (ser servidor)?

Todo lo anterior **más**:

5. Una **IP fija** (pública, o privada con NAT fijo) — porque si tu dirección cambia, nadie te encuentra
6. El **servicio corriendo**, identificado por **nombre y puerto**
7. Los **puertos de entrada abiertos en el firewall**

> 🧠 **Analogía:** para *pedir* una pizza solo necesitas un teléfono. Para *vender* pizzas necesitas además una dirección fija, un local abierto y que la puerta no esté con candado.

---

## 5. Direcciones IP

### IPv4 vs IPv6

| | IPv4 | IPv6 |
|---|---|---|
| Ejemplo | `123.255.16.100` | `FE62:2166:FF00:0000:0126:1161:FC82:FFF1` |
| Tamaño | 32 bits | 128 bits |
| Cuántas hay | ~4.300 millones (2³²) | 3.4×10³⁸ (2¹²⁸) |
| Problema | **Ya se agotaron** | Prácticamente infinitas |

👉 IPv6 nació porque IPv4 se quedó sin direcciones. Mientras tanto, el parche que usamos es **NAT**.

### Públicas vs privadas

| | Privadas | Públicas |
|---|---|---|
| Costo | **Gratis** | **Cuestan** (se alquilan al ISP) |
| Alcance | Solo dentro de la LAN | Visibles desde todo Internet |
| Riesgo | Protegidas | **Se exponen** al mundo |

**Rangos privados que debes reconocer:**

| Rango | Cuántas direcciones |
|---|---|
| `10.0.0.0` – `10.255.255.255` | ~16 millones |
| `172.16.0.0` – `172.31.255.255` | ~1 millón |
| `192.168.0.0` – `192.168.255.255` | ~65 mil |

> ✅ Truco de parcial: si te muestran `8.8.8.8`, `1.1.1.1`, `200.16.86.5` o `52.95.110.1` → son **públicas**. Si empieza por 10, 172.16–31 o 192.168 → **privada**.

**Dato importante:** una máquina puede tener **varias interfaces de red**, cada una con su propia IP y máscara.

### Máscara de red / CIDR

La máscara dice **qué parte de la IP identifica la red** y qué parte identifica al equipo.

| Máscara | CIDR | Significado |
|---|---|---|
| `255.0.0.0` | `/8` | 8 bits para la red |
| `255.255.0.0` | `/16` | 16 bits para la red |
| `255.255.255.0` | `/24` | 24 bits para la red |
| `255.255.255.248` | `/29` | 29 bits (quedan 3 bits → 8 direcciones, 6 usables) |

> 🧠 **Analogía:** la IP es una dirección tipo "Calle 50 #12-34". La máscara dice hasta dónde llega "el barrio" y dónde empieza "la casa".

---

## 6. Gateway (pasarela / puerta de enlace)

- Es el **host que conecta redes con protocolos o arquitecturas diferentes**.
- En la práctica: el equipo de tu LAN que tiene IPs privadas y comparte **una única IP pública** para dar Internet a todos.

> 🧠 **Analogía:** la portería de un conjunto residencial. Todos los apartamentos tienen número interno (IP privada), pero hacia la calle el conjunto tiene **una sola dirección** (IP pública). El portero traduce (**eso es NAT**).

---

## 7. DNS

- Sistema de **resolución de nombres**: le das un nombre, te devuelve la IP.
- `google.com` → `142.250.x.x`
- **Antes de DNS** existía un archivo plano: `/etc/hosts` (Linux) o `HOSTS.TXT`. En UNIX todavía se usa, sobre todo para hosts locales.

> 🧠 **Analogía:** DNS es la agenda de contactos del teléfono. Tú marcas "Mamá", no el número. Si la agenda falla, el número sigue existiendo — pero tú ya no sabes cuál es.

---

## 8. Puertos comunes (¡memorízalos!)

| Puerto | Servicio | | Puerto | Servicio |
|---|---|---|---|---|
| 20, 21 | FTP | | 123 | NTP |
| 22 | SSH | | 139 | NetBIOS |
| 23 | Telnet | | 143 | IMAP |
| 25 | SMTP | | 161, 162 | SNMP |
| 53 | DNS | | 389 | LDAP |
| 80 | **HTTP** | | 443 | **HTTPS** |
| 110 | POP3 | | 465 | SMTPS |
| 993 | IMAPS | | 995 | POP3S |
| 636 | LDAPS | | 3389 | RDP (escritorio remoto) |

> 💡 Regla mental: casi siempre, **la versión segura (con S) tiene otro puerto**: HTTP 80 → HTTPS 443; IMAP 143 → IMAPS 993; POP3 110 → POP3S 995.
> Para ver la lista completa en Linux: `/etc/services` o el comando `whatportis`.

---

## 9. AAA (Accounting, Autenticación, Autorización)

Un sistema es **AAA** si garantiza las tres cosas:

| Concepto | Pregunta que responde | Resumen |
|---|---|---|
| **Autenticación** | *¿Quién eres?* | Verifica la identidad de quien pide acceso (usuario + contraseña) |
| **Autorización** | *¿Qué puedes hacer?* | Define los permisos: leer, escribir, borrar |
| **Accounting** (registro/auditoría) | *¿Qué hiciste?* | Registra las acciones de cada usuario para seguimiento y auditoría |

> 🧠 **Analogía del hotel:**
> - **Autenticación** = muestras la cédula en recepción
> - **Autorización** = tu tarjeta abre la 305 y el gimnasio, pero no la 306
> - **Accounting** = el sistema guarda a qué hora entraste y qué consumiste del minibar

> ⚠️ **Pregunta clásica de parcial:** *"La garantía de que una ejecución la efectúa aquella entidad o persona que asegura ejecutarla es..."* → **Autenticación**.
> (Salió en dos parciales distintos. No la falles.)

---

## 10. Puertos en modo LISTEN y seguridad

- Un servicio "a la escucha" (**LISTEN**) es una puerta abierta.
- **Los servicios innecesarios en LISTEN se deben bajar**, porque cada puerto abierto es un riesgo de seguridad.

### Herramientas

| Comando | Para qué | Local o remoto |
|---|---|---|
| `netstat -tulpan` | Ver puertos en LISTEN | Local |
| `ss -tulpan` | Igual, versión moderna | Local |
| `nmap <IP>` | Escanear puertos de otra máquina | **Remoto** |
| `ipconfig /all` / `ifconfig` | Ver mi configuración de red | Local |
| `ping -c 2 k8s.io` | ¿Está vivo el host? | Remoto |
| `traceroute k8s.io` | Por qué camino viajan mis paquetes | Remoto |
| `dig kubernetes.io` | Consultar DNS | Remoto |
| `telnet host puerto` | Probar si un puerto responde | Remoto |

---

## 11. Servicios de TI (las 3 patas)

Todo servicio informático se reduce a tres recursos. Recuérdalos, porque la nube vende exactamente esto:

- **Procesamiento** (CPU)
- **Almacenamiento** (disco)
- **Red** (conectividad)

---

## 📊 Tabla resumen — Presentación 1

| Tema | Lo mínimo que debes saber |
|---|---|
| **Para qué una red** | Compartir datos y compartir hardware |
| **Tipos por alcance** | PAN (personal) < LAN (edificio) < WAN (mundo/Internet) |
| **Modelo DoD/TCP-IP** | Aplicación → Transporte → Internet → Acceso a la red |
| **Modelo OSI** | 7 capas; **middleware = capas 5 (Sesión) y 6 (Presentación)** |
| **Cliente necesita** | ISP + NOS + interfaz, IP privada + máscara + NAT, Gateway, DNS (todo vía DHCP) |
| **Servidor necesita** | Lo anterior + IP fija + servicio corriendo (nombre y puerto) + firewall abierto |
| **IPv4** | 32 bits, ~4.300 millones, ya agotadas |
| **IPv6** | 128 bits, 3.4×10³⁸ direcciones |
| **IP privadas** | 10.x, 172.16–31.x, 192.168.x — gratis, solo LAN |
| **IP públicas** | Cuestan dinero, visibles desde Internet |
| **Máscara/CIDR** | Separa "parte de red" de "parte de host". /8, /16, /24 |
| **Gateway** | Conecta redes distintas; comparte una IP pública vía NAT |
| **DNS** | Nombre → IP. Antes era `/etc/hosts` |
| **Puertos clave** | 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 25 SMTP, 3389 RDP |
| **AAA** | Autenticación (quién eres) · Autorización (qué puedes) · Accounting (qué hiciste) |
| **Seguridad** | Bajar servicios innecesarios en LISTEN; auditar con `ss`/`netstat`/`nmap` |
| **Servicios TI** | Procesamiento – Almacenamiento – Red |

---

## 📖 Glosario — Presentación 1

| Palabra | En cristiano |
|---|---|
| **Host** | Cualquier equipo conectado a la red (PC, servidor, celular) |
| **Paquete** | Pedacito en el que se parte un mensaje para viajar por la red |
| **Protocolo** | Conjunto de reglas que dos equipos acuerdan para entenderse |
| **PAN / LAN / WAN** | Red personal / red local / red amplia (Internet) |
| **ISP** | *Internet Service Provider*: la empresa que te vende internet (Claro, Tigo…) |
| **NOS** | *Network Operating System*: el sistema operativo con capacidad de red |
| **Interfaz de red** | La tarjeta (física o WiFi) por donde sale el tráfico. Una máquina puede tener varias |
| **IP** | La "dirección postal" de un equipo en la red |
| **IPv4 / IPv6** | Versión vieja (32 bits, agotada) / versión nueva (128 bits) |
| **IP pública** | Direccción visible desde Internet. Cuesta dinero |
| **IP privada** | Dirección solo válida dentro de tu LAN. Gratis |
| **Máscara de red** | Dice qué parte de la IP es "la red" y qué parte es "el equipo" |
| **CIDR** | Forma corta de escribir la máscara: `/24` en vez de `255.255.255.0` |
| **NAT** | *Network Address Translation*: truco para que muchas IPs privadas salgan por una sola IP pública |
| **Gateway / Pasarela** | El equipo que conecta tu red con otra (típicamente el router hacia Internet) |
| **DHCP** | Servicio que reparte automáticamente IP, máscara, gateway y DNS |
| **DNS** | Traductor de nombres (`google.com`) a IPs |
| **`/etc/hosts`** | Archivo local de traducción nombre→IP. El abuelo del DNS |
| **Puerto** | Número que identifica *qué servicio* dentro de un host (80 = web) |
| **LISTEN** | Estado de un puerto que está esperando conexiones (una "puerta abierta") |
| **Firewall** | Filtro que decide qué tráfico entra y sale |
| **AAA** | Accounting + Autenticación + Autorización |
| **Autenticación** | Comprobar **quién eres** |
| **Autorización** | Definir **qué te dejo hacer** |
| **Accounting** | **Registrar** lo que hiciste (auditoría) |
| **`nmap`** | Herramienta para escanear puertos de una máquina remota |
| **`netstat` / `ss`** | Herramientas para ver los puertos abiertos localmente |
