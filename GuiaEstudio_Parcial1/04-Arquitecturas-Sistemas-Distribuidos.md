# Presentación 4 — Arquitecturas de Sistemas Distribuidos

> **Referencia:** Capítulo 2 — *Distributed Systems, 4th ed.* (Tanenbaum & Van Steen)
>
> **La idea en una frase:** la arquitectura es el **plano del edificio**. Decide cosas que después **no se arreglan escribiendo mejor código**.

---

## 1. ¿Qué es una arquitectura?

> Una arquitectura es **"la organización fundamental de un sistema"**, definida por:

| Elemento | Qué significa |
|---|---|
| Sus **componentes reemplazables** (*building blocks*) | La **organización lógica y física** del sistema |
| Las **relaciones** entre componentes (conectores, interfaces, dependencias) y su entorno | **Cómo interactúan y se comunican** |
| Los **principios que guían su diseño** (reglas y restricciones) y su evolución | Facilita el **manejo de la complejidad y la escalabilidad** |

> 🧠 **Analogía:** los ladrillos son los componentes, el cemento y las puertas son los conectores, y las normas de construcción son los principios de diseño. Puedes pintar bonito (buen código), pero si el plano puso una sola escalera para 20 pisos, ese problema no se pinta: hay que rediseñar.

---

## 2. ⭐ Propiedades estructurales (la idea más importante de la clase)

La arquitectura define propiedades que **NO pueden corregirse solo optimizando el código**:

- **Acoplamiento** — qué tan pegadas están las piezas entre sí
- **Puntos únicos de fallo (SPOF)** — si esa pieza cae, cae todo
- **Escalabilidad** — hasta dónde puede crecer
- **Comunicación** — cómo se hablan las piezas

> ⚠️ **Esta es la frase de parcial:** *"Estas propiedades no son un problema de implementación, son un problema de arquitectura."*

### SPOF (Single Point Of Failure)

Un **punto único de fallo** es un componente que, si falla, **tumba todo el sistema**.

> 🧠 **Analogía:** un edificio con un solo ascensor y sin escaleras. Si el ascensor se daña, nadie sube. La solución no es "aceitarlo mejor", es **poner otro ascensor** (replicación).

> ⚠️ **Pregunta de parcial:** *"¿Cuál arquitectura reduce el impacto del SPOF en Cliente/Servidor?"* → **Replicación del servidor.**

---

## 3. Importancia de la arquitectura en SD

- Define la **organización lógica y física** de los componentes distribuidos
- Define **cómo interactúan y se comunican** los procesos
- Facilita el **manejo de la complejidad y la escalabilidad**

---

## 4. Arquitectura Cliente-Servidor

- El **cliente solicita** servicios; el **servidor los procesa y responde**.
- Protocolos: **request-reply** (petición-respuesta), sobre conexión **confiable** (TCP) o **no confiable** (UDP).
- Ejemplos: **NFS**, **HTTP clásico**.

> El profe insistió: *"¿Qué caracteriza a una arquitectura Cliente/Servidor desde el punto de vista **arquitectónico**, no tecnológico?"*
>
> **Respuesta:** los **roles son asimétricos y fijos**. Uno pide, el otro responde; la comunicación la **inicia siempre el cliente**; el servidor es pasivo y espera.

> 🧠 **Analogía:** un restaurante. El cliente pide, el mesero trae. El mesero nunca aparece en tu casa a ofrecerte comida por iniciativa propia.

**Debilidad:** el servidor es un **SPOF** natural y un cuello de botella.

---

## 5. Estilos de arquitectura (los 4 del curso)

1. Arquitectura **por Capas**
2. Arquitectura **Orientada a Servicios** (SOA y Microservicios)
3. Arquitectura **Publish-Subscribe**
4. Arquitectura **RESTful** y basada en recursos

---

### 5.1 Arquitectura por Capas

- Organiza los componentes en **capas lógicas**: UI (interfaz) → lógica de negocio → datos
- Cada capa **solo habla con la de al lado**
- Ejemplo clásico: la **pila de protocolos TCP/IP**
- Beneficio: **modularidad** y **reemplazo de componentes** sin tocar el resto

> 🧠 **Analogía:** una torta de pisos. Puedes cambiar el sabor de un piso sin desarmar toda la torta.

---

### 5.2 SOA y Microservicios

