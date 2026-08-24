# Presentación 2 — Computación en la Nube

> **La idea en una frase:** la nube es **usar el computador de otra persona**, pagando solo por el tiempo y la cantidad que usas.

---

## 1. Definición oficial

> **Computación en la nube:** utilizar recursos informáticos remotos que están bajo **propiedad y gestión de otra persona**.

La fórmula que dio el profe (¡memorízala, es muy "preguntable"!):

```
Centro de datos remoto
      +  Recursos compartidos (VM)
      +  Servicio bajo demanda
      +  Pago por lo que uso
      +  Acceso vía Internet
      =  NUBE
```

> 🧠 **Analogía del carro:**
> - **On-Premise** = comprar un carro. Pagas todo por adelantado, tú lo mantienes, lo parqueas, le pagas seguro… aunque solo lo uses los domingos.
> - **Nube** = pedir un Uber. Pagas solo el recorrido, no te preocupas por la gasolina ni el mecánico, y si necesitas 5 carros para una fiesta, pides 5.

Es **un enfoque conceptual de la informática**, no una tecnología concreta: encapsula cualquier tipo de enfoque informático.

---

## 2. Las 5 características esenciales (NIST)

Estas cinco son *la* definición formal. Si un servicio no las tiene todas, no es realmente nube.

| # | Característica | En cristiano |
|---|---|---|
| 1 | **Auto-servicio por demanda** (*on-demand self-service*) | Yo mismo pido más servidores desde un panel, **sin llamar a nadie** del proveedor |
| 2 | **Acceso ubicuo a la red** (*broad network access*) | Se accede desde cualquier dispositivo (PC, celular, tablet) y desde cualquier lado |
| 3 | **Agrupación de recursos** (*resource pooling*) | Un mismo hardware sirve a muchos clientes; **a ti no te importa dónde está físicamente** |
| 4 | **Rápida elasticidad** (*rapid elasticity*) | Los recursos crecen y se encogen solos según la demanda |
| 5 | **Servicio medido** (*measured service*) | Todo se mide, se monitorea y se te informa → por eso puedes pagar por uso |

> 🧠 **Truco para recordarlas:** *"Me sirvo solo (1), desde donde sea (2), de una bodega compartida (3), estirando o encogiendo lo que pido (4), y con el taxímetro corriendo (5)."*

---

## 3. Ventajas

| Ventaja | Por qué |
|---|---|
| **Reducción de costos / costo-eficiencia** | No inviertes en hardware, mantenimiento ni licencias. Pagas solo lo usado |
| **Optimización de recursos / escalabilidad** | Recursos disponibles solo cuando se necesitan. **Elimina la planeación de capacidad por adelantado** |
| **Administración / servicios gestionados** | El proveedor se encarga de mantenimiento, actualizaciones, seguridad |
| **Disponibilidad** | Acceso desde cualquier lugar |
| **Fácil recuperación** | Los recursos están replicados en distintas ubicaciones geográficas |
| **Rapidez y agilidad** | Arrancas proyectos más rápido, prototipas en minutos |
| **Seguridad** | Cifrado, gestión de identidades, cumplimiento de normas internacionales |

---

## 4. Desventajas

| Desventaja | Por qué duele |
|---|---|
| **Percepción de inseguridad** | Da desconfianza guardar datos confidenciales en infraestructura compartida |
| **Pérdida de control** | No tienes acceso físico al lugar donde están tus datos |
| **Dependencia de Internet** | Sin conexión, no hay recursos |
| **Dependencia del proveedor** (*vendor lock-in*) | Migrar es difícil: APIs, herramientas y formatos distintos → toca reescribir y reconfigurar |
| **Costo de transferencia de datos** | Caro con grandes volúmenes; cobran por mover datos **entre regiones** |
| **Complejidad de gestión de costos** | Muchos servicios con modelos de precio distintos → **costos inesperados** por arquitecturas ineficientes |

> ⚠️ **Muy preguntable:** *"¿Cuál de los siguientes NO es un beneficio de la nube?"* → **Alta latencia**.
> La latencia alta es un **problema**, no un beneficio. Los demás (alta disponibilidad, múltiples ciclos de aprovisionamiento, BD tolerantes a fallas, recursos temporales y descartables) **sí** son beneficios.

---

## 5. Modelos de IMPLEMENTACIÓN (dónde vive la nube)

