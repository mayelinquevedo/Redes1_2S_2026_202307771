# Manual Técnico , Práctica 1
# Universidad de San Carlos de Guatemala, Facultad de Ingeniería, 
# Escuela de Ciencias y Sistemas
# Laboratorio Redes de computadoras 1
# Nombre: Mayelin Lucía Quevedo Camey , carnet: 2023007771
# Fecha: 22 de Agosto año 2026

## 1. Inventario de equipos (switches, patch panels, racks/gabinetes, tomas de red, y demás equipo propuesto). 

## 2. Justificación de la ubicación del cuarto de  telecomunicaciones (MDF). 
**Ubicación seleccionada:** Dentro del departamento de Data Center, en donde colinda la sala de Data Center con Diseño e Innovación y Dirección General

**Justificación:**

Al inicio estuve considerando dos opciones principales para la ubicación del MDF: el hall central (por su centralidad en el plano) y el Data Center (por seguridad y condiciones ambientales). opté finalmente por el **Data Center**, priorizando los siguientes criterios técnicos sobre la mínima distancia geométrica:

- **Seguridad física (ANSI/TIA-569):** El hall central es una zona de tránsito continuo (personal, visitas, limpieza), lo que expone el equipo activo a manipulaciones no autorizadas, polvo y golpes accidentales. El Data Center, al ser un espacio cerrado de acceso restringido, protege adecuadamente el switch principal, el patch panel y el UPS.

- **Condiciones ambientales:** El equipo activo requiere un entorno con temperatura y humedad controladas. Un pasillo abierto no permite climatización dedicada, mientras que el Data Center puede acondicionarse para tal fin.

- **Coherencia operativa:** El Data Center ya aloja los 3 servidores principales de la empresa; concentrar ahí también el MDF permite compartir el mismo sistema de seguridad, climatización y respaldo eléctrico (UPS), evitando duplicar infraestructura de soporte en dos ubicaciones distintas.

- **Impacto en la distancia** Al Ubicar el MDF en el Data Center, en lugar del hall central, incrementa el total de cableado troncal de forma marginal (aproximadamente 111metros), lo cual sigue representando una sola bobina de 305 m. Dado que el sobrecosto en cableado es mínimo frente al beneficio de seguridad y control ambiental.

- **Punto exacto dentro del Data Center:** Se ubica en la esquina del Data Center más próxima al Vestíbulo de Ingreso y a la fila de departamentos superior (Diseño e Innovación / Dirección General), en lugar del fondo del cuarto, con el fin de minimizar la distancia hacia el resto del edificio dentro de las limitaciones de la opción elegida.

## 3. Justificación de la topología física seleccionada por cada departamento. 
El enunciado de la práctica menciona que cada departamento tiene solo un switch al que se conectan todos sus respectivos hosts, esto me llevó a la conclusión de que la topología más adecuada siempre será topología de estrella, para todos los departamentos

Y para la red completa del edificio: es una topología de árbol / estrella extendida,
con el MDF como raíz y cada switch de departamento como rama.


| Departamento | Hosts | Actividad | TOPOLOGIA | Justificación |
| :--- | :---: | :--- | :---: | :--- |
| Recepción | 3 +<br>1 servidor | punto de entrada, 1 servidor | **Estrella** | Pocos hosts, estrella es la opción más económica; suficiente para el tráfico bajo de esta área |
| Recursos Humanos | 8 | datos de empleados | **Estrella** | El switch aísla su tráfico del resto; con 8 hosts no se justifica costo extra de árbol/malla |
| Legal | 4 | documentos confidenciales | **Estrella** | Segmento pequeño pero sensible; la estrella permite aplicar seguridad a nivel de switch sin sobrecosto |
| Capacitación | 10 | uso temporal/rotativo | **Estrella** | Mayor cantidad de hosts del edificio; estrella escala bien dejando puertos libres para picos de uso |
| Diseño e Innovación | 7 +<br>1 servidor | incluye QA y un servidor | **Estrella** | Aísla fallos: si un host falla no afecta al servidor ni a los demás puestos |
| Dirección General | 4 | gestión estratégica | **Estrella** | Segmento pequeño; estrella simple es suficiente y fácil de asegurar físicamente |
| Backend | 6 +<br>1 servidor | servidor de desarrollo | **Estrella** | Prioriza tolerancia a fallos por host individual; costo bajo para 6 puestos |
| Data center | 3 servidores | servidores principales | **Estrella** | Con solo 3 nodos, estrella es más que suficiente; los 3 servidores no compiten por ancho de banda entre sí |


