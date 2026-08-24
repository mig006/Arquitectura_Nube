# Presentación 6 — Llamadas a Procedimientos Remotos (RPC)

> **La idea en una frase:** RPC busca que llamar una función que está **en otro computador** se sienta exactamente igual que llamar una función local.

---

## 0. Conceptos que deben estar claros antes de entrar

El profe listó esto explícitamente:
- ¿Qué es un **middleware**? ¿Qué tipos hay?
- Diseño de aplicaciones distribuidas según el tipo de middleware
- **Serialización**

Y para desarrollar con un middleware de invocación remota hay que tener en cuenta:
- **Envío/recepción de tipos de datos**
- **Diferentes tipos de codificación** de esos datos
- **Identificación de componentes** del SD (objetos, procedimientos, componentes)
- **Mecanismos de activación** de objetos en el servidor
- **Seguridad en tipos de datos** (*type safety*)
- **Mecanismos de sincronización** en TCP y UDP

---

## 1. Serialización ⭐

> En un SD los componentes están en máquinas o procesos distintos y necesitan **intercambiar datos**. La **serialización** convierte los objetos o estructuras de datos en un formato que **puede enviarse por la red** (JSON, XML, Protocol Buffers…). El receptor **deserializa** para reconstruir el objeto original.

### ¿Por qué importa?

| Razón | Explicación |
|---|---|
| **Interoperabilidad** | Permite que sistemas escritos en **lenguajes diferentes** se comuniquen |
| **Eficiencia** | Algunos formatos son **más compactos y rápidos** que otros |
| **Compatibilidad** | Facilita la **evolución de las APIs** sin romper lo que ya funciona |

> 🧠 **Analogía del mueble de IKEA:** no puedes mandar un armario armado por correo. Lo **desarmas y lo empacas plano** (serializar), lo envías, y del otro lado lo **vuelven a armar** (deserializar). Lo importante es que las instrucciones de armado sean las mismas en ambos lados.

---

## 2. ¿En qué consiste el modelo de llamadas remotas?

- Permitir que un cliente **llame a procedimientos, funciones o métodos que están en otra parte de la red**.
- **Nace de la época de la programación procedimental.**
- **Trata de simular la misma semántica de una invocación local.**

> ⭐ **El gran reto de RPC:** *hacer que una llamada remota se parezca a una llamada local, aunque la red pueda retrasarse, perder mensajes o fallar.*

### Funcionamiento a alto nivel
- Un proceso en la **máquina A** llama a un proceso en la **máquina B**.
- **El proceso en A se BLOQUEA** mientras se ejecuta el proceso en B.
- Lo que se intercambia son **parámetros**, y se **espera respuesta**.

### Problemas de fondo
1. **Espacio de direccionamiento diferente** (los punteros de A no significan nada en B)
2. **Diferentes tipos de datos** (cada máquina representa números y letras a su manera)
3. **Fallas** (la red se cae, el servidor no responde)

---

## 3. Recordatorio: ¿cómo funciona una llamada LOCAL?

Ejemplo: `res = sumar(a, b);` llamado desde `main()`

1. El llamador **coloca los parámetros en el stack**, en orden (el último primero)
2. Se ejecuta la función `sumar`
3. Se coloca el **valor de retorno en un registro**, se remueve la dirección de retorno y se **devuelve el control** al llamador
4. El llamador **remueve los parámetros del stack** y sigue

**Paso de parámetros:**
- Por **VALOR** (se copia el dato)
- Por **REFERENCIA** (se pasa la dirección de memoria)

> ⚠️ **Ojo aquí:** el paso por referencia **NO puede funcionar entre máquinas distintas**, porque una dirección de memoria de la máquina A no existe en la máquina B. Esto explica la regla del punto 6.

---

## 4. ⭐ Funcionamiento de RPC: Stub, Proxy, Skeleton e IDL

