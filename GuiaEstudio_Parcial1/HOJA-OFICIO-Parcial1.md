# HOJA OFICIO — Parcial 1

## LO QUE CASI SIEMPRE CAE

- La que **NO** es transparencia: **versionado**
- La semántica RPC que evita duplicados: **at-most-once**
- El método HTTP que **NO** es idempotente: **POST**
- El timeout sirve para: **detectar fallos**
- La disponibilidad se mide con: **uptime**
- Máximo control para escalar: **código de la aplicación**
- Mínimo impacto: **ancho de banda e Internet**
- Lo que **NO** es beneficio de la nube: **alta latencia**
- Error clásico de diseño: **creer que la latencia es cero y la red confiable**
- Lo que un MOM **NO** hace: **control de versiones**
- El middleware aporta: **transparencia de comunicación, heterogeneidad y localización**
- El SPOF se reduce: **replicando el servidor**

> Cuando la pregunta diga **NO**, subráyalo. Ahí es donde se pierden puntos fáciles.

---

## CLASE 1 — REDES

Una red existe para dos cosas: compartir datos y compartir hardware.

- Tamaños: PAN (bluetooth) → LAN (tu casa) → WAN (Internet)
- Capas: Aplicación → Transporte → Internet → Acceso a la red. **OSI tiene 7 y el middleware está en la 5 y la 6.**
- Para ser cliente: internet, IP privada, gateway y DNS (te lo da el DHCP solo). **Para ser servidor, además: IP fija, el servicio prendido con su puerto y el firewall abierto.**
- IPv4 = 32 bits, ya se acabaron. IPv6 = 128 bits.
- Privadas (gratis, solo tu red): 10.x, 172.16–31.x, 192.168.x. Lo demás es pública.
- Gateway = la portería del conjunto: adentro cada uno tiene su número, afuera hay una sola dirección. Eso es NAT.
- DNS = la agenda del celular: le das el nombre, te da el número.
- Puertos: 22 SSH · 53 DNS · **80 HTTP** · **443 HTTPS** · 25 SMTP · 3389 escritorio remoto
- **AAA**: autenticación = *quién eres* · autorización = *qué puedes hacer* · accounting = *qué hiciste*

---

## CLASE 2 — NUBE

Usar el computador de otro, por internet, pagando solo lo que usas.

**Las 5 características:** me sirvo solo · desde cualquier lado · recursos compartidos · crece y se encoge solo · todo se mide (por eso se cobra por uso).

**Ventajas:** menos costo, escala, el proveedor administra, alta disponibilidad.
**Desventajas:** dependes de internet, pierdes control, quedas amarrado al proveedor, mover datos entre regiones cuesta.

**Dónde vive:**
- Privada = casa propia (control total, cara, latencia baja)
- Pública = hotel (barata, elástica, poco control)
- Híbrida = tu casa + hotel cuando llegan visitas

**Cuánto te dan** (analogía de la pizza):
- **IaaS** = masa prehecha → te dan máquinas. Para **administradores**. *Migrar.*
- **PaaS** = pizza a domicilio → te dan la plataforma. Para **desarrolladores**. *Desarrollar.*
- **SaaS** = vas a la pizzería → te dan la app lista. Para el **usuario final**. *Consumir.*

Control: IaaS alto → SaaS bajo. Facilidad: al revés.

---

## CLASE 3 — SISTEMAS DISTRIBUIDOS

Muchos computadores separados que se mandan mensajes y **se ven como uno solo**.

**Los 4 problemas:** todos trabajan al tiempo · no hay un reloj común · cada pieza puede caerse sola · no comparten memoria.

**Las 8 transparencias** (qué le esconden al usuario):

| | esconde |
|---|---|
| acceso | cómo se guardan y se llega a los datos |
| ubicación | dónde está |
| migración | que se puede mover |
| reubicación | que se mueve mientras lo usas |
| replicación | cuántas copias hay |
| concurrencia | que otros lo usan al tiempo |
| falla | que se cayó y se recuperó |
| persistencia | si está en memoria o en disco |

**Versionado no es transparencia. Escalabilidad tampoco.**
Balanceador → transparencia de **acceso**. OneDrive → ubicación, movilidad y acceso.

**Escalabilidad:** seguir funcionando bien cuando crece la gente.
- Horizontal = más máquinas detrás de un balanceador
- CDN escala porque pone copias cerca del usuario y baja el tiempo de ida y vuelta
- Timeout muy corto → retransmisiones y más carga. Muy largo → recursos ocupados.

