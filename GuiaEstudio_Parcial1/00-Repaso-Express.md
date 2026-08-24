# 00 — Repaso Express + Preguntas Tipo Parcial

> **Léelo el martes en la noche.** Es el destilado de las 6 presentaciones + las respuestas a las preguntas que salieron en los tres parciales de ejemplo.

---

## 📁 Índice de la guía

| Archivo | Tema |
|---|---|
| `01-Redes-Repaso.md` | Redes, IP, DNS, puertos, AAA |
| `02-Nube.md` | Cloud computing, IaaS/PaaS/SaaS, nubes pública/privada/híbrida |
| `03-Introduccion-Sistemas-Distribuidos.md` | Qué es un SD, transparencias, escalabilidad, falacias, Flynn |
| `04-Arquitecturas-Sistemas-Distribuidos.md` | Cliente-servidor, capas, SOA, pub-sub, REST, n-tier, SPOF |
| `05-Middleware.md` | Middleware, MOM, colas vs tópicos, síncrono vs asíncrono |
| `06-RPC.md` | RPC, stubs, marshalling, IDL, semánticas de invocación |

---

## 🔥 Las 12 ideas que más se repiten en los parciales

1. **Autenticación** = quien dice ser, es. (Autorización = permisos. Accounting = registro.)
2. **Versionado NO es una transparencia.** Las transparencias son: acceso, ubicación, migración, reubicación, replicación, concurrencia, falla, persistencia.
3. **At-most-once** es la semántica RPC que **evita ejecuciones duplicadas**.
4. **POST NO es idempotente.** GET, PUT y DELETE sí lo son.
5. **HTTP es un protocolo SIN ESTADO (stateless)**, de la capa de **Aplicación**, síncrono/bloqueante y asimétrico.
6. **Stateless escala** porque cada petición trae toda la información → cualquier servidor puede atenderla.
7. **Máximo control para escalar = el código de la aplicación. Mínimo impacto = ancho de banda e Internet.**
8. **Alta latencia NO es un beneficio de la nube.**
9. **El timeout sirve para DETECTAR FALLOS POTENCIALES.**
10. **Uptime** es la métrica de **disponibilidad**. (Tiempo de respuesta y TPS miden rendimiento.)
11. **El error clásico de diseño en SD es asumir latencia despreciable y red confiable/estática.**
12. **El middleware aporta transparencia de comunicación, heterogeneidad y localización.**

---

## ✅ Banco de preguntas resueltas (de los 3 parciales de ejemplo)

### Sistemas Distribuidos — conceptos generales

**1. ¿Cuál NO es una transparencia?**
A. Acceso · B. Ubicación · **C. Versionado ✅** · D. Concurrencia

**2. Un balanceador de carga introduce principalmente:**
**A. Transparencia de acceso ✅**
*(El cliente no sabe cuál servidor lo atendió — le da igual.)*

**3. Un sistema de carpeta como OneDrive proporciona transparencia de (múltiple):**
**A. ubicación ✅ · B. movilidad ✅ · C. acceso ✅**
*(No sabes en qué servidor está tu archivo, ni que se movió, ni cómo se representa internamente.)*

**4. La garantía de que una ejecución la efectúa aquella entidad o persona que asegura ejecutarla es:**
**A. Autenticación ✅**

**5. En sistemas distribuidos, un timeout se usa para:**
**B. Detectar fallos potenciales ✅**

**6. ¿Cuáles métricas permiten evaluar la disponibilidad de un sistema?**
**B. uptime ✅** *(y en la versión múltiple, también "denegación de servicio")*

**7. Un error de diseño frecuente en SD es asumir:**
**B. Latencia despreciable y red confiable/estática ✅**

**8. En SD, elegir distribución a veces no es opcional cuando:**
**B. Hay múltiples organizaciones o ubicaciones (movilidad) ✅**

**9. ¿Cuál describe mejor un sistema SIMD?**
**C. Un sistema donde una instrucción se ejecuta simultáneamente sobre múltiples conjuntos de datos ✅**

---

### Escalabilidad

**10. Al diseñar, el componente donde tenemos MÁXIMO control e impacto en la escalabilidad:**
**A. Código de la aplicación ✅**

**11. Al diseñar, el componente donde tenemos MÍNIMO impacto en la escalabilidad:**
**B. Ancho de banda e Internet ✅**

**12. ¿Cómo puede afectar un valor de timeout inadecuado a la escalabilidad?**
**A. Un timeout corto puede aumentar la carga del servidor por retransmisiones ✅**
*(y un timeout largo mantiene recursos ocupados — ambas afectan.)*

**13. Una técnica clásica para escalar horizontalmente una app web es:**
**D. Añadir más instancias de servidores detrás de un balanceador de carga ✅**

**14. Una app puede escalar globalmente usando CDNs porque:**
**C. Replican contenido en múltiples ubicaciones cercanas a los usuarios, reduciendo el RTT ✅**