## 4. Tabla con el tipo y categoría de cable utilizado por segmento, con su justificación.

Utilicé cable de cobre UTP en la totalidad del edificio (troncal y horizontal). 
sin utilizar fibra óptica, ya que las distancias máximas registradas en el diseño son menores de 30 metros. están muy por debajo del límite de 100 m que soporta el cableado de cobre según el estándar TIA/EIA-568, 
por lo que el uso de fibra significaría un sobrecosto injustificado para las necesidades del edificio.

Categoría de cable: Seleccioné UTP Cat6 de manera uniforme para todos los segmentos (troncal y horizontal), por las siguientes razones:
- Soporta velocidades de hasta 10 Gbps a distancias de hasta ~55 m, y hasta 1 Gbps a los 100 m completos esto es más que suficiente para las distancias del diseño.
- Es compatible con la capacidad Gigabit de los switches (Cisco 2960-24TT).
- Deja margen de crecimiento a futuro sin necesidad de recablear, en caso de actualizar el equipo.


| Segmento | Tipo de cable | Categoría | Justificación |
| :--- | :--- | :--- | :--- |
| Troncal (MDF ↔ switches de cada uno de los departamentos) | UTP | Cat6 | las Distancias son menores que 30 metros (≤ 30 m), compatible con uplink Gigabit |
| Troncal (MDF ↔ switch de Data Center) | UTP | Cat6 | El Enlace es mucho más corto (3 m) y cabe destacar que es dentro del mismo cuarto |
| Horizontal (switch ↔ hosts) | UTP | Cat6 | Estándar homogéneo, soporta Gigabit por host, escalable a futuro |





## 5. Justificación de cada elemento de equipo activo, con el dimensionamiento de patch panel y switch del edificio. 



| Departamento | Hosts | Dispositivos | Switch  | Puertos libres |
| :--- | :---: | :--- | :---: | :---: |
| Recepción | 4 | 3 computadoras + 1 servidor | 8 puertos | 4 |
| RRHH | 8 | 8 computadoras | 8 puertos | 0 |
| Legal | 4 | 4 computadoras | 8 puertos | 4 |
| Capacitación | 10 | 10 computadoras | 16 puertos | 6 |
| Diseño e Innovación | 8 | 7 computadoras + 1 servidor | 8 puertos | 0 |
| Dirección General | 4 | 4 computadoras | 8 puertos | 4 |
| Backend | 7 | 6 computadoras + 1 servidor | 8 puertos | 1 |
| Data Center | 3 | 3 computadoras | 8 puertos | 5 |



## 6. Justificación del medio de transmisión seleccionado para el cableado troncal (MDF - switches de departamento).

El Cable que seleccioné fue el ,  cobre UTP Categoría 6, en todos los enlaces troncales (MDF ↔ switch de cada departamento, incluyendo Data Center).

Los switches de departamento (Cisco 2960-24TT) operan a velocidades Gigabit (10/100/1000 Mbps) en sus puertos. UTP Cat6 soporta hasta 1 Gbps en el límite completo de 100 m, y hasta 10 Gbps en distancias de hasta ~55 m — en ambos casos, muy por encima de lo que estos switches pueden generar como tráfico de uplink. No se requiere una categoría superior (Cat6a) ni fibra óptica para alcanzar esta velocidad.

