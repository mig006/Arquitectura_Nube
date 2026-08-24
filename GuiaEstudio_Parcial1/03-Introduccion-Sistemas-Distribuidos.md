# Presentación 3 — Introducción a los Sistemas Distribuidos

> **Referencia:** Capítulo 1 — *Distributed Systems, 4th ed.* (Tanenbaum & Van Steen)
>
> **La idea en una frase:** muchos computadores trabajando juntos, pero que **se ven como uno solo** para el usuario.

---

## 1. Un poco de historia (para entender el "por qué")

### Al principio
- **Mainframes con terminales brutas** (pantallas sin cerebro, todo lo procesaba la máquina central)
- **Bancos de cintas** para guardar información
- 👉 La idea de distribuir la computación **no es nueva**; lo que cambia es cómo se hace.

### Los años 80: dos inventos que cambiaron todo
1. **Microprocesadores** — se pasó de una máquina de **10 millones de dólares** que ejecutaba **1 instrucción por minuto**, a máquinas de **1.000 dólares** con **10 millones de instrucciones por segundo**.
2. **Las redes** — de repente se podían conectar computadores entre sí.

### Consecuencias
- Es **fácil y barato** montar muchos equipos con múltiples procesadores en red.
- Aparece la **necesidad de sincronizar** el trabajo de cada componente (dos entidades trabajan más rápido que una… si se coordinan).
- Dos desarrollos cambiaron el panorama de forma irreversible:
  - La **expansión de Internet** → red / **multicomputador**
  - El diseño de **computadores multinúcleo** → **multiprocesador**

---

## 2. Definición de Sistema Distribuido

> **Sistema cuyos componentes están ubicados en diferentes redes, que se comunican y coordinan sus acciones mediante el paso de mensajes, y que dan al usuario la impresión de constituir un único sistema coherente.**

Fíjate en las 3 partes de la definición (esto es lo que califican):
1. Componentes **en máquinas/redes diferentes**
2. Se comunican y coordinan **por paso de mensajes**
3. El usuario percibe **un único sistema coherente** ← esto es *transparencia*

### Aplicación en red
Es un sistema distribuido implementado sobre una red. Ejemplos que dio el profe:
- Sistema de consulta de saldos bancarios
- La **WWW**
- Sistema distribuido multimedia (radio en línea)
- **DNS**
- Sensores que monitorean puentes o montañas

### Las definiciones "chistosas" que hay que reconocer

| Autor | Frase |
|---|---|
| **Leslie Lamport** | "Un sistema distribuido es aquel en el que al fallar una computadora que **ni sabías que existía** puede hacer que el sistema NO quede completamente inservible" |
| **Sape Mullender** | "Un sistema distribuido es aquel que **NO deja de trabajar** cuando un equipo completamente desconocido falla" |

> 🧠 Ambas apuntan a lo mismo: **fallas parciales**. En un sistema centralizado, si falla, falla todo. En uno distribuido, falla un pedazo y el resto sigue.

---

## 3. ¿Por qué construir sistemas distribuidos?

Para **aprovechar eficientemente los recursos, mejorar la disponibilidad y el rendimiento**, y enfrentar las dinámicas cambiantes de las organizaciones y la informática a escala.

> Hoy **casi todas las apps son distribuidas**. Muy pocas son centralizadas o *standalone*.

### Los 3 grandes beneficios

| Beneficio | Qué significa |
|---|---|
| **Rendimiento** | Más máquinas trabajando = más trabajo hecho |
| **Escalabilidad** | Puedo crecer agregando máquinas |
| **Disponibilidad** | Si una falla, otra responde |

Y todo esto **a un menor costo** que una supermáquina única.

---

## 4. Consecuencias de ser distribuido (los 4 dolores de cabeza)

| Consecuencia | Qué implica |
|---|---|
| **Concurrencia** | Todos los servicios operan al mismo tiempo |
| **Carencia de reloj global** | **No existe un reloj compartido** → es dificilísimo saber qué pasó "antes" |
| **Independencia** | Cada componente/proceso **puede fallar por separado** (fallas parciales) |
| **Sin memoria compartida** | Los procesos **no comparten un espacio de memoria física** → toca mandarse mensajes |

> 🧠 **Analogía:** un grupo de amigos organizando una fiesta por WhatsApp, cada uno en una ciudad distinta. Nadie ve la libreta de los otros (sin memoria compartida), los relojes de cada uno están un poquito desfasados (sin reloj global), todos escriben a la vez (concurrencia) y a uno se le puede acabar la batería sin que los demás se enteren (independencia/falla parcial).

---