> La idea es **hacer ver una llamada remota como si fuera local**, por lo que la invocación debe ser **transparente** para quien la usa.
>
> **La transparencia en RPC se logra agregando un Stub (o proxy) tanto al cliente como al servidor, y usando un IDL.**

### Los 10 pasos (¡el profe los numeró, y eso es señal de que los va a preguntar!)

| # | Paso |
|---|---|
| 1 | El **Cliente** llama a un procedimiento local llamado el **Client Stub (proxy)** |
| 2 | El Client Stub **empaqueta (marshalling)** los parámetros, construye un mensaje y lo envía al kernel |
| 3 | El **kernel local** lo envía al **kernel remoto** donde está el Server Stub |
| 4 | El kernel remoto entrega el mensaje al **Server Stub** |
| 5 | El Server Stub **desempaqueta (unmarshalling)** los parámetros, identifica el procedimiento y **lo ejecuta** |
| 6 | El Server Stub **recibe el resultado** del servidor |
| 7 | El Server Stub **empaqueta (marshalling)** la respuesta, construye un mensaje y lo envía al kernel |
| 8 | El **kernel remoto** lo envía de vuelta al **kernel local** (cliente) |
| 9 | El kernel local entrega el mensaje al **Client Stub** |
| 10 | El **Client Stub desempaqueta** el resultado y lo retorna **como si fuera un procedimiento local** |

```
   CLIENTE                                            SERVIDOR
 ┌──────────┐  1  ┌────────────┐              ┌────────────┐  5 ┌──────────┐
 │  main()  │────►│ Client Stub│              │ Server Stub│───►│ sumar()  │
 │          │◄────│  (proxy)   │              │ (skeleton) │◄───│          │
 └──────────┘ 10  └────────────┘              └────────────┘  6 └──────────┘
                   2│marshalling  ▲9                 ▲4  │7 marshalling
                    ▼             │                  │   ▼
                 ┌──────────────────┐          ┌──────────────────┐
                 │  Kernel local    │──── 3 ──►│  Kernel remoto   │
                 │                  │◄─── 8 ───│                  │
                 └──────────────────┘   RED    └──────────────────┘
```

### ¿Qué es cada pieza?

| Pieza | Qué hace |
|---|---|
| **Client Stub / Proxy** | Vive en el cliente. **Traduce las llamadas de procedimiento locales en mensajes de red** (y viceversa). Oculta toda la complejidad de comunicación |
| **Server Stub / Skeleton** | Vive en el servidor. Desempaqueta el mensaje, identifica qué procedimiento invocar y lo ejecuta |
| **Marshalling** | **Empaquetar**: convertir los parámetros a un formato estándar transmisible |
| **Unmarshalling** | **Desempaquetar**: reconstruir los parámetros del otro lado |
| **IDL** | El **contrato** que describe la interfaz en un lenguaje neutral |

> ⚠️ **Pregunta de parcial:** *"¿Qué papel juega el stub en RPC?"* → **Sirve como intermediario que traduce las llamadas de procedimiento locales en mensajes de red.**

> 🧠 **Analogía del traductor diplomático:** tú (cliente) hablas normal en tu idioma. El traductor (stub) escucha, traduce y escribe la carta. La manda por valija diplomática (kernel/red). Del otro lado, otro traductor (server stub) la lee, la traduce y se la dice al funcionario extranjero. Tú **nunca supiste** que hubo una traducción de por medio. Eso es transparencia.

---

## 5. Paso de parámetros en RPC ⭐

### El problema de las representaciones

Las máquinas no representan los datos igual:
- **Alfabetos:** ASCII vs EBCDIC
- **Enteros:** complemento a uno vs complemento a dos
- **Punto flotante:** distintos formatos
- **Ordenamiento de bytes:** **Little Endian** vs **Big Endian**

> Por eso se hace necesario **establecer una forma canónica de representar los distintos tipos de datos**.

