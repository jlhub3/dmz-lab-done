
# Informe de configuración de DMZ con Cisco Packet Tracer


### 1. Objetivo del laboratorio

> Explica brevemente qué se buscaba lograr con este laboratorio.

**Ejemplo:**  
Configurar una DMZ segura usando un router Cisco ISR, aplicando NAT y ACLs para controlar el tráfico entre LAN, DMZ y red externa.

---

### 2. Topología implementada

> Describe la red. Puedes incluir una imagen si el software lo permite (captura de Packet Tracer).

- Cantidad de redes: __________
- Dispositivos usados: __________
- Breve descripción de la función de cada zona (LAN, DMZ, Externa).



### 3. Plan de direccionamiento IP

Completa la tabla con las IPs asignadas (puedes copiarla del enunciado si no cambió).

| Dispositivo             | IP              | Máscara           | Gateway           |
|-------------------------|------------------|-------------------|-------------------|
| PC_Internal             |                  |                   |                   |
| Server_DMZ              |                  |                   |                   |
| PC_External             |                  |                   |                   |
| Router_FW Gi0/0 (LAN)   |                  |                   |                   |
| Router_FW Gi0/1 (DMZ)   |                  |                   |                   |
| Router_FW Gi0/2 (Ext)   |                  |                   |                   |


### 4. Configuración aplicada (resumen)

> Resume los comandos o pasos más relevantes que ejecutaste. Usa texto + fragmentos de código cuando sea necesario.

- Interfaces configuradas con `ip address`
- NAT:
```bash
ip nat inside source static 192.168.2.10 192.168.3.1
```
- ACLs:
```bash
access-list 101 permit tcp any host 192.168.3.1 eq 80
access-list 100 deny ip 192.168.2.0 0.0.0.255 192.168.1.0 0.0.0.255
```



### 5. Verificaciones realizadas

> Describe las pruebas y su resultado. Incluye capturas o salidas de comandos si se puede.

- `ping` desde PC_Internal al router: ✅
- Acceso web desde PC_External: ✅
- Bloqueo de acceso desde DMZ a LAN: ✅


### 6. Conclusiones y recomendaciones

> ¿Qué aprendiste con este ejercicio? ¿Qué mejorarías?

**Ejemplo:**
Aprendí a aplicar NAT y ACLs en un entorno simulado. Recomiendo verificar conectividad básica antes de aplicar reglas de firewall, ya que un error en la IP puede bloquear todo.


### 7. Capturas de evidencia

> Adjunta aquí (o en un PDF anexo) las capturas solicitadas: pings, navegador, comandos `show`, etc.
