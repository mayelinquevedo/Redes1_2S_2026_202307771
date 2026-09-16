# Manual Técnico — Proyecto "SmartCity Tech Park"
# Universidad de San Carlos de Guatemala, Facultad de Ingeniería, 
# Escuela de Ciencias y Sistemas
# Laboratorio Redes de computadoras 1
# Nombre: Mayelin Lucía Quevedo Camey , carnet: 202307771
# Fecha: 17 de Septiembre año 2026



---

## Información general 

| Parámetro | Valor |
|---|---|
| Dominio VTP | `Smart_7` (7 penúltimo digito de carnet) |
| Contraseña VTP | `proyecto12S2026` |
| VLAN nativa | `91` (1 último dígito de carnet)|
| Protocolo EtherChannel | PAgP  |
| Protocolo STP | Rapid-PVST  |
| Banner MOTD | `Acceso Restringido - TechPark_202307771` |
| Root Bridge (todas las VLANs) | Core-SW (prioridad 4096) |

---

## Capturas de la topología

> Vista completa de la topología en Packet Tracer (las 5 zonas).
<div align="center">
  <img src="imagenes/topologia-completa.png" alt="MDF">
</div>


> Zona Core / Centro de Datos (Core-SW + Server-SW + 4 servidores).
<div align="center">
  <img src="imagenes/centro-de-datos.png" alt="MDF">
</div>

> Zona Centro de I+D (IyD-SW1, IyD-SW2, IyD-SW3 + 8 PCs).
<div align="center">
  <img src="imagenes/I+D.png" alt="MDF">
</div>

> Zona Edificio Corporativo (Corp-Dist-SW, Ala-A-SW, Ala-B-SW, Access Point, laptops de visitantes).
<div align="center">
  <img src="imagenes/corporativo.png" alt="MDF">
</div>

> Zona Planta de Producción (Prod-SW, Hub, 3 PCs legacy).
<div align="center">
  <img src="imagenes/produccion.png" alt="MDF">
</div>

---

## Tabla de VLANs

| Edificio | VLAN ID | Nombre |
|---|---|---|
Edificio Corporativo  | 11 | GERENCIA | 
| Centro de I+D | 21 | INVESTIGACION | 
Planta de Producción | 31 | PRODUCCION | 
Centro de Datos | 41 | SERVIDORES | 
Edificio Corporativo (WiFi) | 51 | VISITANTES | 
| Todos los enlaces troncales del campus | 91 | (nativa) 

---

## Tabla de asignación de puertos por switch

### Core-SW
| Puerto | Modo | Conecta a | Notas |
|---|---|---|---|
| Fa0/1 | Trunk (Po2) | Server-SW Fa0/5 | EtherChannel PAgP |
| Fa0/2 | Trunk | Corp-Dist-SW Fa0/3 | Enlace simple |
| Fa0/3 | Trunk | Prod-SW Fa0/2 | Enlace simple |
| Fa0/4 | Trunk (Po1) | IyD-SW1 Fa0/3 | EtherChannel PAgP |
| Fa0/5 | Trunk (Po1) | IyD-SW1 Fa0/4 | EtherChannel PAgP |
| Fa0/6 | Trunk (Po2) | Server-SW Fa0/6 | EtherChannel PAgP |

### Server-SW
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 – Fa0/4 | Access | Servidores 1-4 | 41 (SERVIDORES) |
| Fa0/5, Fa0/6 | Trunk (Po1) | Core-SW Fa0/1, Fa0/6 | EtherChannel PAgP |

### IyD-SW1
| Puerto | Modo | Conecta a |
|---|---|---|
| Fa0/1 | Trunk | IyD-SW2 Fa0/1 |
| Fa0/2 | Trunk | IyD-SW3 Fa0/1 |
| Fa0/3, Fa0/4 | Trunk (Po1) | Core-SW Fa0/4, Fa0/5 |

### IyD-SW2
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 | Trunk | IyD-SW1 Fa0/1 | — |
| Fa0/2 | Trunk | IyD-SW3 Fa0/2 | — |
| Fa0/3 – Fa0/6 | Access | PCs de I+D | 21 (INVESTIGACION) |

### IyD-SW3
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 | Trunk | IyD-SW1 Fa0/2 | — |
| Fa0/2 | Trunk | IyD-SW2 Fa0/2 | — |
| Fa0/3 – Fa0/6 | Access | PCs de I+D | 21 (INVESTIGACION) |

### Prod-SW
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 | Access | Hub (3 PCs legacy) | 31 (PRODUCCION) |
| Fa0/2 | Trunk | Core-SW Fa0/3 | — |