> 🧠 **Analogía del endianness:** es como escribir la fecha. En Colombia `24/08/2026`, en EE.UU. `08/24/2026`. Es la misma fecha, pero si no acuerdan el formato, alguien va a llegar en el mes equivocado.

### Las reglas de oro (¡pregunta segura!)

| Tipo de dato | Cómo se pasa |
|---|---|
| **Tipos básicos / escalares** | **Por VALOR** |
| **Arreglos** | Se hace una **copia local** en cliente y servidor, se manipulan y se envían por la red |
| **Punteros** | ⛔ **En RPC NO hay paso de punteros** |

> ⚠️ **Memoriza esto: "En RPC no hay paso de punteros."** Es una frase textual de la diapositiva.
> **¿Por qué?** Porque un puntero es una dirección de memoria **local**. En la otra máquina esa dirección apunta a otra cosa (o a nada). Espacios de direccionamiento diferentes.

### Además de los parámetros, se transfiere:
- **Nombre del procedimiento**
- **Versión**
- Etc.

### 🔍 Pregunta abierta que salió en parcial: las `union` de C

> *"C tiene una construcción llamada `union`, en la que un byte se puede fraccionar en varias variables de varios bits y puede mantener cualquiera de varias alternativas. En tiempo de ejecución, no hay una manera segura de saber cuál alternativa se encuentra ahí. ¿Esta característica de C tiene alguna implicación para las llamadas a procedimientos remotos?"*
>
> **Respuesta:** **Sí, y es un problema grave.** El marshalling necesita saber **exactamente qué tipo de dato** está empaquetando para convertirlo a la forma canónica. Con una `union` el stub **no puede saber en tiempo de ejecución** cuál de las alternativas está activa, así que no sabe cómo serializarla ni cómo interpretarla del otro lado. Esto rompe el ***type safety*** y puede producir datos corruptos. Por eso los IDL suelen exigir **uniones discriminadas** (una unión que lleva pegada una etiqueta indicando cuál alternativa contiene).

---

## 6. Localización de servidores (Binding) ⭐

> *¿Cómo hace un cliente para localizar un servidor?*

**Dos opciones:**

1. **Que el cliente tenga la dirección del servidor "quemada"** → **muy ineficiente y estático**
2. **Hacer un proceso dinámico de asociación (*binding*)** ✅

### El Binder

Se parte de una **especificación formal del servidor**:
- **Nombre** del servidor
- **Número de versión**
- **Lista de procedimientos**
- Para cada procedimiento, sus **parámetros**

**Cómo funciona:**
1. Cuando un servidor **arranca**, envía un mensaje a un programa (local o remoto) llamado el **"binder"** o **servidor de registro**
2. El **cliente conoce la dirección del binder**, y le pregunta si existen o no los servidores que quiere contactar
3. El binder le responde dónde está

> Este binder se conoce como un **Localizador de Recursos** en la red, un **broker** o un **"corredor" de procedimientos**.

> 🧠 **Analogía:** el binder son las Páginas Amarillas. No memorizas el teléfono de todos los plomeros de la ciudad; buscas en el directorio, que se mantiene actualizado solo cuando los plomeros se registran.

**La especificación formal también es muy útil para los generadores de código.**

---

## 7. IDL — Interface Definition Language ⭐

> **¿Por qué necesitamos un IDL en RPC?**

- **Separa la especificación de la implementación**
- Define las interfaces en un **lenguaje neutral** (no atado a C, Java, Python…)
- **Describe procedimientos y parámetros**
- Para **SUN-RPC** el lenguaje es **RPCL** (archivos `.x`)
- **Se precompila** para generar automáticamente:
  - **Stubs** (cliente — empaquetar)
  - **Proxies** (ocultan la complejidad de la comunicación)
  - **Skeletons** (servidor — desempaquetar)

```
   Cliente          IDL              Servidor
   (Stub)   ◄──► (Contrato) ◄──►  (Implementación)
```