**Las falacias (lo que uno cree y es mentira):** que la red es confiable, segura y no cambia; que la latencia es cero; que el ancho de banda es infinito; que mandar datos es gratis.

**Flynn:** **SIMD = una instrucción sobre muchos datos a la vez** (la GPU). **MIMD = ahí viven los sistemas distribuidos.**

---

## CLASE 4 — ARQUITECTURAS

> La frase clave: acoplamiento, SPOF, escalabilidad y comunicación **no se arreglan escribiendo mejor código. Son problemas de arquitectura.**

**SPOF** = la pieza que si se cae, tumba todo (el único ascensor del edificio). Se arregla **replicando**.

**Cliente-servidor:** roles fijos, el cliente siempre inicia, el servidor espera. El servidor es un SPOF natural.

**Los 4 estilos:**
1. **Capas** — interfaz → lógica → datos, cada capa solo habla con la vecina
2. **SOA / microservicios** — piezas independientes; si una cae, las otras siguen
3. **Publish-subscribe** — como una emisora: el que habla no sabe quién lo oye, y no espera a nadie
4. **REST** — todo es un recurso con URL y el servidor no guarda nada tuyo

**HTTP:** sin estado ✅ · asimétrico ✅ · bloqueante ✅ · capa de Aplicación ✅
No es de Transporte, no es asincrónico, no es binario.
> Sale **dos veces en el mismo parcial**: si está la opción "sin estado", esa. Si dice "CON estado" (falso), entonces la buena es **"asimétrico"**.

**Por qué stateless escala:** cada petición trae toda la información, entonces cualquier servidor puede atenderla. Como un cajero que no te recuerda: incómodo, pero puedes poner 100 cajeros.

**Idempotente** = hacerlo una vez o cincuenta da el mismo resultado.
**GET, PUT, DELETE sí. POST no** (mandas el pedido 3 veces, llegan 3 pizzas).

**n-tier:** cliente + servidor de aplicaciones + base de datos. **La capa del medio hace la lógica del negocio.**

---

## CLASE 5 — MIDDLEWARE

El middleware es el **intérprete**: dos aplicaciones que no se entienden hablan a través de él y cada una cree que habla su propio idioma.

Hace tres cosas: **esconde que las máquinas son distintas**, arma el modelo del sistema y **te da una API**. Vive en las **capas 5 y 6 de OSI**.

**Tipos:** RPC (gRPC, CORBA, REST) · **MOM** (Kafka, RabbitMQ, MQTT) · TOM (ODBC, ORM)

### Cola vs tópico — la que más cae

- **Cola** = la fila de tareas de la oficina. Cada mensaje lo toma **un solo** trabajador. Sirve para **repartir trabajo**.
- **Tópico** = el altavoz de la oficina. **Todos** oyen lo mismo. Sirve para **avisar**.

Respuestas hechas:
- Papel de la cola: **guarda el mensaje hasta que alguien lo procese**
- Ventaja de las colas: **mandar y olvidarse** (*fire-and-forget*)
- Ventaja del publish/subscribe: **todos reciben cada mensaje**
- Qué permite MOM: **desacoplar el rendimiento** (si el que consume va lento, la cola aguanta y el que produce no se frena)
- Lo que MOM **no** hace: **control de versiones**

**Síncrono vs asíncrono:**
- **HTTP y RPC = llamada telefónica.** Los dos tienen que estar ahí y tú esperas colgado.
- **MOM = correo.** Mandas y sigues con tu vida.

---

## CLASE 6 — RPC

Llamar una función que está en otro computador **como si fuera local**. El que llama **se queda bloqueado** esperando.

**Las piezas:**
- **Stub del cliente** → **traduce la llamada local en un mensaje de red** ← esta es la respuesta que preguntan
- **Stub del servidor** → desempaca y ejecuta
- **Marshalling** → empacar los datos para mandarlos
- **IDL** → el contrato, en un lenguaje que no es de nadie

Es el traductor diplomático: tú hablas normal, alguien traduce, del otro lado alguien destraduce, y tú nunca supiste que hubo traducción.

**Paso de datos:**
- Números y letras → **por valor**
- Arreglos → se copian
- **En RPC NO hay paso de punteros** (una dirección de memoria de una máquina no significa nada en la otra)

Hay que acordar un formato común porque las máquinas guardan los bytes en orden distinto. Es como la fecha: 24/08 aquí, 08/24 allá.