## 5. Retos de los Sistemas Distribuidos

Desde la perspectiva de **procesos**:
- **Comunicación** entre procesos
- **Coordinación** entre procesos
- **Concurrencia**

Otros retos transversales:
- **Naming** — dónde están los recursos y cómo localizarlos
- **Datos** — consistencia, replicación, transacciones
- **Tolerancia a fallas**
- **Seguridad**

---

## 6. Metas de diseño (¡tema estrella del parcial!)

1. **Heterogeneidad**
2. **Apertura** (*Openness*)
3. **Seguridad**
4. **Escalabilidad**
5. **Manejo de fallas**
6. **Concurrencia**
7. **Transparencia** (Acceso, Localización, Concurrencia, Replicación, Fallo, Persistencia)

### 6.1 Heterogeneidad
El sistema debe convivir con componentes distintos en varios niveles:
- de **red**, de **hardware**, de **sistema operativo**, de **lenguaje de programación**, y desde la perspectiva de **múltiples desarrolladores**

> 🧠 Analogía: una reunión internacional donde cada quien habla un idioma distinto. Necesitas traductores → eso será el **middleware**.

### 6.2 Apertura (Openness)
- Capacidad de que el sistema pueda ser **extendido y re-implementado** de distintas formas.
- Se mide por **cuántos recursos nuevos se pueden agregar** y poner a disposición de clientes distintos.
- Requiere **publicar las interfaces** de los componentes → *"interfaces as a contract"* (la interfaz es un contrato).
- Requiere **especificación y documentación** disponibles.
- Trae el problema derivado de la **integración** de esos componentes.

### 6.3 Seguridad
La seguridad de la información tiene **tres componentes** (la famosa tríada CIA):
- **Confidencialidad** — que solo lo vea quien debe
- **Integridad** — que nadie lo altere sin permiso
- **Disponibilidad** — que esté ahí cuando se necesite

### 6.4 Escalabilidad

> Un sistema es escalable si **sigue siendo eficiente cuando aumentan los recursos y los usuarios** (la demanda), manteniéndose dentro de un rendimiento aceptable.

**Retos para diseñar sistemas escalables:**
- Controlar el **costo de los recursos físicos**
- Controlar la **pérdida de rendimiento**
- Evitar que se **agoten los recursos de software**
- Evitar **cuellos de botella**
- Optimizar la **carga** usando componentes de infraestructura y software

**Técnicas de escalamiento (las 3 del profe):**

| Técnica | Cómo funciona | Ejemplo |
|---|---|---|
| **Ocultar latencias** | Evitar esperar la respuesta remota | Validar el formulario completo en el navegador antes de enviarlo |
| **Ocultar la distribución** | Particionar componentes en partes más pequeñas | Zonas de DNS |
| **Ocultar la replicación** | Guardar copias cerca del usuario | Uso de **caché** y **CDN** |

> ⚠️ **Preguntas de parcial que salieron:**
> - *"Al diseñar, el componente donde tenemos **máximo control** y podemos tener mayor impacto en la escalabilidad"* → **Código de la aplicación**
> - *"...donde tenemos **mínimo impacto**"* → **Ancho de banda e Internet** (eso no lo controlas tú)
> - *"Técnica clásica para escalar **horizontalmente** una app web"* → **Añadir más instancias de servidores detrás de un balanceador de carga**
> - *"Una app escala globalmente usando CDNs porque..."* → **Replican contenido en múltiples ubicaciones cercanas a los usuarios, reduciendo el RTT**
> - *"Ejemplo de pérdida de escalabilidad"* → **IPv4** (se agotó, no crece)
> - *"Dividir y vencer (divide & conquer) aplicado a procesamiento distribuido"* → **Algoritmos MapReduce / procesamiento paralelo de datos**

### 6.5 Manejo de fallas
- Los sistemas fallan, y los fallos conducen a **resultados inexactos**.
- En un SD las fallas son **PARCIALES**: unos componentes fallan mientras otros siguen. Esto lo hace complejo *y* relevante.
- Aspectos a considerar:
  - **Detectar** fallas
  - **Enmascarar** fallas (retransmitir un mensaje, replicar datos en varios discos)
  - **Tolerar** las fallas
  - **Recuperarse** de fallas
  - **Redundancia**

> ⚠️ **Pregunta de parcial:** *"En sistemas distribuidos, un timeout se usa para..."* → **Detectar fallos potenciales**.

### 6.6 Concurrencia
- Los servicios pueden ser accedidos **al mismo tiempo por múltiples usuarios**.
- Pregunta clave del profe: *¿qué pasa si el proceso que gestiona un recurso compartido atiende varias peticiones al tiempo? ¿Qué pasa con el throughput?*