> 🧠 **Analogía:** el IDL es el **contrato firmado ante notario**. Dice exactamente qué servicios se prestan y con qué parámetros. El cliente y el servidor lo firman por separado y no necesitan conocerse ni estar escritos en el mismo lenguaje.

---

## 8. Semánticas de invocación en RPC ⭐⭐

Cuando la red falla, ¿qué garantía tienes de cuántas veces se ejecutó tu llamada?

| Semántica | Qué garantiza | Riesgo |
|---|---|---|
| **Maybe / Best-effort** | **Ninguna garantía**. Se intenta y ya | Puede no ejecutarse nunca |
| **At-least-once** (al menos una vez) | Se reintenta hasta obtener respuesta → **se ejecuta 1 o más veces** | ⚠️ **Puede haber DUPLICADOS** |
| **At-most-once** (a lo sumo una vez) | El servidor **filtra los duplicados** → se ejecuta **0 o 1 vez, nunca más** | ✅ **Evita ejecuciones duplicadas** |
| **Exactly-once** | Exactamente una vez. Es el ideal, muy difícil de lograr en la práctica | — |
| **Fire-and-forget** | Envío y no espero respuesta | Sin garantía alguna |

> ⚠️ **Pregunta de parcial (salió textual):** *"¿Qué semántica RPC evita ejecuciones duplicadas?"* → **At-most-once.**

> 🧠 **Analogía:** mandas una transferencia bancaria y se cae el internet. ¿Se hizo o no?
> - **At-least-once** = vuelves a mandarla hasta estar seguro… pero puede que se haya hecho **dos veces**. 😬
> - **At-most-once** = el banco detecta que ese ID de transacción ya llegó y lo ignora. **Máximo una vez.** 👍
>
> Por eso **la idempotencia y at-most-once son primos hermanos**: ambos existen para poder reintentar sin hacer daño.

> ⚠️ **Otra pregunta relacionada:** *"Si un proceso A envía una petición a B y no recibe respuesta, es imposible saber por qué (si falló el mensaje, el receptor o la respuesta). Un mecanismo que tiene A para tener mayor garantía de éxito o fracaso es:"* → **Limitar el tiempo de espera de la respuesta a la llamada por parte del proceso A** (es decir, un **timeout**).

---

## 9. Retos de RPC en sistemas distribuidos

> ⚠️ **Pregunta de parcial:** *"¿Cuál es un desafío común al utilizar RPC en sistemas distribuidos?"* → **Manejo de fallos en la red durante la llamada remota.**

Otros retos:
- La llamada es **bloqueante** → si el servidor tarda, el cliente se queda pegado
- **Acoplamiento temporal alto**: ambos deben estar vivos al mismo tiempo
- El servidor es un posible **SPOF**
- No hay punteros → hay que rediseñar interfaces pensando en valores

### ¿Pueden existir métodos con el mismo nombre en un servidor RPC?

> Pregunta abierta que salió en parcial. **Respuesta:** **No, en el RPC clásico no**, porque cada procedimiento se identifica por un **número de programa, versión y número de procedimiento** en el IDL — no por una firma con tipos como en la sobrecarga de C++/Java. El *dispatcher* del server stub necesita un identificador **único**; dos procedimientos con el mismo nombre serían ambiguos. Para tener algo parecido a sobrecarga hay que darles nombres distintos en el `.x` (por ejemplo `sumar_int` y `sumar_float`).

---

## 10. RPC en la práctica: portmap vs rpcbind

### portmap vs rpcbind

| Característica | **portmap** (RPC tradicional) | **rpcbind** (TI-RPC) |
|---|---|---|
| Soporte de transporte | Solo **IPv4** | **IPv4 e IPv6** |
| Protocolo | PORTMAPPER (ID: 100000) | RPCBIND (ID: 100000) |
| Autenticación | **No soporta** | Soporta **GSS-API (Kerberos)** |
| Disponibilidad | **Obsoleto** | Reemplazo moderno |
| Seguridad | Baja (sin autenticación) | Mejor seguridad y control de acceso |
| Uso en Ubuntu 22.04 | **Eliminado** | **Obligatorio** para TI-RPC |