| | **Nube Privada** | **Nube Híbrida** | **Nube Pública** |
|---|---|---|---|
| **Propiedad** | Una sola organización | Combinación de ambas | Proveedor externo (AWS, Azure, GCP) |
| **Acceso** | Exclusivo de la empresa | Parte privada, parte pública | Cualquiera que contrate |
| **Costo** | Alto (infraestructura propia) | Medio | Bajo / pago por uso |
| **Seguridad** | Muy alta, control total | Alta, según dónde pongas cada dato | Buena, pero con menor control directo |
| **Escalabilidad** | Limitada a lo que tengas | Alta (se apoya en la pública) | Muy alta, casi ilimitada |
| **Latencia de red** | **Baja** | Media | Depende |
| **Mantenimiento** | Lo hace la organización | Compartido | Lo hace el proveedor |
| **Control** | Total | Parcial | Limitado |
| **Implementación** | Lenta y compleja | Compleja (hay que integrar dos mundos) | Muy rápida |
| **Flexibilidad** | Baja | Muy alta | Alta |
| **Casos de uso** | Bancos, hospitales, gobierno, datos sensibles | Datos críticos adentro + picos de demanda afuera | Startups, apps web, desarrollo y pruebas |
| **Ejemplo** | Data center propio de una empresa | Datos sensibles en casa + AWS para picos | AWS, Azure, Google Cloud |

> 🧠 **Analogía de la vivienda:**
> - **Privada** = casa propia. Control total, pero tú arreglas el techo.
> - **Pública** = hotel. Llegas y ya está todo listo, pero sigues reglas ajenas.
> - **Híbrida** = vives en tu casa pero alquilas un hotel cuando llegan 20 invitados.

---

## 6. Modelos de SERVICIO (cuánto administra el proveedor)

La pila de un sistema, de abajo hacia arriba:

```
Hardware → Almacenamiento → Red → Virtualización → SO → Runtime → Aplicación → Funciones
```

Lo único que cambia entre modelos es **dónde se traza la raya** entre "lo administra el proveedor" y "lo administro yo".

| | **On-Premise** | **IaaS** | **PaaS** | **FaaS** | **SaaS** |
|---|---|---|---|---|---|
| Tú administras | **Todo** | SO hacia arriba | App y datos | Solo tu función | Solo tus datos de uso |
| El proveedor administra | Nada | Hasta virtualización | Hasta el runtime | Todo excepto tu código | **Todo** |

### El cuadro que hay que saberse

| | **IaaS** (Infrastructure) | **PaaS** (Platform) | **SaaS** (Software) |
|---|---|---|---|
| **¿Qué ofrece?** | Infraestructura virtual: servidores, redes, almacenamiento, VMs | Plataforma para desarrollar, probar y desplegar apps | Aplicaciones listas para usar por Internet |
| **¿Qué administra el usuario?** | SO, apps, datos y configuraciones | Aplicaciones y datos | Solo el uso de la app y sus datos |
| **Nivel de control** | **Alto** | Medio | **Bajo** |
| **Facilidad de uso** | Baja (requiere conocimiento técnico) | Media | **Muy alta** |
| **Escalabilidad** | Alta | Alta | Alta |
| **Usuario típico** | Administradores de sistemas / TI | **Desarrolladores** | Usuarios finales |
| **Casos de uso** | Hospedar servidores, VMs, almacenamiento, redes | Desplegar apps web o móviles | Correo, ofimática, CRM, videoconferencias |
| **Ventaja principal** | Máxima flexibilidad y control | Simplifica el desarrollo: te olvidas de la infraestructura | Sin instalación ni mantenimiento |
| **Desventaja principal** | Más responsabilidad de administración | Menos control sobre la plataforma | Poca personalización, dependencia del proveedor |
| **Ejemplos** | Amazon EC2, Azure VMs, Google Compute Engine | Google App Engine, Azure App Service, Heroku | Gmail, Microsoft 365, Google Workspace, Salesforce |

> 🧠 **Analogía de la pizza:**
> - **On-Premise** = haces la masa, la salsa y todo desde cero en tu casa.
> - **IaaS** = compras la masa prehecha; tú le pones ingredientes y la horneas.
> - **PaaS** = pides pizza a domicilio; tú solo pones la mesa y las bebidas.
> - **SaaS** = te vas a comer a la pizzería. No lavas ni un plato.

### El mapa mental del profe

| Verbo | Modelo | A quién le sirve |
|---|---|---|
| **Consumir** | SaaS | Usuario de aplicaciones |
| **Desarrollar** | PaaS | Desarrollador |
| **Migrar** | IaaS | Administrador de TI |

### Otros "as a Service" que mencionó

- **DBaaS** — Database as a Service
- **INaaS** — Information as a Service
- **BPaaS** — Business Process as a Service
- **FaaS** — Functions as a Service (serverless: subes una función, no un servidor)

---

## 7. El modelo mental de fondo

> **La infraestructura se ve como software.**

Consecuencias:

- Las soluciones son **flexibles**
- Cambian **más rápido, más fácil y más barato** que las de hardware
- Puedes **escalar los recursos de forma elástica y automatizada**
- Los recursos se pueden ver como **"temporales" y "desechables"**

> 🧠 **Analogía "mascotas vs ganado":** antes cada servidor era una mascota con nombre, y si se enfermaba lo cuidabas. En la nube los servidores son ganado: si uno falla, lo reemplazas por otro idéntico y sigues.

---

## 8. Números del mercado