**Distancia:**
El enlace troncal más largo del diseño (MDF → Recepción) mide aproximadamente 25.3 metros(hacia recepción) con holgura incluida, y el resto de los enlaces son aún más cortos. Todos están muy por debajo tanto del límite de 100 m del estándar TIA/EIA-568 para cobre, como del límite de ~55 m donde Cat6 empieza a perder capacidad de 10 Gbps — por lo que la señal viaja sin atenuación significativa en ningún segmento.

**Costo y escalabilidad:**
El cobre UTP Cat6 es significativamente más económico que la fibra óptica.Dado que las distancias del edificio son cortas y no existe justificación técnica de ancho de banda que exija fibra, usarla representaría un sobrecosto sin beneficio real.
 Cat6 además nos deja margen de escalabilidad: si en el futuro se actualiza el equipo activo a switches con capacidad de 10 Gbps, el cableado troncal ya instalado seguiría siendo compatible (dentro del límite de ~55 m), evitando un recableado completo.



## 7. Justificación del tipo de canalización utilizada (escalerilla abierta/cerrada u otro medio). 

**Cableado troncal (MDF ↔ switches de departamento):** Se utiliza **escalerilla metálica tipo charola cerrada**, instalada sobre el cielo raso, recorriendo el Hall Central para distribuir los 8 enlaces troncales hacia ambas filas de departamentos. Se elige tipo cerrada (en lugar de abierta) porque:
- Protege los cables de polvo, golpes accidentales y manipulación no autorizada, dado que el hall es una zona de tránsito constante.
- Al agrupar los 8 troncales en un mismo canal, facilita el orden, la identificación y el mantenimiento futuro (agregar o reemplazar un cable sin afectar a los demás).
- Es una solución económica y de fácil instalación para un edificio de un solo nivel, sin necesidad de canalización subterránea o en muros.

**Cableado horizontal (switch de departamento ↔ hosts):** Se utiliza **canaleta de PVC superficial**, adosada a pared o zócalo dentro de cada oficina, para conectar cada toma de red con el switch de su departamento. Se elige canaleta en lugar de escalerilla porque:
- Los tramos son cortos (dentro de una misma sala) y de bajo volumen de cables por punto, no se justifica el costo de una escalerilla metálica dentro de cada oficina.
- Es más discreta estéticamente para espacios de trabajo (oficinas, sala de capacitación, dirección), a diferencia del hall que es una zona técnica de paso.
- Facilita el acceso para mantenimiento o reubicación de tomas de red sin necesidad de intervenir el cielo raso.


| Segmento | Tipo de canalización | Justificación breve |
|---|---|---|
| Troncal (Hall Central) | Escalerilla metálica cerrada | Protege 8 enlaces agrupados en zona de tránsito |
| Horizontal (dentro de cada oficina) | Canaleta de PVC superficial | Económica, discreta, suficiente para tramos cortos |



## 8. Justificación del tipo de rack o gabinete propuesto para el MDF.

**Rack de piso (floor-standing), de 24U, ubicado dentro del Data Center.**

hice la elección un gabinete de pared porque el MDF contiene una cantidad considerable de equipo activo y pasivo que debe convivir en el mismo espacio:

| Equipo | Unidades de rack (U) aprox. |
|---|---|
| Patch panel (48 puertos) | 2U |
| Switch principal (48 puertos) | 1U |
| Switch de Data Center | 1U |
| Servidores (3, rack-mount) | 4-6U |
| UPS (rack-mount) | 2-4U |
| **Total ocupado aprox.** | **10-14U** |

Un gabinete de pared típico (6-12U) resultaría insuficiente o quedaría al límite sin espacio para crecimiento. Un **rack de piso de 24U** deja margen suficiente para el equipo actual más una reserva de crecimiento futuro.
Al ubicarlo dentro del Data Center se aprovechan las ventajas de seguridad física y control ambiental.