**15. Un ejemplo de pérdida de escalabilidad:**
**C. IPv4 ✅** *(se agotó el espacio de direcciones)*

**16. El principio de "divide and conquer" aplicado a procesamiento distribuido se implementa típicamente en:**
**B. Algoritmos MapReduce / procesamiento paralelo de datos ✅**

**17. La táctica de COLAS se usa para apoyar:**
**D. Rendimiento ✅** *(amortigua picos y desacopla productor de consumidor)*

---

### Nube

**18. ¿Cuál de los siguientes NO es un beneficio de la computación en nube? (múltiple)**
**A. Alta latencia ✅**
*(Sí son beneficios: múltiples ciclos de aprovisionamiento, alta disponibilidad, BD tolerantes a fallas, recursos temporales y descartables.)*

---

### Arquitecturas

**19. ¿Cuál arquitectura reduce el impacto del SPOF en Cliente/Servidor?**
**B. Replicación del servidor ✅**

**20. En arquitecturas multicapa (n-tier), la capa intermedia suele encargarse de:**
**B. Lógica de negocio y coordinación entre clientes y servidores de datos ✅**

**21. El uso de middleware en arquitecturas de SD aporta:**
**D. Transparencia de comunicación, heterogeneidad y localización ✅**

**22. ¿Cuál característica pertenece al protocolo HTTP?**
**A. Protocolo sin estado ✅** *(y también: asimétrico)*
*(NO es: de la capa de Transporte, asincrónico, ni de codificación binaria.)*

**23. La propiedad de stateless en HTTP facilita la escalabilidad porque:**
**D. Cada petición contiene toda la información necesaria, evitando dependencia de estado en el servidor ✅**

**24. ¿Cuál de los siguientes métodos NO es idempotente?**
**C. POST ✅**

**25. ¿En cuál escenario la idempotencia NO aporta beneficios claros?**
**C. En operaciones de solo lectura ✅** *(leer ya es idempotente por naturaleza)*

---

### Middleware y MOM

**26. ¿Cuál táctica facilita las comunicaciones asincrónicas entre sistemas?**
**B. Sistemas de colas ✅**

**27. ¿Qué papel juega una cola en un sistema de MOM?**
**B. Almacena mensajes de manera temporal hasta que sean procesados por un consumidor ✅**

**28. ¿Cuál es una ventaja de utilizar colas en un sistema de MOM?**
**D. Facilitan la implementación de patrones de mensajería como el envío y olvido (fire-and-forget) ✅**

**29. ¿Qué ventaja clave proporciona el modelo "publish/subscribe" en MOM?**
**A. Todos los consumidores reciben cada mensaje ✅**

**30. ¿Qué propiedad de la escalabilidad permite MOM?**
**B. La capacidad de desacoplar el rendimiento de los subsistemas ✅**

**31. ¿Cuál NO es un servicio proporcionado por plataformas MOM?**
**D. Control de versiones ✅**
*(MOM sí ofrece: balanceo de carga, transacciones distribuidas, autenticación de usuarios.)*

**32. Diseño: nodos de procesamiento + clientes, ¿RPC o MOM?**
**B. Utilizar un MOM server configurando COLAS, de tal manera que cada CLIENTE registre una Tarea y los NODOS accedan a una Tarea en la Cola ✅**
*(Colas, no tópicos: con tópicos todos los nodos harían la misma tarea.)*

---

### RPC

**33. ¿Qué semántica RPC evita ejecuciones duplicadas?**
**B. At-most-once ✅**

**34. ¿Qué papel juega el "stub" en RPC?**
**B. Sirve como intermediario que traduce las llamadas de procedimiento locales en mensajes de red ✅**

**35. ¿Cuál es un desafío común al utilizar RPC en sistemas distribuidos?**
**C. Manejo de fallos en la red durante la llamada remota ✅**

**36. Si A envía una petición a B y no recibe respuesta, ¿qué mecanismo le da mayor garantía de éxito o fracaso?**
**B. Limitar el tiempo de espera de la respuesta por parte del proceso A (timeout) ✅**

**37. ¿Pueden existir en un servidor RPC métodos o procedimientos con el mismo nombre? Justifique.**
**No.** Cada procedimiento se identifica en el IDL por **número de programa + versión + número de procedimiento**, no por una firma con tipos. El dispatcher del server stub necesita un identificador **único**; dos con el mismo nombre serían ambiguos. Para algo parecido a la sobrecarga hay que darles nombres distintos en el `.x`.

**38. La `union` de C: ¿tiene implicaciones para RPC? Explique.**
**Sí.** El marshalling necesita conocer el tipo exacto del dato para convertirlo a la forma canónica. En una `union` **no hay forma segura de saber en tiempo de ejecución** cuál alternativa está activa, así que el stub no sabe cómo serializarla ni cómo interpretarla del otro lado. Rompe el ***type safety*** y produce datos corruptos. Por eso los IDL exigen **uniones discriminadas** (con una etiqueta que indica cuál alternativa contiene).

