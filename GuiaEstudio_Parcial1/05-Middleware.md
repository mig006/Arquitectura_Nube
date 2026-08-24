# Presentación 5 — Comunicaciones y Middleware

> **La idea en una frase:** el middleware es el **traductor y mensajero** que se pone en medio de dos aplicaciones para que puedan hablar sin importar en qué lenguaje, sistema operativo o máquina estén.

---

## 1. ¿Cómo se comunican los procesos distribuidos?

Dos grandes caminos:

### A. Comunicación mediante **protocolos** (nivel bajo, "a mano")
- **Sockets**
- **HTTP**

### B. Comunicación mediante **middleware** (nivel alto, "con ayuda")
- **RPC**
- **MOM** (Message-Oriented Middleware)
- **Eventos**
- Otros

> 🧠 **Analogía:** con sockets tú mismo escribes la carta, la metes al sobre, buscas la estampilla y vas al correo. Con middleware, le entregas el paquete a una empresa de mensajería y ellos se encargan de todo.

---

## 2. ¿Qué es un Middleware? ⭐

> **La comunicación entre objetos / componentes / programas que están distribuidos a través de una red es habilitada por un MIDDLEWARE.**

Sus tres funciones clave (memoriza esta tripleta, es pregunta de parcial):

1. **Enmascara la heterogeneidad** — hace que da igual el SO, el lenguaje o el hardware
2. **Mapea un modelo de Sistema Distribuido**
3. **Provee un modelo de programación de aplicaciones (API)**

**Ubicación:**
- Normalmente está **entre la Aplicación y el Sistema Distribuido**
- Cubre los **niveles 5 y 6 del modelo OSI**: **Sesión** y **Presentación**

> ⚠️ Esto de "capas 5 y 6 de OSI" es un dato muy concreto y muy preguntable.

### Definición alternativa (más simple)

> Término usado para referirse a los **componentes de software que actúan como intermediarios entre otros componentes de software.**

**Ejemplo del profe:** Aplicación → Base de datos. El middleware es el programa que ejecuta las consultas que los usuarios de la red le hacen a una base de datos central ubicada en el servidor, **a través de una API**.

> 🧠 **Analogía del intérprete de la ONU:** un delegado japonés y uno brasileño no se entienden. El intérprete (middleware) escucha, traduce, y cada uno cree que está hablando en su propio idioma. Ninguno tuvo que aprender el idioma del otro.

---

## 3. Cliente/Servidor: con y sin middleware

### Sin middleware (2 capas)

```
Cliente (Front-end)                         Servidor (Back-end)
 ┌───────────────────┐                     ┌───────────────────┐
 │  Proceso Cliente  │ ──── Petición ────► │  Proceso Servidor │
 │ Servicios Sistema │ ◄─── Respuesta ──── │ Servicios Sistema │
 │     Hardware      │                     │     Hardware      │
 └───────────────────┘                     └───────────────────┘
              ¿Y el canal de comunicaciones? ← lo tienes que hacer tú
```

**Problema:** el programador debe resolver TODO a mano: formatos de datos, errores de red, reintentos, sincronización.

### Con middleware (N capas)

```
Cliente (Front-end)          MIDDLEWARE           Servidor (Back-end)
 ┌────────────────┐        ┌───────────┐        ┌─────────────────┐
 │ Proceso Cliente│        │ Middleware│        │ Proceso Servidor│
 │  Middleware    │◄──────►│  (MOM)    │◄──────►│   Middleware    │
 │  Servicios SO  │        └───────────┘        │  Servicios SO   │
 │   Hardware     │                             │    Hardware     │
 └────────────────┘                             └─────────────────┘
        │                                                │
        └────── Petición / Respuesta directa ────────────┘
                     Opción 1: RPC
```

**Dos opciones de topología:**
- **Opción 1 (RPC):** el middleware del cliente habla **directo** con el del servidor (petición/respuesta)
- **Opción 2 (MOM):** hay un **middleware intermedio** (un broker) entre los dos

---

## 4. Clasificación de Middleware — Tipo 1 (por paradigma) ⭐