| Trimestre | Gasto mundial en infraestructura cloud | AWS + Azure + Google | Top 8 proveedores |
|---|---|---|---|
| Q4 2023 | 73.700 millones USD | 66% | >80% |
| Q3 2024 | 84.000 millones USD | 63% | >76% |
| **Q1 2026** | **129.000 millones USD** | **63%** | **>77%** |

> 💡 Lo que hay que retener: el mercado **crece muy rápido** y está **muy concentrado** en pocos proveedores (los tres grandes se llevan ~2/3).

---

## 9. ¿Por qué usar la nube? (razones de negocio)

- **Valor estratégico:** *TTM* (Time To Market) más rápido
- **Reduce el CAPEX:** cero inversión inicial en infraestructura
- **Aumenta el ROI**
- **Reduce el personal de TI necesario**
- **Uso eficiente de los recursos:**
  - Velocidad y capacidad bajo demanda (pago por uso)
  - Aprovisionamiento rápido
  - Escalabilidad elástica

> 🧠 **CAPEX vs OPEX:** CAPEX es comprar un carro (una inversión grande de una vez). OPEX es pagar Uber (un gasto pequeño y continuo). La nube convierte CAPEX en OPEX.

---

## 📊 Tabla resumen — Presentación 2

| Tema | Lo mínimo que debes saber |
|---|---|
| **Definición** | Usar recursos informáticos remotos gestionados por otro |
| **Fórmula** | Datacenter remoto + recursos compartidos (VM) + bajo demanda + pago por uso + acceso por Internet |
| **5 características NIST** | Auto-servicio bajo demanda · Acceso ubicuo · Agrupación de recursos · Elasticidad rápida · Servicio medido |
| **Ventajas** | Costo-eficiencia, escalabilidad, servicios gestionados, disponibilidad, recuperación, agilidad, seguridad |
| **Desventajas** | Percepción de inseguridad, pérdida de control, dependencia de Internet, **vendor lock-in**, costo de transferencia, gestión de costos compleja |
| **Modelos de implementación** | Privada (control total, baja latencia) · Híbrida (flexible) · Pública (elástica, pago por uso) |
| **Modelos de servicio** | IaaS (control alto, admins) · PaaS (desarrolladores) · SaaS (usuarios finales) · FaaS (serverless) |
| **Mapa de verbos** | Migrar→IaaS · Desarrollar→PaaS · Consumir→SaaS |
| **NO es beneficio de la nube** | **Alta latencia** ← trampa típica de parcial |
| **Razones de negocio** | TTM rápido, ↓CAPEX, ↑ROI, ↓personal TI, elasticidad |

---

## 📖 Glosario — Presentación 2

| Palabra | En cristiano |
|---|---|
| **Nube (Cloud)** | Usar servidores, apps y almacenamiento de otra persona por Internet |
| **Data center** | Edificio lleno de servidores donde "vive" la nube |
| **VM (Máquina Virtual)** | Un computador "de mentiras" que corre dentro de un computador real |
| **Virtualización** | La técnica que permite crear varias VMs en un solo servidor físico |
| **On-demand (bajo demanda)** | Pedir recursos en el momento en que los necesitas, sin trámite |
| **Elasticidad** | Que los recursos crezcan y se encojan solos según la demanda |
| **Escalabilidad** | Que el sistema aguante más carga sin dejar de funcionar bien |
| **Resource pooling** | Muchos clientes compartiendo el mismo hardware sin darse cuenta |
| **Measured service** | Todo se mide y se te reporta → base del "pago por uso" |
| **Aprovisionamiento** | El acto de crear/entregar un recurso (levantar un servidor) |
| **Pago por uso** | Pagas solo lo que consumiste, como la luz o el agua |
| **IaaS** | Te alquilan la infraestructura cruda (VMs, red, disco) |
| **PaaS** | Te alquilan una plataforma lista para desplegar tu código |
| **SaaS** | Te alquilan la aplicación terminada (Gmail, Salesforce) |
| **FaaS / Serverless** | Subes solo una función; no administras ningún servidor |
| **DBaaS / INaaS / BPaaS** | Base de datos / Información / Proceso de negocio como servicio |
| **Nube pública** | Del proveedor, compartida, abierta a quien pague |
| **Nube privada** | Tuya, exclusiva, control total, cara |
| **Nube híbrida** | Mezcla de las dos |
| **Vendor lock-in** | Quedar "amarrado" a un proveedor porque migrar sale carísimo |
| **CAPEX** | *Capital Expenditure*: inversión grande de una vez (comprar servidores) |
| **OPEX** | *Operational Expenditure*: gasto recurrente (alquilar en la nube) |
| **ROI** | *Return On Investment*: cuánto ganas frente a lo que invertiste |
| **TTM** | *Time To Market*: qué tan rápido sacas tu producto al mercado |
| **Latencia** | Tiempo que tarda un dato en ir de un punto a otro. **Menos es mejor** |
| **Región / Zona** | Ubicación geográfica del data center. Mover datos entre regiones **cuesta** |
