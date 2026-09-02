# 1234
### Resumen del nuevo flujo de trabajo

1. **La PC Principal (Tú):** Se conecta por el **cable de consola (USB)** para hacer la configuración maestra: le da IP al equipo, habilita el protocolo **SSH** y crea las cuentas de usuario (todas con nivel de privilegio 15).
2. **Las otras PCs (Compañeros):** Se conectan a los puertos normales del equipo con un **cable de red (Ethernet)**, se ponen una IP en su computadora y entran por **SSH desde PuTTY** con su propio usuario y contraseña.

---

### PARTE 1: Configuración maestra (Desde tu laptop por cable de Consola)

Abre PuTTY en modo **Serial** (como lo hiciste antes) y copia/pega los siguientes comandos uno a uno:

1. Entrar al modo privilegiado:
```text
enable
```

2. Entrar a configuración general:
```text
configure terminal
```

3. Asignar un nombre al equipo (obligatorio para activar SSH):
```text
hostname SwitchLab
```

4. Asignar un nombre de dominio (obligatorio para activar SSH):
```text
ip domain-name labsis.com
```

5. Crear una contraseña general de seguridad:
```text
enable secret 12345678
```

---

#### Creación de los usuarios (todos con privilegio 15)
*(Puedes repetir este comando cambiando el nombre y la contraseña para cada compañero)*

* Usuario 1:
```text
username usuario1 privilege 15 secret pass1234
```

* Usuario 2:
```text
username usuario2 privilege 15 secret pass1234
```

* Usuario 3:
```text
username usuario3 privilege 15 secret pass1234
```

---

#### Configurar la IP del Switch

6. Entrar a la interfaz de red:
```text
interface vlan 1
```

7. Asignar la dirección IP:
```text
ip address 192.168.1.1 255.255.255.0
```

8. Encender la interfaz:
```text
no shutdown
```

9. Salir de la interfaz:
```text
exit
```

---

#### Generar las llaves de seguridad y habilitar SSH

10. Generar la llave de cifrado (te preguntará el tamaño en bits):
```text
crypto key generate rsa
```
*(Cuando en la pantalla te pregunte `How many bits [512]:`, escribe `1024` y presiona Enter)*:
```text
1024
```

11. Configurar los canales de acceso remoto:
```text
line vty 0 15
```

12. Indicar que use los usuarios creados anteriormente:
```text
login local
```

13. Permitir el acceso únicamente por SSH:
```text
transport input ssh
```

14. Salir de la configuración de líneas:
```text
exit
```

15. Salir al modo principal:
```text
end
```

16. Guardar todo en la memoria del equipo:
```text
write memory
```

---

### PARTE 2: Lo que deben hacer las otras computadoras (Por cable de red)

Cada compañero debe seguir estos pasos en su laptop:

#### 1. Ponerse una IP en su computadora:
1. Conectar su cable de red (Ethernet) a cualquier puerto libre del Switch.
2. Ir a **Panel de control** > **Redes e Internet** > **Centro de redes y recursos compartidos** > **Cambiar configuración del adaptador**.
3. Clic derecho en **Ethernet** > **Propiedades** > Doble clic en **Protocolo de Internet versión 4 (TCP/IPv4)**.
4. Seleccionar **Usar la siguiente dirección IP** y colocar:
   * **Dirección IP:** Cada uno debe usar un número final distinto (por ejemplo, PC 1 usa `192.168.1.2`, PC 2 usa `192.168.1.3`, PC 3 usa `192.168.1.4`, etc.).
   * **Máscara de subred:** `255.255.255.0`
   * **Puerta de enlace:** `192.168.1.1`
5. Dar clic en **Aceptar**.

#### 2. Entrar por SSH con PuTTY:
1. Abrir **PuTTY**.
2. Marcar la casilla **SSH**.
3. En **Host Name (or IP address)** escribir:
```text
192.168.1.1
```
4. Puerto: `22`.
5. Clic en **Open** (si sale una alerta, darle a **Aceptar** o **Yes**).
6. Escribir su usuario (ejemplo: `usuario1`) y su contraseña (`pass1234`). Al entrar ya tendrán acceso total gracias al privilegio 15.