### 6.7 Transparencia (⭐ el tema más preguntado)

> Hacer que ciertos aspectos de la distribución sean **invisibles** para el programador y para el usuario final.
>
> **Mientras más transparencia ofrece un sistema, menos detalles técnicos necesita conocer el usuario.**

| Tipo de transparencia | ¿Qué oculta? |
|---|---|
| **Acceso** | Oculta las **diferencias en la representación de los datos** y en la forma de acceder a ellos |
| **Ubicación (Localización)** | Oculta **dónde** está el recurso |
| **Migración** | Oculta que un recurso **pudiera moverse** a otra ubicación |
| **Reubicación** | Oculta que un recurso se mueve **mientras está en uso** |
| **Replicación** | Oculta **cuántas copias** hay del recurso |
| **Concurrencia** | Oculta que el recurso es **compartido por varios usuarios** que compiten por él |
| **Falla** | Oculta la **falla y recuperación** de un recurso |
| **Persistencia** | Oculta si el recurso está en memoria o en disco |

> ⚠️ **Preguntas de parcial:**
> - *"¿Cuál NO es una transparencia?"* → **Versionado** (las demás — acceso, ubicación, concurrencia — sí lo son)
> - *"Un balanceador de carga introduce principalmente..."* → **Transparencia de acceso**
> - *"Un sistema de carpeta como OneDrive proporciona transparencia de (múltiple)..."* → **ubicación, movilidad (migración/reubicación) y acceso**

> 🧠 **Analogía general:** cuando pides comida por una app, no sabes en qué cocina se preparó, ni si el restaurante cambió de local, ni cuántos domiciliarios hay. **Eso es transparencia.**

---

## 7. Las 8 (+1) suposiciones erróneas — *Fallacies of Distributed Computing*

Declaradas por **Peter Deutsch**. Son los errores clásicos de diseño:

1. La red es **confiable**
2. La red es **segura**
3. La red es **homogénea**
4. La **topología no cambia**
5. La **latencia es cero**
6. El **ancho de banda es infinito**
7. El **costo de transporte es cero**
8. Existe **un solo administrador**
9. (Extra del profe) Usar el clúster **agiliza** automáticamente

> ⚠️ **Pregunta de parcial:** *"Un error de diseño frecuente en SD es asumir:"* → **Latencia despreciable y red confiable/estática**.
> (Ojo: "Heterogeneidad inevitable" y "Administración multi-dominio" **NO** son errores — esas son verdades.)

> 🧠 **Analogía:** es como planear un viaje asumiendo que nunca hay trancón, que la gasolina es gratis y que el carro nunca se daña. Puede que funcione una vez… pero no diseñes tu vida con eso.

---

## 8. Características de los SD (resumen del profe)

- **Ausencia de memoria común**
- **Sincronización del trabajo**
- **Ausencia de un estado global** perceptible por un observador
- **Comunicación a través de mensajes**

### Comunicación

Los componentes están separados lógica y físicamente → **tienen que comunicarse**.

Comunicarse implica dos operaciones:
- La **transferencia de datos**
- La **sincronización** de la recepción con la emisión

**Dos enfoques:**
- **Paso de mensajes**
- **Llamado a procedimiento remoto (RPC)**

**Dos modelos:**
- Comunicación **par a par** (1 a 1)
- Comunicación **grupal** (1 a muchos)

---

## 9. Otros aspectos importantes

| Aspecto | Qué es |
|---|---|
| **Tolerancia a fallos** | El sistema **sigue funcionando** (tal vez más lento) aunque falle un componente. Dos enfoques: **redundancia de hardware** (componentes duplicados) y **recuperación por software** (programas diseñados para recuperarse) |
| **Confiabilidad** | Los datos viajan por vías de comunicación → **pueden perderse o modificarse** |
| **Disponibilidad** | Proporción del tiempo en que el sistema está disponible. En un sistema **centralizado**, si falla la máquina **todos** se quedan sin servicio. En uno **distribuido**, solo se afecta el trabajo que usaba ese componente; el usuario puede moverse a otra estación o el servidor reiniciarse en otra máquina |
| **Concurrencia** | Ejecución **intercalada** si hay un solo procesador, **simultánea** si hay N procesadores |
| **Reconfiguración** | Habilidad de **acomodar cambios a cualquier escala** sin afectar el sistema |

