# Código Fuente — Scripts CLI de todos los switches

- **Nombre:** Mayelin Lucía Quevedo Camey
- **Carnet:** 202307771  
- **Curso:** Redes de Computadoras 1 · 2S2026


Scripts completos de los comandos CLI usados en los 10 switches (Cisco 3560 y 2960) en Cisco Packet Tracer.

| Parámetro | Valor |
|---|---|
| VLANs de zona | 11, 21, 31, 41, 51 |
| VLAN nativa / Blackhole | 99 / 999 |
| Dominio VTP / contraseña | `202307771` / `Cayala2026` |
| EtherChannel | PAgP (`desirable`) |
| STP | Rapid PVST+ |

## Resumen de dispositivos

| Switch | Modelo | Rol |
|---|---|---|
| SW-Z2-P | 3560 | Núcleo L3 · Zona 2 (Administración y seguridad) · servidor VTP · enruta entre VLANs |
| SW-Z1-P | 3560 | Principal Zona 1 (Plaza comercial) · servidor VTP · root VLAN 11 |
| SW-Z4-P | 3560 | Principal Zona 4 (Corporativa) · servidor VTP · root VLAN 41 |
| SW-Z3-P | 2960 | Principal Zona 3 (Residencial) · servidor VTP · root VLAN 31 |
| SW-Z5-P | 2960 | Principal Zona 5 (Cines) · servidor VTP · root VLAN 51 |
| SW-Z1-A1 | 2960 | Acceso Zona 1 · cliente VTP |
| SW-Z1-A2 | 2960 | Acceso Zona 1 · cliente VTP |
| SW-Z2-A1 | 2960 | Acceso Zona 2 · cliente VTP |
| SW-Z3-A1 | 2960 | Acceso Zona 3 · cliente VTP |
| SW-Z4-A1 | 2960 | Acceso Zona 4 · cliente VTP |

## SW-Z2-P

*Cisco 3560 — Núcleo L3 · Zona 2 (Administración y seguridad) · servidor VTP · enruta entre VLANs*

```text
enable
configure terminal
hostname SW-Z2-P
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode server
spanning-tree mode rapid-pvst
interface range gi0/1 - 2
 channel-protocol pagp
 channel-group 1 mode desirable
 exit
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/1 - 2
 channel-protocol pagp
 channel-group 2 mode desirable
 exit
interface port-channel 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 31,99
 exit
interface range fa0/4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 51,99
 exit
interface range fa0/5 - 6
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 21,99
 exit
end
write memory


configure terminal
vlan 11
 name Z1_Comercial_Gastro
vlan 21
 name Z2_Admin_Seguridad
vlan 31
 name Z3_Residencial
vlan 41
 name Z4_Corporativa
vlan 51
 name Z5_Cines
vlan 99
 name Nativa_Admin
vlan 999
 name Blackhole
exit
end
write memory

configure terminal
interface range fa0/7 - 24
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
spanning-tree vlan 21 priority 4096
spanning-tree vlan 99 priority 4096
interface vlan 99
 ip address 192.168.1.193 255.255.255.224
 no shutdown
 exit
ip routing
interface vlan 11
 ip address 192.168.1.1 255.255.255.192
 no shutdown
 exit
interface vlan 21
 ip address 192.168.1.129 255.255.255.224
 no shutdown
 exit
interface vlan 31
 ip address 192.168.1.161 255.255.255.240
 no shutdown
 exit
interface vlan 41
 ip address 192.168.1.65 255.255.255.192
 no shutdown
 exit
interface vlan 51
 ip address 192.168.1.177 255.255.255.240
 no shutdown
 exit
end
write memory
```

## SW-Z1-P

*Cisco 3560 — Principal Zona 1 (Plaza comercial) · servidor VTP · root VLAN 11*

```text
enable
configure terminal
hostname SW-Z1-P
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode server
spanning-tree mode rapid-pvst
interface range gi0/1 - 2
 channel-protocol pagp
 channel-group 1 mode desirable
 exit
interface port-channel 1
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/4 - 5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,99
 exit
end
write memory

configure terminal
interface range fa0/6 - 24
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
spanning-tree vlan 11 priority 4096
interface vlan 99
 ip address 192.168.1.194 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z4-P

*Cisco 3560 — Principal Zona 4 (Corporativa) · servidor VTP · root VLAN 41*

```text
enable
configure terminal
hostname SW-Z4-P
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode server
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 channel-protocol pagp
 channel-group 2 mode desirable
 exit
interface port-channel 2
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/3
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,41,99
 exit
interface range fa0/4 - 5
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 41,99
 exit
end
write memory


configure terminal
interface range fa0/6 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
spanning-tree vlan 41 priority 4096
interface vlan 99
 ip address 192.168.1.195 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z3-P

*Cisco 2960 — Principal Zona 3 (Residencial) · servidor VTP · root VLAN 31*

```text
enable
configure terminal
hostname SW-Z3-P
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode server
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 31,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
spanning-tree vlan 31 priority 4096
interface vlan 99
 ip address 192.168.1.196 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z5-P

*Cisco 2960 — Principal Zona 5 (Cines) · servidor VTP · root VLAN 51*

```text
enable
configure terminal
hostname SW-Z5-P
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode server
spanning-tree mode rapid-pvst
interface range fa0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 51,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 5
 switchport mode access
 switchport access vlan 51
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/2 , fa0/6 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
spanning-tree vlan 51 priority 4096
interface vlan 99
 ip address 192.168.1.197 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z1-A1

*Cisco 2960 — Acceso Zona 1 · cliente VTP*

```text
enable
configure terminal
hostname SW-Z1-A1
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode client
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 4
 switchport mode access
 switchport access vlan 11
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/5 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
interface vlan 99
 ip address 192.168.1.198 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z1-A2

*Cisco 2960 — Acceso Zona 1 · cliente VTP*

```text
enable
configure terminal
hostname SW-Z1-A2
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode client
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 11,99
 exit
end
write memory

configure terminal
interface range fa0/3
 switchport mode access
 switchport access vlan 11
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/4 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
interface vlan 99
 ip address 192.168.1.199 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z2-A1

*Cisco 2960 — Acceso Zona 2 · cliente VTP*

```text
enable
configure terminal
hostname SW-Z2-A1
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode client
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 21,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 5
 switchport mode access
 switchport access vlan 21
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/6 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
interface vlan 99
 ip address 192.168.1.200 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z3-A1

*Cisco 2960 — Acceso Zona 3 · cliente VTP*

```text
enable
configure terminal
hostname SW-Z3-A1
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode client
spanning-tree mode rapid-pvst
interface range fa0/1
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 31,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 5
 switchport mode access
 switchport access vlan 31
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/2 , fa0/6 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
interface vlan 99
 ip address 192.168.1.201 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```

## SW-Z4-A1

*Cisco 2960 — Acceso Zona 4 · cliente VTP*

```text
enable
configure terminal
hostname SW-Z4-A1
no ip domain-lookup
vtp version 2
vtp domain 202307771
vtp password Cayala2026
vtp mode client
spanning-tree mode rapid-pvst
interface range fa0/1 - 2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 41,99
 exit
end
write memory

configure terminal
interface range fa0/3 - 5
 switchport mode access
 switchport access vlan 41
 spanning-tree portfast
 no shutdown
 exit
interface range fa0/6 - 24 , gi0/1 - 2
 switchport mode access
 switchport access vlan 999
 shutdown
 exit
interface vlan 99
 ip address 192.168.1.202 255.255.255.224
 no shutdown
 exit
ip default-gateway 192.168.1.193
end
write memory
```