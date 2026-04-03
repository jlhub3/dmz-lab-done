
# Informe de configuración de DMZ con Cisco Packet Tracer

## 1. Objetivo del laboratorio

Implementar y asegurar una arquitectura de red mediante la creación de una Zona Desmilitarizada (DMZ). El objetivo principal es permitir que servicios públicos (como un servidor Web) sean accesibles desde una red externa (Internet) sin comprometer la seguridad de la red interna (LAN), utilizando para ello Listas de Control de Acceso (ACL) y Traducción de Direcciones de Red (NAT).
---

### 2. Topología implementada

- Cantidad de redes: ____3_(LAN, DMZ, Externa/Internet).
- Dispositivos usados: _1 Router (Cisco ISR 2911 o similar), 2 Switches, 1 Servidor (DMZ), 2 PCs (Internal y External).
- Descripción de zonas:

LAN (Red Interna): Zona privada donde residen los usuarios locales; tiene acceso a la DMZ e Internet, pero no debe ser accesible desde fuera.

DMZ (Zona Desmilitarizada): Contiene el servidor web; es accesible desde la LAN e Internet bajo reglas estrictas.

Externa (Internet): Representa a los usuarios fuera de la organización que solo deben ver servicios públicos.



### 3. Plan de direccionamiento IP

Dispositivo,IP,Máscara,Gateway
PC_Internal,192.168.1.10,255.255.255.0,192.168.1.1
Server_DMZ,192.168.2.10,255.255.255.0,192.168.2.1
PC_External,200.0.0.10,255.255.255.0,200.0.0.1
Router (LAN) Gi0/0,192.168.1.1,255.255.255.0,N/A
Router (DMZ) Gi0/1,192.168.2.1,255.255.255.0,N/A
Router (Ext) Gi0/2,200.0.0.1,255.255.255.0,N/A


### 4. Configuración aplicada (resumen)
Configuración aplicada (resumen)
Configuración de Interfaces:
Se asignaron direcciones IP y se definieron los roles de NAT (inside para LAN/DMZ y outside para Internet).

Bash
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside

interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
 ip nat inside

interface GigabitEthernet0/2
 ip address 200.0.0.1 255.255.255.0
 ip nat outside
NAT Estático:
Se mapeó la IP privada del servidor a una IP pública (o se permitió el tráfico directo si no usaste NAT dinámico).

Bash
ip nat inside source static 192.168.2.10 200.0.0.100
ACLs de Seguridad:
Se aplicaron reglas para permitir tráfico web (puerto 80) y denegar el acceso no deseado de la DMZ a la LAN.

Bash
access-list 101 permit tcp any host 192.168.2.10 eq 80
access-list 101 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255



### 5. Verificaciones realizadas

Ping desde PC_Internal al router: ✅ Exitoso. ![Ping LAN](../evidencias/ping_lan.png)

Acceso web desde PC_External: ✅ El PC externo visualiza la página del servidor DMZ vía HTTP. ![Acceso Web](../evidencias/web_access.png)

Bloqueo de acceso desde DMZ a LAN: ✅ El ping desde el servidor a la PC interna falla como se esperaba por la ACL. ![Bloqueo DMZ](../evidencias/bloqueo.png)

### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?
> En este laboratorio aprendí la importancia de la segmentación física y lógica de la red. La configuración de ACLs es crítica; un pequeño error en el orden de las líneas (secuencia) puede dejar a la red vulnerable o sin conexión.
Recomendación: Siempre aplicar el principio de "mínimo privilegio", denegando todo el tráfico por defecto y permitiendo solo lo estrictamente necesario (HTTP, DNS).



### 7. Capturas de evidencia

> Adjunta aquí (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