> ⚠️ **Pregunta de parcial:** *"¿Cuáles métricas permiten evaluar la **disponibilidad** de un sistema?"* → **uptime** (y en la versión de selección múltiple, también *denegación de servicio*). Ojo: *tiempo de respuesta* y *transacciones por segundo* miden **rendimiento**, no disponibilidad; *número de usuarios* mide carga.

---

## 10. Clasificación de Flynn

Clasifica arquitecturas según cuántos **flujos de instrucciones** y cuántos **flujos de datos** manejan.

| Sigla | Significado | Qué es | Ejemplo |
|---|---|---|---|
| **SISD** | *Single Instruction, Single Data* | Una instrucción sobre un dato. Arquitectura clásica de **Von Neumann** | Una CPU normal de un solo núcleo |
| **SIMD** | *Single Instruction, Multiple Data* | **Una instrucción se ejecuta simultáneamente sobre múltiples conjuntos de datos** | **GPU**, procesadores vectoriales, *array processors* |
| **MISD** | *Multiple Instruction, Single Data* | Múltiples instrucciones sobre el mismo dato (raro, casi teórico) | Sistemas redundantes críticos |
| **MIMD** | *Multiple Instruction, Multiple Data* | Muchas instrucciones sobre muchos datos. **Aquí viven los sistemas distribuidos** | Multiprocesadores y multicomputadores |

### El árbol completo (Tanenbaum)

```
                Parallel Computer Architectures
        ┌────────┬──────────┬──────────┬──────────┐
      SISD      SIMD       MISD       MIMD
   (Von Neumann) │                      │
        ┌────────┴────────┐    ┌────────┴─────────┐
   Vector proc.  Array proc.  Multiprocesadores  Multicomputadores
                              (memoria           (memoria
                               compartida)        distribuida)
                              ┌────┴────┐        ┌────┴────┐
                             UMA      NUMA      MPP     Cluster
                        FUERTEMENTE acoplados   DÉBILMENTE acoplados
```

- **Multiprocesadores** → memoria **compartida**, comunicación por **lectura/escritura en memoria**, **fuertemente acoplados**
- **Multicomputadores** → memoria **distribuida**, comunicación por **send/recv de mensajes por red**, **débilmente acoplados** ← **aquí están los sistemas distribuidos**

> ⚠️ **Pregunta de parcial:** *"¿Cuál describe mejor un sistema SIMD?"* → **Un sistema donde una instrucción se ejecuta simultáneamente sobre múltiples conjuntos de datos.**

---

## 11. Acoplamiento

| | **Fuertemente acoplado** | **Débilmente acoplado** |
|---|---|---|
| Retraso al enviar mensajes | **Corto** | **Grande** |
| Transmisión de datos | **Alta** | **Baja** |
| Conexión física | Equipos conectados por cables en tarjetas | Dos computadores conectados por red |
| Se usan como | Sistemas **paralelos** | Sistemas **distribuidos** (problemas no relacionados entre sí) |
| Velocidad de intercambio | A velocidad de **memoria** | Algunas fibras alcanzan velocidad de memoria |

> 🧠 **Analogía:** fuertemente acoplado = dos personas trabajando en el mismo escritorio, se pasan papeles al instante. Débilmente acoplado = dos personas en ciudades distintas mandándose correos.

---

## 12. Tipos de Sistemas Distribuidos (resumen del curso)

- **SD de cómputo**
  - **Clúster** (máquinas iguales, misma red local, mismo objetivo)
  - **Grid / Malla** (máquinas distintas, distintas organizaciones)
- **SD de información**
  - Procesamiento de transacciones
  - Integración de aplicaciones empresariales
- **SD embebidos** (sensores, IoT)

---

## 📊 Tabla resumen — Presentación 3

| Tema | Lo mínimo que debes saber |
|---|---|
| **Definición de SD** | Componentes en redes distintas + se comunican por **paso de mensajes** + parecen **un solo sistema coherente** |
| **Por qué** | Mejor rendimiento, escalabilidad y disponibilidad, a menor costo |
| **4 consecuencias** | Concurrencia · Sin reloj global · Independencia (fallas parciales) · Sin memoria compartida |
| **Retos** | Comunicación, coordinación, concurrencia, naming, datos, tolerancia a fallas, seguridad |
| **7 metas de diseño** | Heterogeneidad, Apertura, Seguridad, Escalabilidad, Manejo de fallas, Concurrencia, **Transparencia** |
| **Tríada de seguridad** | Confidencialidad · Integridad · Disponibilidad |
| **Transparencias** | Acceso, Ubicación, Migración, Reubicación, Replicación, Concurrencia, Falla, Persistencia |
| **NO es transparencia** | **Versionado** |
| **Técnicas de escalamiento** | Ocultar latencia · Ocultar distribución (particionar) · Ocultar replicación (caché/CDN) |
| **Máximo control para escalar** | **Código de la aplicación** |
| **Mínimo impacto para escalar** | **Ancho de banda e Internet** |
| **Escalar horizontalmente** | Más instancias detrás de un **balanceador de carga** |
| **8 falacias (Deutsch)** | Red confiable, red segura, red homogénea, topología fija, latencia cero, ancho de banda infinito, transporte gratis, un solo admin |
| **Timeout sirve para** | **Detectar fallos potenciales** |
| **Métrica de disponibilidad** | **uptime** |
| **Flynn** | SISD · **SIMD** (una instrucción, muchos datos → GPU) · MISD · **MIMD** (los SD) |
| **Acoplamiento** | Fuerte = paralelo, memoria compartida · Débil = distribuido, red |
| **Tipos de SD** | Cómputo (clúster, grid) · Información (transacciones, EAI) · Embebidos |