| Tipo | Qué es | Ejemplos |
|---|---|---|
| **RPC — Remote Procedure Call** | Llamar a un procedimiento que está en otra máquina | |
| ├─ *RPC-Oriented Middleware* | Orientado a procedimientos | **gRPC** |
| ├─ *Object-Oriented Middleware* | Orientado a objetos remotos | **CORBA**, RMI |
| └─ *Service-Oriented Middleware* | Orientado a servicios | **API REST** |
| **MOM — Message-Oriented Middleware** | Comunicación por mensajes en colas o tópicos | **Kafka, RabbitMQ, MQTT**, ActiveMQ |
| **TOM — Transaction-Oriented Middleware** | Orientado a transacciones y bases de datos | **ODBC, ORM** |

---

## 5. Clasificación de Middleware — Tipo 2 (por función/nivel)

### 5.1 Middleware de **comunicaciones** (bajo nivel)
> Proporciona el **medio de comunicación** para que las aplicaciones puedan conversar entre sí.

- Sockets
- HTTP
- RMI-IIOP
- SOAP
- RPC

### 5.2 Middleware de **base de datos** (SQL)
> **Enmascara las complejidades de acceso a la base de datos**, escondiendo los detalles de implementación de cada motor.

- **ODBC**, **JDBC**, OCI

> 🧠 **Analogía:** JDBC es como un control remoto universal. No importa si el televisor es Samsung, LG o Sony (Oracle, MySQL, PostgreSQL): tú aprietas el mismo botón.

### 5.3 Middleware de **aplicación** (Browser ↔ WebServer)
> Permite el **arranque, extensión e integración** de otras aplicaciones. Usa HTML/JS y HTTP.

- Ruby on Rails, Servlets/JSP, PHP (Laravel), ASPX, Python (Django/Flask), Node.js (Express)

---

## 6. Clasificación — Tipo 3 (por topología)

- **C/S** — Cliente/Servidor
- **P2P** — Peer to Peer

### Otros criterios
- Middleware **para IoT**
- Middleware **específico por dominio de aplicación**

---

## 7. Caracterización del Cliente Web

| Característica | Explicación |
|---|---|
| **Cliente universal** | El navegador funciona en cualquier dispositivo |
| **Protocolo de sesión** | **HTTP** |
| **Formato de presentación** | **HTML / CSS** |
| **Procesamiento en el cliente** | JS, TS, React, Angular, Vue |
| **Stateless** | HTTP no guarda estado entre peticiones |
| **Contenido estático vs dinámico** | Páginas fijas vs generadas al vuelo |
| **Limitación** | **Se pierden características** respecto a clientes standalone (acceso al hardware, ficheros locales, trabajo offline) |
| **Evolución** | Hacia **PWA** (Progressive Web Apps), que recuperan parte de eso |

---

## 8. Middlewares Aplicación ↔ Aplicación

### Basados en servicios distribuidos
- **Web Services:** SOAP, **API REST**
- **gRPC**

### MOMs con tópicos o colas
- **RabbitMQ**
- **ActiveMQ**
- **MQTT** (típico de IoT)
- **Apache Kafka**

## 9. Middleware Aplicación ↔ Datos
- **JDBC**
- **ADO, ODBC, OLEDB**

---

## 10. ⭐ MOM en profundidad (esto salió MUCHO en los parciales)

### ¿Qué es un MOM?
Un middleware que permite que las aplicaciones se comuniquen **enviando y recibiendo mensajes a través de un intermediario (broker)**, en vez de hablarse directamente.

### Colas vs Tópicos ⭐

| | **COLA (Queue)** | **TÓPICO (Topic)** |
|---|---|---|
| Cardinalidad | **1 : 1** — un mensaje, **un solo** consumidor | **1 : N** — un mensaje, **todos** los suscriptores |
| Patrón | Punto a punto / *work queue* | **Publish-Subscribe** |
| Se usa para | **Repartir trabajo** entre trabajadores | **Difundir** un evento a muchos interesados |
| Analogía | Fila de tareas en una oficina: cada quien toma una distinta | Altavoz de la oficina: todos escuchan lo mismo |