### Corp-Dist-SW
| Puerto | Modo | Conecta a |
|---|---|---|
| Fa0/1 | Trunk | Ala-B-SW Fa0/1 |
| Fa0/2 | Trunk | Ala-A-SW Fa0/1 |
| Fa0/3 | Trunk | Core-SW Fa0/2 |

### Ala-A-SW
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 | Trunk | Corp-Dist-SW Fa0/2 | — |
| Fa0/2 | Trunk | Ala-B-SW Fa0/2 (redundancia) | — |
| Fa0/3 | Access | Access Point | 51 (VISITANTES) |
| Fa0/4, Fa0/5 | Access | PC-Ger-03, PC-Ger-04 | 11 (GERENCIA) |

### Ala-B-SW
| Puerto | Modo | Conecta a | VLAN |
|---|---|---|---|
| Fa0/1 | Trunk | Corp-Dist-SW Fa0/1 | — |
| Fa0/2 | Trunk | Ala-A-SW Fa0/2 (redundancia) | — |
| Fa0/3, Fa0/4 | Access | PC-Ger-01, PC-Ger-02 | 11 (GERENCIA) |

---

##  Dominios de colisión

En una red conmutada (switches), **cada puerto activo de un switch es su propio dominio de colisión** (full-duplex punto a punto). La única excepción es el segmento Legacy, donde un Hub (dispositivo de Capa 1) obliga a todos sus dispositivos conectados a compartir un único dominio.

| Switch | Puertos activos | Dominios de colisión que genera |
|---|---|---|
| Core-SW | 6 | 6 |
| Server-SW | 6 | 6 |
| IyD-SW1 | 4 | 4 |
| IyD-SW2 | 6 | 6 |
| IyD-SW3 | 6 | 6 |
| Prod-SW | 2 | 2  |
| Corp-Dist-SW | 3 | 3 |
| Ala-A-SW | 5 | 5 |
| Ala-B-SW | 4 | 4 |

**Dominio de colisión compartido (segmento Legacy):**
El Hub en la Planta de Producción une en un solo dominio de colisión a: el puerto Fa0/1 de Prod-SW **+** las 3 PCs legacy conectadas al Hub. Los 4 dispositivos compiten por el mismo medio físico compartido — cualquier transmisión simultánea de dos de ellos produce una colisión real, a diferencia del resto de la red donde eso es imposible gracias a la conmutación.

*Impacto y medidas de contención : al no poder eliminar el segmento Legacy por la maquinaria industrial antigua, la única mitigación posible está en el puerto de acceso del switch (Fa0/1 de Prod-SW): configurarlo en half-duplex forzado para que coincida con el comportamiento real de un Hub, y aislar ese tráfico en su propia VLAN (31) para que al menos las colisiones y la carga de ese segmento no afecten el dominio de broadcast del resto del campus.*

---

## Dominios de broadcast (uno por VLAN activa)

| VLAN | Dominio de broadcast (switches que lo comparten) |
|---|---|
| 11 (GERENCIA) | Corp-Dist-SW, Ala-A-SW, Ala-B-SW |
| 21 (INVESTIGACION) | IyD-SW1, IyD-SW2, IyD-SW3 |
| 31 (PRODUCCION) | Prod-SW (y el segmento Legacy detrás del Hub) |
| 41 (SERVIDORES) | Server-SW |
| 51 (VISITANTES) | Ala-A-SW (vía Access Point) |

Cada VLAN limita su propio dominio de broadcast exclusivamente a los switches y puertos que la tienen asignada — un broadcast generado por una laptop de visitante (VLAN 51) nunca llega a las PCs de Gerencia (VLAN 11), aunque compartan el mismo switch físico.

---

##  Justificación: Switch Servidor VTP

>  `show vtp status` 

<div align="center">
  <img src="imagenes/Server.png" alt="MDF">
</div>

**Core-SW** fue elegido como servidor VTP porque es el switch más central de la topología (conecta directamente con las 4 áreas del campus). Esto minimiza la distancia lógica que debe recorrer cualquier actualización de VLANs para llegar a todos los switches del dominio, y concentra la administración en el punto de mayor jerarquía de la red — coherente con su rol de Core en un diseño jerárquico de 3 capas.

---

##  Justificación: Root Bridge por VLAN

>  `show spanning-tree vlan 11` 
<div align="center">
  <img src="imagenes/root.png" alt="MDF">
</div>

**Core-SW** se configuró como Root Bridge para las 5 VLANs del proyecto (prioridad 4096, la más baja del campus). 

**Justificación:** al ser el switch con mayor conectividad (6 enlaces activos, incluyendo 2 EtherChannel) y el punto de tránsito obligatorio entre las 4 áreas, colocar el Root ahí minimiza el costo acumulado de las rutas STP hacia cualquier otro punto de la red, evitando rutas subóptimas en forma de "zigzag" entre edificios.