---

## 📖 Glosario — Presentación 3

| Palabra | En cristiano |
|---|---|
| **Sistema Distribuido (SD)** | Muchos computadores que se ven como uno solo para el usuario |
| **Nodo** | Cada computador que participa en el sistema distribuido |
| **Paso de mensajes** | Forma de comunicarse mandándose "cartas" en vez de compartir memoria |
| **Reloj global** | Un reloj único para todos. **En un SD no existe** — de ahí muchos problemas |
| **Falla parcial** | Que se dañe un pedazo del sistema mientras el resto sigue funcionando |
| **Concurrencia** | Varias cosas ocurriendo al mismo tiempo |
| **Naming** | Cómo se le pone nombre a los recursos y cómo se localizan |
| **Heterogeneidad** | Que las piezas del sistema sean distintas (SOs, lenguajes, hardware) |
| **Openness / Apertura** | Que el sistema se pueda extender; requiere publicar interfaces documentadas |
| **Interfaz como contrato** | Publicar qué hace un componente para que otros lo usen sin conocer su código |
| **Transparencia** | Ocultar la complejidad de la distribución al usuario y al programador |
| **Transparencia de acceso** | Oculta cómo se representan y se accede a los datos |
| **Transparencia de ubicación** | Oculta dónde está el recurso |
| **Transparencia de migración** | Oculta que el recurso puede moverse |
| **Transparencia de reubicación** | Oculta que se mueve **mientras lo estás usando** |
| **Transparencia de replicación** | Oculta cuántas copias hay |
| **Transparencia de falla** | Oculta que algo se cayó y se recuperó |
| **Escalabilidad** | Aguantar más carga sin degradarse |
| **Escalamiento horizontal** | Agregar **más máquinas** |
| **Escalamiento vertical** | Hacer **más grande** una máquina (más RAM/CPU) |
| **Cuello de botella** | El punto más lento que limita todo el sistema |
| **Caché** | Copia temporal cercana de datos para no ir hasta el origen |
| **CDN** | Red de servidores repartidos por el mundo que replican contenido cerca del usuario |
| **RTT** | *Round Trip Time*: lo que tarda un dato en ir y volver |
| **Balanceador de carga (LB)** | Reparte las peticiones entre varios servidores |
| **Tolerancia a fallos** | Seguir funcionando aunque algo se dañe |
| **Redundancia** | Tener componentes de repuesto/duplicados |
| **Enmascaramiento de fallas** | Tapar la falla para que el usuario no la note (reintentar, usar la copia) |
| **Disponibilidad** | % del tiempo que el sistema está usable. Se mide con **uptime** |
| **Throughput** | Cantidad de trabajo por unidad de tiempo (transacciones/segundo) |
| **Timeout** | Tiempo máximo de espera. Sirve para **detectar fallos** |
| **Falacias de Deutsch** | Las 8 suposiciones erróneas clásicas al diseñar SD |
| **Flynn** | Clasificación SISD / SIMD / MISD / MIMD |
| **SIMD** | Una instrucción sobre muchos datos a la vez (GPU) |
| **MIMD** | Muchas instrucciones sobre muchos datos (SD y multiprocesadores) |
| **UMA / NUMA** | Tipos de acceso a memoria compartida en multiprocesadores |
| **MPP / Cluster** | Multicomputadores de memoria distribuida |
| **Acoplamiento fuerte/débil** | Qué tan rápido y dependiente es el intercambio entre componentes |
| **Clúster** | Muchas máquinas iguales, misma red, trabajando como una sola |
| **Grid** | Máquinas heterogéneas de distintas organizaciones colaborando |