> ⚠️ **Preguntas de parcial:**
> - *"¿Qué papel juega una cola en un sistema MOM?"* → **Almacena mensajes de manera temporal hasta que sean procesados por un consumidor.**
> - *"¿Cuál es una ventaja de usar colas en MOM?"* → **Facilitan la implementación de patrones de mensajería como el envío y olvido (*fire-and-forget*).**
> - *"¿Qué ventaja clave proporciona publish/subscribe en MOM?"* → **Todos los consumidores reciben cada mensaje.**
> - *"¿Qué propiedad de la escalabilidad permite MOM?"* → **La capacidad de desacoplar el rendimiento de los subsistemas.** (Si el consumidor es lento, la cola amortigua; el productor no se frena.)
> - *"¿Cuál NO es un servicio proporcionado por plataformas MOM?"* → **Control de versiones.** (MOM sí hace balanceo de carga, transacciones distribuidas y autenticación.)
> - *"¿Cuál táctica facilita las comunicaciones asincrónicas?"* → **Sistemas de colas.**

### ¿Por qué MOM ayuda tanto?

- **Desacopla en el tiempo:** el emisor manda el mensaje y sigue trabajando; el receptor lo procesa cuando pueda. → **NO bloqueante**
- **Amortigua picos:** si llegan 10.000 pedidos de golpe, la cola los guarda y los nodos los procesan a su ritmo
- **Tolerancia a fallos:** si el consumidor se cae, los mensajes esperan en la cola

> 🧠 **Analogía del correo vs la llamada:**
> - **RPC / HTTP** = llamada telefónica. Los dos tienen que estar disponibles **al mismo tiempo**. Tú esperas colgado hasta que te respondan (**bloqueante**, acoplamiento temporal).
> - **MOM** = correo electrónico. Escribes, envías, sigues con tu vida. El otro lo lee cuando quiera (**no bloqueante**, desacoplamiento temporal).

---

## 11. Síncrono vs Asíncrono, Bloqueante vs No bloqueante

| | **Síncrono / Bloqueante** | **Asíncrono / No bloqueante** |
|---|---|---|
| Ejemplos | HTTP, RPC clásico | MOM, eventos, Publish-Subscribe |
| Quien llama | **Se queda esperando** la respuesta | **Sigue trabajando** |
| Acoplamiento temporal | **Alto** (ambos deben estar activos) | **Bajo** (pueden no coincidir) |
| Cardinalidad típica | 1:1 | 1:N |
| Analogía | Llamada telefónica | Correo / WhatsApp |

> **Notas de clase:** *"HTTP es bloqueante. Bloqueado hasta que aparezca una respuesta."*
> *"HTTP es asimétrico → para convertirlo en simétrico, usar WebSockets."*
> *"Async → disminuye el acoplamiento temporal."*

---

## 12. Clasificación de Flynn y Acoplamiento (repaso)

*(El profe volvió a mostrar estas dos láminas aquí — ver la Presentación 3 para el detalle completo.)*

| | **Fuertemente acoplado** | **Débilmente acoplado** |
|---|---|---|
| Retraso de mensajes | Corto | Grande |
| Transmisión de datos | Alta | Baja |
| Conexión | Cables en tarjetas | Red |
| Se usa como | Sistemas **paralelos** | Sistemas **distribuidos** |
| Velocidad | A velocidad de memoria | Algunas fibras llegan a velocidad de memoria |

---

## 📊 Tabla resumen — Presentación 5