---

##  Justificación: EtherChannel

>  `show etherchannel summary` 

<div align="center">
  <img src="imagenes/su.png" alt="MDF">
</div>


| Port-Channel | Switches | Puertos | Protocolo | Justificación |
|---|---|---|---|---|
| Po1 | Core-SW ↔ IyD-SW1 | Fa0/4-5 / Fa0/3-4 | PAgP | El enunciado exige mayor ancho de banda hacia I+D que el resto de troncales del campus; agregar 2 enlaces físicos duplica la capacidad disponible y evita que I+D quede aislado si un solo cable falla. |
| Po2 | Core-SW ↔ Server-SW | Fa0/1,6 / Fa0/5-6 | PAgP | La granja de servidores concentra alto volumen de tráfico hacia el Core; el EtherChannel asegura que ningún servidor dependa de una única conexión física, cumpliendo el requisito de alta disponibilidad para servicios críticos. |

---

##  Medios de transmisión

>  Topología en Packet Tracer con etiquetas de texto junto a cada enlace indicando el medio (cobre/fibra).
<div align="center">
  <img src="imagenes/topologia-completa.png" alt="MDF">
</div>



| Enlace | Medio | Justificación |
|---|---|---|
| Core-SW ↔ Server-SW | Cobre UTP Cat6a | Ambos equipos en la misma sala de datos, distancia corta; el EtherChannel ya provee el ancho de banda necesario sin requerir fibra. |
| Core-SW ↔ IyD-SW1 | Fibra óptica | Enlace entre edificios distintos (>100 m típico) + mayor demanda de ancho de banda exigida para esta zona. |
| Core-SW ↔ Corp-Dist-SW | Fibra óptica | Distancia entre edificios del campus. |
| Core-SW ↔ Prod-SW | Fibra óptica | Cercanía a maquinaria industrial pesada; la fibra es inmune a la interferencia electromagnética (EMI) que el cobre sí sufre en ese entorno. |
| Enlaces intra-edificio (triángulo I+D, triángulo Corporativo, accesos a PCs/servidores/Hub) | Cobre UTP Cat6 | Distancias cortas dentro del mismo edificio; costo-efectivo y suficiente para el volumen de tráfico local. |

---

## Seguridad básica implementada

- **Banner MOTD** en todos los switches: `Acceso Restringido - TechPark_202307771`
- **VLAN nativa cambiada de la 1 (default) a la 91** en todos los enlaces troncales del campus, para prevenir ataques de VLAN hopping.
- **Red inalámbrica de invitados** (VLAN 51) protegida con WPA-PSK, SSID `TechPark-Visitantes`, completamente aislada del resto del tráfico corporativo .

---

##  Configuración del Access Point

| Parámetro | Valor |
|---|---|
| SSID | TechPark-Visitantes |
| Seguridad | WPA-PSK (AES) |
| VLAN asociada | 51 (VISITANTES) |
| Ubicación física | Ala-A-SW, puerto Fa0/3 |

1. >Pantalla de configuración del Access Point (Config → Port 1).
<div align="center">
  <img src="imagenes/Access-point.png" alt="MDF">
</div>


2. >Laptop de visitante con estado "Adapter is Active" conectada al SSID.
<div align="center">
  <img src="imagenes/lap-visitante.png" alt="MDF">
</div>

3. > Ping exitoso entre las 2 laptops de visitantes (prueba de conectividad intra-VLAN).
<div align="center">
  <img src="imagenes/ping-visitantes.png" alt="MDF">
</div>

4. > Ping fallido desde una laptop de visitante hacia una PC de Gerencia (prueba de aislamiento inter-VLAN).
<div align="center">
  <img src="imagenes/ping-fallido.png" alt="MDF">
</div>

---

##  Evidencia de pruebas (comandos show)

> `show spanning-tree` (primera cap en Core-SW y 2da cap en uno de los switches del triángulo de I+D ).
<div align="center">
  <img src="imagenes/core-sw.png" alt="MDF">
</div>

<div align="center">
  <img src="imagenes/i+d-sw.png" alt="MDF">
</div>


> `show etherchannel summary` en Core-SW, Server-SW e IyD-SW1 (los 3 lados de los EtherChannel).
<div align="center">
  <img src="imagenes/summary-1.png" alt="MDF">
</div>

<div align="center">
  <img src="imagenes/summary-2.png" alt="MDF">
</div>

<div align="center">
  <img src="imagenes/summary-3.png" alt="MDF">
</div>

> `show interfaces trunk` 
<div align="center">
  <img src="imagenes/trunk-1.png" alt="MDF">