| | **SOA** | **Microservicios** |
|---|---|---|
| Idea | **Composición de servicios independientes** | **Servicios pequeños y desplegables por separado** |
| Tamaño | Servicios grandes, de negocio | Servicios muy pequeños, de una responsabilidad |
| Despliegue | Suele ser conjunto | **Cada uno se despliega solo** |

Ambos **promueven escalabilidad y mantenimiento**.

> 🧠 **Analogía:** un monolito es un restaurante donde un solo cocinero hace todo; si se enferma, no hay servicio. Los microservicios son una plaza de comidas: cada puesto es independiente, y si uno cierra, los demás siguen vendiendo.

---

### 5.3 Arquitectura Publish-Subscribe ⭐

- Los procesos **publican** eventos y **se suscriben** a eventos **sin conocerse entre sí**
- **Desacoplamiento temporal y referencial**
- Ideal para sistemas **escalables y dinámicos**

Los **dos desacoplamientos** (esto lo preguntan):

| Desacoplamiento | Qué significa |
|---|---|
| **Referencial** | **El emisor NO sabe quién lo escucha** (ni el receptor sabe quién publicó) |
| **Temporal** | **El emisor NO espera al receptor.** Pueden no estar conectados al mismo tiempo |

> 🧠 **Analogía:** una emisora de radio. El locutor habla sin saber quién está escuchando (referencial). Y si grabas el programa, lo escuchas mañana (temporal). Comparado con una llamada telefónica, donde ambos deben estar presentes y saben con quién hablan.

> **Contraste con HTTP:** HTTP es **bloqueante** — el cliente queda esperando hasta que aparezca la respuesta. Publish-Subscribe **no es bloqueante**.
>
> Cardinalidades: paso de mensajes clásico es **1:1**; publish-subscribe es **1:N (uno a muchos)**.

---

### 5.4 Arquitectura RESTful / basada en recursos

- Todo se modela como **recursos** con una URL
- Se manipulan con verbos HTTP: `GET`, `POST`, `PUT`, `DELETE`
- **Stateless**: el servidor **no guarda el estado de la sesión**
- Ejemplo moderno: la nube usa **RESTful y serverless**

#### Propiedades de HTTP que debes tener claras ⚠️

| Característica | ¿HTTP la tiene? |
|---|---|
| Protocolo **sin estado** (*stateless*) | ✅ **SÍ** ← respuesta típica de parcial |
| Protocolo **asimétrico** (cliente inicia, servidor responde) | ✅ SÍ |
| Protocolo de la capa de **Transporte** | ❌ NO — es de **Aplicación** |
| Protocolo **asincrónico** | ❌ NO — es **síncrono/bloqueante** |
| Codificación **binaria** | ❌ NO — HTTP/1.1 es **texto** |

> ⚠️ **Pregunta de parcial:** *"La propiedad de stateless en HTTP facilita la escalabilidad porque..."* → **Cada petición contiene toda la información necesaria, evitando dependencia de estado en el servidor.**
>
> 🧠 **Analogía:** un cajero que no recuerda nada de ti. Cada vez le tienes que mostrar la cédula. Es incómodo, ¡pero significa que **cualquier cajero** te puede atender! Por eso escala: puedes poner 100 cajeros y da igual a cuál llegues.

#### Idempotencia ⚠️

> Un método es **idempotente** si ejecutarlo **1 vez o N veces produce el mismo resultado**.

| Método | ¿Idempotente? | Por qué |
|---|---|---|
| `GET` | ✅ Sí | Leer 5 veces no cambia nada |
| `PUT` | ✅ Sí | Poner el valor "X" 5 veces deja el valor en "X" |
| `DELETE` | ✅ Sí | Borrar algo ya borrado lo deja borrado |
| **`POST`** | ❌ **NO** | Crear 5 veces → **crea 5 cosas distintas** |

> ⚠️ **Pregunta de parcial:** *"¿Cuál NO es idempotente?"* → **POST**
>
> *"¿En cuál escenario la idempotencia NO aporta beneficios claros?"* → **En operaciones de solo lectura** (porque leer ya es idempotente por naturaleza; no ganas nada nuevo).

> 🧠 **Analogía:** apagar el interruptor de la luz es idempotente (si ya está apagado y lo vuelves a apagar, sigue apagado). Pedir una pizza NO es idempotente (si mandas el pedido 3 veces, llegan 3 pizzas).