| Tema | Lo mínimo que debes saber |
|---|---|
| **Comunicación por protocolos** | Sockets, HTTP (nivel bajo, tú haces todo) |
| **Comunicación por middleware** | RPC, MOM, Eventos (nivel alto) |
| **Middleware (definición)** | Habilita la comunicación entre componentes distribuidos |
| **3 funciones del middleware** | **Enmascara heterogeneidad** · mapea un modelo de SD · provee una **API** |
| **Ubicación en OSI** | **Capas 5 (Sesión) y 6 (Presentación)** |
| **Tipo 1 (paradigma)** | **RPC** (gRPC, CORBA, REST) · **MOM** (Kafka, RabbitMQ, MQTT) · **TOM** (ODBC, ORM) |
| **Tipo 2 (función)** | Comunicaciones (sockets, HTTP, SOAP, RPC) · Base de datos (ODBC, JDBC) · Aplicación (Django, Node, Laravel) |
| **Tipo 3 (topología)** | C/S · P2P |
| **Cliente web** | Universal, HTTP, HTML/CSS, **stateless**, evoluciona a **PWA** |
| **Cola (queue)** | **1:1** — un consumidor por mensaje. Reparte trabajo. Fire-and-forget |
| **Tópico (topic)** | **1:N** — todos los suscriptores reciben. Publish-Subscribe |
| **Ventaja MOM** | **Desacopla el rendimiento de los subsistemas** y permite asincronía |
| **MOM NO ofrece** | **Control de versiones** |
| **HTTP** | Síncrono, **bloqueante**, asimétrico, stateless. Para simétrico → **WebSockets** |
| **Async** | Disminuye el **acoplamiento temporal** |

---

## 📖 Glosario — Presentación 5

| Palabra | En cristiano |
|---|---|
| **Middleware (MW/MD)** | Software intermediario que conecta aplicaciones distribuidas y esconde la complejidad |
| **API** | *Application Programming Interface*: el conjunto de funciones que un componente ofrece para que otros lo usen |
| **Heterogeneidad** | Que las piezas sean distintas (SOs, lenguajes, hardware). El middleware la **enmascara** |
| **Socket** | El "enchufe" de más bajo nivel para mandar bytes entre dos procesos por red |
| **HTTP** | Protocolo de la web. Petición-respuesta, sin estado, bloqueante |
| **WebSocket** | Conexión permanente y **bidireccional/simétrica**, a diferencia de HTTP |
| **RPC** | *Remote Procedure Call*: llamar una función que vive en otra máquina |
| **gRPC** | Implementación moderna de RPC de Google |
| **CORBA** | Middleware clásico orientado a objetos remotos |
| **RMI** | *Remote Method Invocation*: RPC en el mundo Java |
| **SOAP** | Protocolo de web services basado en XML |
| **API REST** | Estilo de servicios sobre HTTP con recursos y verbos |
| **MOM** | *Message-Oriented Middleware*: comunicación por mensajes vía un intermediario |
| **TOM** | *Transaction-Oriented Middleware*: middleware orientado a transacciones |
| **Broker** | El intermediario que recibe, guarda y entrega mensajes |
| **Cola (Queue)** | Buzón donde **un solo** consumidor toma cada mensaje. Para repartir trabajo |
| **Tópico (Topic)** | Canal donde **todos** los suscriptores reciben el mensaje. Para difundir |
| **Productor** | El que envía mensajes |
| **Consumidor** | El que recibe y procesa mensajes |
| **Kafka / RabbitMQ / ActiveMQ / MQTT** | Implementaciones concretas de MOM |
| **Publish-Subscribe** | Patrón: publicar eventos, suscribirse a ellos, sin conocerse |
| **Fire-and-forget** | "Envío y olvido": mando el mensaje y no espero confirmación |
| **Síncrono** | El que llama espera la respuesta |
| **Asíncrono** | El que llama sigue trabajando; la respuesta llega después |
| **Bloqueante** | El proceso queda detenido esperando |
| **Acoplamiento temporal** | Necesidad de que ambas partes estén activas al mismo tiempo |
| **ODBC / JDBC / OLEDB / ADO** | Middlewares que dan un acceso uniforme a bases de datos distintas |
| **ORM** | *Object-Relational Mapping*: capa que traduce objetos del código a tablas de BD |
| **P2P** | *Peer to Peer*: todos los nodos son iguales, cliente y servidor a la vez |
| **PWA** | *Progressive Web App*: app web que se comporta casi como una app nativa |
| **Stateless** | Sin memoria de estado entre peticiones |
| **Balanceo de carga** | Repartir peticiones entre varios servidores |