---

### Cómputo paralelo (por si acaso)

**39. Programa con Ts = 150 unidades, 70% paralelizable, se quiere speedup de 2.5. ¿Cuántos cores?**

Se usa la **Ley de Amdahl**:

$$S = \frac{1}{(1-P) + \frac{P}{N}}$$

Con P = 0.70 y S = 2.5:

$$2.5 = \frac{1}{0.30 + \frac{0.70}{N}} \;\Rightarrow\; 0.30 + \frac{0.70}{N} = 0.4 \;\Rightarrow\; \frac{0.70}{N} = 0.1 \;\Rightarrow\; N = 7$$

**Respuesta: 7 cores ✅**

> 💡 **Cómo hacerlo rápido en el parcial:** `1/S = (1-P) + P/N` → despejas `P/N = 1/S - (1-P)` → `N = P / (1/S - (1-P))`.

**40. Combinación correcta de MPI con multithreading/MP:**
**C. Los procesos MPI reemplazan el multithreading lanzando un proceso MPI por cada core ✅**

---

## 🗺️ Mapa mental de todo el parcial

```
                      REDES (P1)
              IP, DNS, puertos, gateway, AAA
                          │
                          ▼
                      NUBE (P2)
        IaaS/PaaS/SaaS · pública/privada/híbrida · 5 características
                          │
                          ▼
          SISTEMAS DISTRIBUIDOS (P3)
   definición · transparencias · escalabilidad · falacias · Flynn
                          │
                          ▼
             ARQUITECTURAS (P4)
   C/S · capas · SOA/microservicios · pub-sub · REST · n-tier · SPOF
                          │
                          ▼
              MIDDLEWARE (P5)
        RPC · MOM (colas vs tópicos) · sync vs async
                          │
                          ▼
                   RPC (P6)
    stubs · marshalling · IDL · binder · semánticas de invocación
```

---

## ⚠️ Trampas y confusiones frecuentes

| Se confunden… | La diferencia |
|---|---|
| **Autenticación / Autorización** | Quién eres / qué puedes hacer |
| **Escalabilidad / Elasticidad** | Aguantar más carga / ajustarse **automáticamente** hacia arriba y abajo |
| **Disponibilidad / Rendimiento** | *uptime* / *tiempo de respuesta y TPS* |
| **Cola / Tópico** | Un solo consumidor (repartir) / todos los suscriptores (difundir) |
| **At-least-once / At-most-once** | Puede duplicar / **evita** duplicar |
| **Síncrono / Asíncrono** | Espero la respuesta / sigo trabajando |
| **Stub / Skeleton** | Lado cliente (empaqueta) / lado servidor (desempaqueta) |
| **Marshalling / Serialización** | En RPC son prácticamente lo mismo: convertir datos a formato transmisible |
| **IaaS / PaaS / SaaS** | Te dan máquinas / te dan plataforma / te dan la app terminada |
| **Nube privada / híbrida / pública** | Solo tuya / mezcla / del proveedor |
| **Acoplamiento fuerte / débil** | Memoria compartida, paralelo / red, distribuido |
| **SPOF / cuello de botella** | Si falla, todo cae / lo que limita la velocidad |
| **Transparencia de migración / reubicación** | Se puede mover / se mueve **mientras lo usas** |
| **Desacoplamiento temporal / referencial** | No coinciden en el tiempo / no se conocen entre sí |

---

## 🎯 Chequeo final: ¿me sé el parcial?

Marca mentalmente. Si dudas en alguna, vuelve al archivo correspondiente.

- [ ] Sé decir qué es un sistema distribuido con las 3 partes de la definición
- [ ] Me sé las 8 transparencias y sé que **versionado no es una**
- [ ] Me sé las 5 características de la nube (NIST)
- [ ] Puedo llenar la tabla IaaS / PaaS / SaaS de memoria
- [ ] Sé qué es un SPOF y cómo se reduce
- [ ] Sé qué hace un middleware (3 funciones) y en qué capas de OSI está
- [ ] Puedo explicar la diferencia entre cola y tópico con un ejemplo
- [ ] Me sé los 10 pasos de RPC (al menos el orden general)
- [ ] Sé qué es marshalling, stub, skeleton e IDL
- [ ] Sé por qué **no hay paso de punteros en RPC**
- [ ] Me sé las semánticas de invocación y cuál evita duplicados
- [ ] Sé qué métodos HTTP son idempotentes y cuál no
- [ ] Me sé las 8 falacias de Deutsch
- [ ] Puedo aplicar la Ley de Amdahl para sacar el número de cores

---

**¡Éxitos el miércoles! 🚀**