**¿Para qué sirve?** Para poder **reintentar con seguridad** cuando la red falla. Si no sabes si tu petición llegó, con un método idempotente simplemente la repites.

---

## 6. Middleware en Sistemas Distribuidos

*(Se profundiza en la Presentación 5, aquí solo la introducción)*

- Es una **capa entre el Sistema Operativo y las aplicaciones distribuidas**
- Ofrece **comunicación, seguridad, tolerancia a fallos**
- Facilita **transparencia y portabilidad**

> ⚠️ **Pregunta de parcial:** *"El uso de middleware en arquitecturas de SD aporta:"* → **Transparencia de comunicación, heterogeneidad y localización.**

### Patrones de Middleware

| Patrón | Qué hace | Analogía |
|---|---|---|
| **Wrappers / Adaptadores** | **Integran componentes heterogéneos** | Un adaptador de enchufe para viajar |
| **Interceptors** | **Modifican el flujo** para agregar comportamiento (logs, seguridad, reintentos) | Un filtro que revisa cada carta antes de que salga |
| **Middleware modificable** | Permite **adaptación en tiempo real** | Cambiar la ruta del bus sin bajar a los pasajeros |

---

## 7. Arquitectura Multinivel (n-Tier) ⭐

| Modelo | Composición | Ejemplo |
|---|---|---|
| **1-Tier** | Todo en la misma máquina | App de escritorio monolítica |
| **2-Tier** | **Cliente + Servidor** | Cliente pesado hablando directo con la BD |
| **3-Tier** | **Cliente + Servidor de Aplicaciones + Base de Datos** | Web clásica |
| **N-Tier** | Web y apps modernas: **múltiples capas + microservicios** | Arquitectura cloud actual |

### ¿De qué se encarga cada capa? ⚠️

| Capa | Responsabilidad |
|---|---|
| **Presentación** (cliente) | Renderizado de la interfaz gráfica |
| **Intermedia** (servidor de aplicaciones) | **Lógica de negocio y coordinación entre clientes y servidores de datos** ← respuesta de parcial |
| **Datos** | Almacenamiento físico de los datos |

> 🧠 **Analogía del restaurante 3-tier:**
> - **Cliente** = el comensal en la mesa
> - **Capa intermedia** = el mesero + el cocinero (aplican las reglas: "no hay pasta después de las 10")
> - **Capa de datos** = la despensa

---

## 8. Ejemplos de arquitecturas en la práctica

| Sistema | Arquitectura |
|---|---|
| **NFS** | Cliente-servidor para archivos |
| **Web** | Evolución de **2-tier → 3-tier → n-tier** |
| **Cloud** | **RESTful y serverless** |

---

## 9. Ejercicio conceptual que salió en parcial ⭐

> *"Tiene una app con un conjunto de NODOS de procesamiento y una serie de CLIENTES. Debe decidir si usa RPC o MOM para distribuir las cargas de trabajo. ¿Cuál es el mejor diseño?"*
>
> **Respuesta:** Utilizar un **MOM server configurando COLAS**, de tal manera que cada CLIENTE registre una Tarea y los NODOS accedan a una Tarea en la Cola.

**¿Por qué colas y no tópicos?**
- **Cola (queue)** = cada mensaje lo consume **UN SOLO** consumidor → perfecto para **repartir trabajo** (que cada nodo haga una tarea distinta)
- **Tópico (topic)** = **TODOS** los suscriptores reciben el mensaje → si usaras tópicos, **todos los nodos harían la misma tarea**. Mal.

> 🧠 **Analogía:** una cola es la fila de tareas de una oficina: cada empleado toma un papel distinto. Un tópico es el altavoz de la oficina: todos escuchan el mismo anuncio.

---

## 📊 Tabla resumen — Presentación 4