**Binding:** el servidor se registra al arrancar en un directorio (**el binder**, como las páginas amarillas) y el cliente le pregunta dónde está. En Linux es **rpcbind**, puerto 111.

**Semánticas** (te caes de internet mandando una transferencia):
- **at-least-once** → reintenta hasta que responda, **puede cobrarte dos veces**
- **at-most-once** → el servidor detecta que ya llegó y lo ignora. **Evita duplicados** ✅
- exactly-once → el ideal, casi imposible

**Desafío común de RPC:** **manejar los fallos de red durante la llamada.**

---

# RESPUESTAS DE PARCIALES PASADOS

**Generales**
1. NO es transparencia → **versionado**
2. Balanceador → **transparencia de acceso**
3. OneDrive → **ubicación, movilidad y acceso**
4. "Quien dice ser, es" → **autenticación**
5. Timeout → **detectar fallos**
6. Disponibilidad → **uptime**
7. Error de diseño → **latencia despreciable y red confiable**
8. Distribuir no es opcional → **hay varias organizaciones o ubicaciones**
9. SIMD → **una instrucción sobre múltiples conjuntos de datos**

**Escalabilidad**
10. Máximo control → **código de la aplicación**
11. Mínimo impacto → **ancho de banda e Internet**
12. Timeout corto → **aumenta la carga por retransmisiones**
13. Escalar horizontal → **más servidores detrás de un balanceador**
14. CDN → **replica cerca del usuario y baja el RTT**
15. Pérdida de escalabilidad → **IPv4**
16. Dividir y vencer → **MapReduce**
17. Las colas apoyan → **rendimiento**

**Nube**
18. NO es beneficio → **alta latencia**

**Arquitecturas**
19. Reducir SPOF → **replicar el servidor**
20. Capa del medio → **lógica de negocio y coordinación**
21. Middleware aporta → **comunicación, heterogeneidad y localización**
22. HTTP → **sin estado** (o **asimétrico** si la otra opción dice "con estado")
23. Stateless escala porque → **cada petición trae toda la información**
24. NO idempotente → **POST**
25. Idempotencia no sirve en → **operaciones de solo lectura**

**MOM**
26. Comunicación asincrónica → **colas**
27. Papel de la cola → **guarda el mensaje hasta que lo procesen**
28. Ventaja de las colas → **envío y olvido** (si no está, "garantizan la entrega"; nunca marques la de "múltiples suscriptores", esos son tópicos)
29. Publish/subscribe → **todos reciben cada mensaje**
30. MOM permite → **desacoplar el rendimiento**
31. MOM NO hace → **control de versiones**
32. Nodos y clientes → **MOM con COLAS**: cada cliente deja una tarea, cada nodo toma una. *Con tópicos todos harían lo mismo.*

**RPC**
33. Evita duplicados → **at-most-once**
34. El stub → **traduce llamadas locales en mensajes de red**
35. Desafío de RPC → **fallos de red durante la llamada**
36. A no recibe respuesta → **poner un timeout**

**Las dos abiertas**

**37. ¿Puede haber dos métodos con el mismo nombre en un servidor RPC?**
No. Cada procedimiento se identifica con número de programa, versión y número de procedimiento, no con el nombre y los tipos como en Java. El servidor necesita un identificador único; con dos nombres iguales no sabría a cuál llamar. Toca ponerles nombres distintos.

**38. ¿La `union` de C afecta a RPC?**
Sí, y es grave. Para empacar un dato hay que saber exactamente de qué tipo es. En una `union` no hay forma segura de saber cuál de las alternativas está guardada, entonces no se sabe cómo mandarla ni cómo leerla del otro lado, y llegan datos corruptos. Por eso los IDL exigen uniones con una etiqueta que diga qué contienen.

**39. Amdahl** — 70% paralelizable, quiero speedup 2.5:
`1/2.5 = 0.4` → `1−0.7 = 0.3` → `0.4−0.3 = 0.1` → `0.7/0.1 =` **7 cores**

---

## LAS TRAMPAS

- **autenticación / autorización** → quién eres / qué te dejo hacer
- **escalabilidad / elasticidad** → aguantar más / ajustarse solo
- **disponibilidad / rendimiento** → uptime / tiempo de respuesta
- **cola / tópico** → uno solo consume / todos reciben
- **at-least-once / at-most-once** → puede duplicar / evita duplicar
- **stub / skeleton** → cliente empaca / servidor desempaca
- **migración / reubicación** → se puede mover / se mueve mientras lo usas
- **SPOF / cuello de botella** → si cae todo cae / lo que frena