</div>

<div align="center">
  <img src="imagenes/trunk-2.png" alt="MDF">
</div>

---

## Comandos utilizados por dispositivo

### Core-SW
```
hostname Core-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode server
vlan 11
 name GERENCIA
vlan 21
 name INVESTIGACION
vlan 31
 name PRODUCCION
vlan 41
 name SERVIDORES
vlan 51
 name VISITANTES
exit
banner motd $Acceso Restringido - TechPark_202307771$
spanning-tree mode rapid-pvst
spanning-tree vlan 11,21,31,41,51 priority 4096
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/3
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface range fastEthernet0/4-5
 switchport mode trunk
 switchport trunk native vlan 91
 channel-group 1 mode desirable
 no shutdown
interface port-channel 1
 switchport mode trunk
 switchport trunk native vlan 91
interface range fastEthernet0/1,fastEthernet0/6
 switchport mode trunk
 switchport trunk native vlan 91
 channel-group 2 mode desirable
 no shutdown
interface port-channel 2
 switchport mode trunk
 switchport trunk native vlan 91
end
copy running-config startup-config
```

### Server-SW
```
hostname Server-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface range fastEthernet0/5-6
 channel-group 1 mode desirable
interface port-channel 1
 switchport mode trunk
 switchport trunk native vlan 91
interface range fastEthernet0/1-4
 switchport mode access
 switchport access vlan 41
 no shutdown
end
copy running-config startup-config
```

### IyD-SW1
```
hostname IyD-SW1
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface range fastEthernet0/3-4
 switchport mode trunk
 switchport trunk native vlan 91
 channel-group 1 mode desirable
 no shutdown
interface port-channel 1
 switchport mode trunk
 switchport trunk native vlan 91
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
end
copy running-config startup-config
```

### IyD-SW2
```
hostname IyD-SW2
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface range fastEthernet0/3-6
 switchport mode access
 switchport access vlan 21
 no shutdown
end
copy running-config startup-config
```

### IyD-SW3
```
hostname IyD-SW3
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface range fastEthernet0/3-6
 switchport mode access
 switchport access vlan 21
 no shutdown
end
copy running-config startup-config
```

### Prod-SW
```
hostname Prod-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/1
 switchport mode access
 switchport access vlan 31
 no shutdown
end
copy running-config startup-config
```

### Corp-Dist-SW
```
hostname Corp-Dist-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/3
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
end
copy running-config startup-config
```

### Ala-A-SW
```
hostname Ala-A-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/3
 switchport mode access
 switchport access vlan 51
 no shutdown
interface range fastEthernet0/4-5
 switchport mode access
 switchport access vlan 11
 no shutdown
end
copy running-config startup-config
```

### Ala-B-SW
```
hostname Ala-B-SW
vtp domain Smart_7
vtp password proyecto12S2026
vtp version 2
vtp mode client
spanning-tree mode rapid-pvst
banner motd $Acceso Restringido - TechPark_202307771$
interface fastEthernet0/1
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface fastEthernet0/2
 switchport mode trunk
 switchport trunk native vlan 91
 no shutdown
interface range fastEthernet0/3-4
 switchport mode access
 switchport access vlan 11
 no shutdown
end
copy running-config startup-config
```

---

## Presupuesto estimado de equipos físicos simulados (Guatemala)

| Equipo | Cantidad | Precio unitario (GTQ) | Subtotal (GTQ) |
|---|---|---|---|
| Switch Cisco Catalyst 2960-24TT-L | 9 | Q5,812.50 | Q52,312.50 |
| Módulo de fibra (SFP/GBIC) | 6 | Q348.75 | Q2,092.50 |
| Cable UTP Cat6 (patch cord, promedio) | 30 | Q62.00 | Q1,860.00 |
| Cable de fibra óptica (patch cord dúplex) | 3 | Q271.25 | Q813.75 |
| **Total estimado** | | | **Q57,078.75** |

*Precios de referencia aproximados*

---

## Conclusiones

El proyecto SmartCity Tech Park resuelve los problemas planteados en el enunciado mediante:
- Segmentación lógica con 5 VLANs administradas centralizadamente por VTP.
- Prevención de bucles y alta disponibilidad mediante Rapid-PVST en los triángulos de I+D y Edificio Corporativo.
- Agregación de enlaces (EtherChannel PAgP) para dar mayor ancho de banda y redundancia a los segmentos de mayor tráfico crítico (I+D y Servidores).
- Aislamiento completo del tráfico de invitados mediante una VLAN dedicada y una red inalámbrica independiente.
- Documentación explícita del dominio de colisión heredado del segmento Legacy, sin comprometer la operación del resto del campus.