| Tema | Lo mínimo que debes saber |
|---|---|
| **Arquitectura** | Componentes reemplazables + relaciones/conectores + principios de diseño |
| **Propiedades estructurales** | **Acoplamiento, SPOF, Escalabilidad, Comunicación** — no se arreglan optimizando código |
| **SPOF** | Punto único de fallo. Se reduce con **replicación del servidor** |
| **Cliente-Servidor** | Roles asimétricos y fijos; request-reply; el cliente inicia. Ej: NFS, HTTP |
| **Por Capas** | UI → lógica → datos. Ej: pila TCP/IP. Da modularidad |
| **SOA / Microservicios** | Servicios independientes / servicios pequeños desplegables por separado |
| **Publish-Subscribe** | Publican y se suscriben **sin conocerse**. Desacoplamiento **temporal** y **referencial**. 1:N |
| **REST** | Recursos + verbos HTTP + **stateless** |
| **HTTP** | Sin estado, asimétrico, síncrono/bloqueante, capa de **Aplicación**, texto |
| **Stateless escala porque** | Cada petición trae toda la info → cualquier servidor puede atenderla |
| **Idempotencia** | GET, PUT, DELETE sí. **POST no** |
| **Middleware** | Capa entre SO y apps distribuidas. Aporta **transparencia de comunicación, heterogeneidad y localización** |
| **Patrones de middleware** | Wrappers/Adaptadores · Interceptors · Middleware modificable |
| **n-Tier** | 2-Tier: cliente+servidor · 3-Tier: cliente + app server + BD |
| **Capa intermedia** | **Lógica de negocio y coordinación** |
| **Colas vs Tópicos** | Cola = un solo consumidor (repartir trabajo) · Tópico = todos reciben (difundir) |

---

## 📖 Glosario — Presentación 4

| Palabra | En cristiano |
|---|---|
| **Arquitectura** | El plano del sistema: qué piezas hay, cómo se conectan y bajo qué reglas |
| **Componente / Building block** | Una pieza reemplazable del sistema |
| **Conector** | El mecanismo por el que dos componentes se comunican |
| **Interfaz** | El "contrato" público de un componente: qué ofrece y cómo se le pide |
| **Acoplamiento** | Qué tan dependientes son las piezas entre sí. Menos acoplamiento = mejor |
| **SPOF** | *Single Point Of Failure*: si esa pieza cae, cae todo |
| **Replicación** | Tener copias de un componente para que no haya SPOF |
| **Cliente-Servidor** | El cliente pide, el servidor responde. Roles fijos |
| **Request-reply** | Patrón petición → respuesta |
| **NFS** | *Network File System*: compartir archivos por red, ejemplo clásico cliente-servidor |
| **Arquitectura por capas** | Organizar el sistema en niveles donde cada uno solo habla con el vecino |
| **SOA** | *Service Oriented Architecture*: sistema compuesto por servicios independientes |
| **Microservicios** | Servicios muy pequeños, cada uno desplegable por separado |
| **Monolito** | Todo el sistema en un solo bloque de código desplegado junto |
| **Publish-Subscribe** | Unos publican eventos, otros se suscriben, sin conocerse |
| **Desacoplamiento referencial** | El emisor **no sabe quién** lo escucha |
| **Desacoplamiento temporal** | El emisor **no espera** al receptor; no tienen que coincidir en el tiempo |
| **Evento** | Un aviso de que algo pasó ("se creó un pedido") |
| **Tópico (topic)** | Canal de publish-subscribe: **todos** los suscriptores reciben el mensaje |
| **Cola (queue)** | Canal donde **un solo** consumidor toma cada mensaje. Sirve para repartir trabajo |
| **REST / RESTful** | Estilo donde todo es un recurso con URL y se usa GET/POST/PUT/DELETE |
| **Stateless** | El servidor no guarda el estado de la sesión entre peticiones |
| **Stateful** | El servidor sí recuerda el estado de cada cliente |
| **Idempotente** | Ejecutarlo una o mil veces da el mismo resultado |
| **GET / PUT / DELETE / POST** | Leer / reemplazar / borrar / crear. **Solo POST no es idempotente** |
| **Serverless** | Modelo donde subes funciones y no administras servidores |
| **Middleware** | Capa intermedia que conecta apps distribuidas y esconde la complejidad |
| **Wrapper / Adaptador** | Pieza que "envuelve" un componente incompatible para poder usarlo |
| **Interceptor** | Pieza que se mete en el flujo para agregar comportamiento (log, seguridad) |
| **n-Tier / Multinivel** | Arquitectura dividida en N niveles (cliente, app, datos, …) |
| **Capa de presentación** | La interfaz que ve el usuario |
| **Capa de lógica de negocio** | Donde viven las reglas del negocio |
| **Capa de datos** | Donde se guardan los datos (la BD) |
| **Cliente pesado / liviano** | Cliente que hace mucho procesamiento / que casi solo muestra la pantalla |