### RPC tradicional vs TI-RPC

| Característica | **RPC tradicional** | **TI-RPC** |
|---|---|---|
| Dependencia de transporte | UDP/TCP (BSD Sockets) | Múltiples protocolos (**IPv4, IPv6, SCTP**) |
| Librería | `librpcsvc` | **`libtirpc`** |
| Manejador de puertos | `portmap` | **`rpcbind`** |
| Soporte IPv6 | No | **Sí** |
| Seguridad | Limitada | Autenticación avanzada (**Kerberos**) |
| Compatibilidad | Sistemas antiguos | Sistemas modernos |

> 💡 En resumen: **`rpcbind` es el reemplazo moderno de `portmap`**, necesario para TI-RPC con IPv6 y seguridad avanzada. Si tu código usa RPC tradicional, hay que migrarlo a TI-RPC y asegurarse de que `rpcbind` esté corriendo.

> 🧠 **¿Qué hace este servicio?** Es el **binder** de SUN-RPC. Escucha en el puerto **111** y responde: "¿Buscas el programa 0x20000001 versión 1? Está en el puerto 45231."

---

## 11. Ejemplo en RPCL (SUN-RPC)

Flujo de trabajo típico:

```
  sumador.x   ──rpcgen──►  ┌── sumador.h        (cabeceras)
  (el IDL)                 ├── sumador_clnt.c   (client stub)
                           ├── sumador_svc.c    (server skeleton, sin main)
                           └── sumador_xdr.c    (marshalling / XDR)
                                    │
                     Tú escribes:   ├── sumador_server.c  (la implementación real)
                                    └── sumador_client.c  (tu main del cliente)
```

Puntos que el profe recalcó:
- El **servidor generado va "sin main"** — tú escribes la implementación de los procedimientos
- El archivo `.x` es **el IDL**, la única fuente de verdad del contrato
- **XDR** (*eXternal Data Representation*) es el formato canónico de SUN-RPC → la **serialización**

---

## 📊 Tabla resumen — Presentación 6

| Tema | Lo mínimo que debes saber |
|---|---|
| **Serialización** | Convertir objetos a un formato transmisible (JSON, XML, Protobuf, XDR). Da **interoperabilidad, eficiencia y compatibilidad** |
| **RPC (definición)** | Llamar procedimientos que están en otra máquina, simulando una llamada local |
| **El gran reto de RPC** | Que la llamada remota **parezca local** aunque la red falle, se retrase o pierda mensajes |
| **RPC es** | **BLOQUEANTE**: el proceso llamador se detiene mientras se ejecuta el remoto |
| **3 problemas de fondo** | Espacio de direccionamiento distinto · Tipos de datos distintos · Fallas |
| **Cómo se logra transparencia** | Agregando **Stub/Proxy en cliente y servidor** + un **IDL** |
| **Client Stub / Proxy** | **Traduce llamadas locales en mensajes de red** |
| **Server Stub / Skeleton** | Desempaqueta, identifica el procedimiento y lo ejecuta |
| **Marshalling / Unmarshalling** | Empaquetar / desempaquetar a formato canónico |
| **10 pasos de RPC** | Stub → marshalling → kernel → red → kernel → server stub → unmarshalling → ejecuta → marshalling → vuelta |
| **Paso de parámetros** | Escalares **por valor** · arreglos por copia · **NO hay paso de punteros** |
| **Problemas de representación** | ASCII/EBCDIC, complemento a 1/2, punto flotante, **Little/Big Endian** → forma **canónica** |
| **Binding** | El servidor se registra en el **binder** (broker/localizador); el cliente le pregunta |
| **IDL** | **Separa especificación de implementación**, lenguaje neutral, genera stubs/proxies/skeletons |
| **IDL de SUN-RPC** | **RPCL** (archivos `.x`), formato de datos **XDR** |
| **Semánticas** | Maybe · **At-least-once** (duplicados) · **At-most-once** (evita duplicados) · Exactly-once |
| **Evita duplicados** | **At-most-once** |
| **Desafío común de RPC** | **Manejo de fallos en la red durante la llamada** |
| **portmap vs rpcbind** | rpcbind = moderno, IPv6, Kerberos. portmap = obsoleto, solo IPv4, sin autenticación |
| **RPC vs TI-RPC** | `librpcsvc`/portmap vs **`libtirpc`/rpcbind**, IPv6 y seguridad avanzada |