Adicionalmente, un rack de piso facilita la gestión de cableado vertical (organizadores pasacables) para los 8 enlaces troncales y los cables de los servidores, algo más difícil de lograr en un gabinete de pared de menor profundidad.


## 9. Estimación del consumo eléctrico y capacidad de UPS requerida. 

**Consumo estimado del equipo activo (switch principal + switches de departamento):**

| Equipo | Consumo típico | Cantidad | Subtotal |
|---|---|---|---|
| Switch principal (48 puertos) | 50 W | 1 | 50 W |
| Switch Data Center (8 puertos) | 15 W | 1 | 15 W |
| Switches de departamento (8 puertos) | 15 W | 6 | 90 W |
| Switch Capacitación (16 puertos) | 20 W | 1 | 20 W |
| **Total consumo de switches** | | | **175 W** |

**Margen de seguridad (25%):**
175 W × 1.25 = 218.75 W ≈ **220 W**

**Conversión a VA** (factor de potencia típico de UPS ≈ 0.9):
220 ÷ 0.9 ≈ **245 VA**

**UPS:** Un UPS comercial de **600 VA / 360 W**  es más que suficiente para cubrir el equipo de red, con amplio margen de crecimiento.

### Servidores Principales (Data Center)

**Consumo estimado de los servidores:**

| Equipo | Consumo típico bajo carga | Cantidad | Subtotal |
|---|---|---|---|
| Servidor rack 1U (uso típico corporativo) | 350 W | 3 | 1,050 W |


**Margen de seguridad (25%):**
1,050 W × 1.25 = 1,312.5 W ≈ **1,315 W**

**Conversión a VA** (factor de potencia ≈ 0.9):
1,315 ÷ 0.9 ≈ **1,461 VA**

**UPS recomendado para servidores:** Un UPS de línea interactiva u online de **2,200 VA / 1,980 W** , ya que ofrece margen suficiente para picos de carga y es acorde a la criticidad del Data Center.

###  UPS del Cuarto de Telecomunicaciones

| Carga | UPS recomendado |
|---|---|
| Switches (principal + departamentales) | 600 VA / 360 W |
| Servidores principales | 2,200 VA / 1,980 W |
| **Opción alternativa: UPS único combinado** | **3,000 VA / 2,700 W**, cubriendo ambas cargas con un solo equipo y margen adicional de crecimiento |


## 10. Tabla indicando straight-through/cros

Esto depende si conectamos dispositivos del mismo tipo o de tipo distinto.

-Dispositivos distintos (PC ↔ switch, switch ↔ patch panel) → cable straight-through (mismo estándar en ambos extremos: T568B-T568B, o T568A-T568A).
-Dispositivos del mismo tipo (switch ↔ switch) → cable crossover (un extremo T568A, el otro T568B).



| Enlace | Dispositivos que conecta | Tipo de cable | Justificación |
| :--- | :--- | :--- | :--- |
| Host → toma de red / patch panel (cableado horizontal) | PC/laptop/servidor ↔ patch panel | Straight-through | Dispositivos de tipo distinto (host vs. equipo pasivo de red); mismo estándar en ambos extremos, como exige el enunciado |
| Patch panel → switch de departamento | Patch panel ↔ switch | Straight-through | El patch panel solo extiende la señal del host; sigue siendo lógicamente host ↔ switch |
| Switch de departamento → switch principal (MDF) | Switch ↔ switch | Crossover | Dispositivos del mismo tipo (dos switches); requiere invertir pares de transmisión/recepción |

## 11. Disposición de pines documentada (mínimo un  straight-through y un crossover). 



## 12. Tabla de etiquetado de cables. 

## 13. Comparación entre el etiquetado usado y el estándar TIA/EIA-606, con al menos dos diferencias concretas. 


## 14. Descripción del flujo de conexión end-to-end. 


# 15. Presupuesto estimado del equipo y materiales utilizados. 