---

## 📖 Glosario — Presentación 6

| Palabra | En cristiano |
|---|---|
| **RPC** | *Remote Procedure Call*: llamar una función que vive en otro computador |
| **Serialización** | Convertir un objeto en una secuencia de bytes que se puede enviar por la red |
| **Deserialización** | Reconstruir el objeto a partir de esos bytes |
| **Marshalling** | Empaquetar los parámetros a un formato estándar para enviarlos |
| **Unmarshalling** | Desempaquetarlos del otro lado |
| **Formato canónico** | Una forma "neutral" acordada de representar los datos, que ambas máquinas entienden |
| **XDR** | *eXternal Data Representation*: el formato canónico que usa SUN-RPC |
| **Protocol Buffers (Protobuf)** | Formato de serialización binario, compacto y rápido (lo usa gRPC) |
| **JSON / XML** | Formatos de serialización de texto, legibles por humanos |
| **Stub** | Pieza generada automáticamente que traduce llamadas locales ↔ mensajes de red |
| **Proxy** | Sinónimo práctico de client stub: representa localmente al objeto remoto |
| **Skeleton** | El stub del lado del servidor: desempaqueta y llama al procedimiento real |
| **Kernel** | El núcleo del sistema operativo; aquí, quien realmente manda los bytes por la red |
| **IDL** | *Interface Definition Language*: contrato neutral que describe la interfaz |
| **RPCL** | El IDL concreto de SUN-RPC. Archivos `.x` |
| **`rpcgen`** | El compilador que lee el `.x` y genera stubs, skeletons y cabeceras |
| **Binding** | El proceso de que un cliente encuentre y se asocie a un servidor |
| **Binder** | El registro/directorio donde los servidores se anuncian y los clientes consultan |
| **Broker** | Otro nombre para el intermediario/localizador de recursos |
| **portmap** | El binder clásico de SUN-RPC. Obsoleto, solo IPv4 |
| **rpcbind** | Su reemplazo moderno: IPv6, Kerberos, mejor seguridad |
| **TI-RPC** | *Transport Independent RPC*: versión moderna, independiente del transporte |
| **`libtirpc`** | La librería de TI-RPC |
| **Paso por valor** | Se copia el dato. Es lo que hace RPC con los escalares |
| **Paso por referencia** | Se pasa una dirección de memoria. **No funciona entre máquinas** |
| **Endianness (Little/Big Endian)** | El orden en que una máquina guarda los bytes de un número |
| **Type safety** | Garantía de que un dato es del tipo que se dice que es |
| **`union` (de C)** | Estructura que puede contener varias alternativas; **rompe el type safety** en RPC |
| **Semántica de invocación** | La garantía de cuántas veces se ejecutó realmente la llamada |
| **At-least-once** | Al menos una vez → puede haber **duplicados** |
| **At-most-once** | A lo sumo una vez → **evita duplicados** |
| **Exactly-once** | Exactamente una vez. El ideal, difícil de lograr |
| **Best-effort / Maybe** | Sin garantías |
| **Fire-and-forget** | Envío y no espero respuesta |
| **Timeout** | Límite de tiempo de espera; sirve para detectar fallos |
| **Bloqueante** | El proceso que llama queda detenido hasta recibir la respuesta |
| **gRPC** | Implementación moderna de RPC (Google), usa Protobuf e HTTP/2 |
