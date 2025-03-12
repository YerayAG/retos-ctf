### **Reto 3: Flag en Tráfico de Red**
**Tema principal**: Captura y análisis de tráfico de red.

---

#### **Implementación Paso a Paso**:

##### **1. Generar la Captura de Tráfico con la Flag**:
- Se simula una conexión HTTP donde un usuario ingresa credenciales en un formulario de inicio de sesión.
- Se captura el tráfico de red con **Wireshark** o **tcpdump** mientras se realiza esta acción.
- La flag se encuentra en el campo de **contraseña** enviado por el formulario.

##### **2. Proporcionar la Captura al Jugador**:
- Se entrega un archivo `captura.pcap` con el tráfico generado.

---

#### **Cómo lo resolverían los jugadores**:

##### **Análisis con Wireshark**:
1. **Abrir `captura.pcap` en Wireshark**.
2. **Aplicar un filtro HTTP**:
   ```
   http.request.method == POST
   ```
3. **Seguir el flujo TCP de la petición POST**.
4. **Identificar el campo de contraseña** en los datos de la solicitud.
5. **La contraseña es la flag** con formato `FLAG{...}`.

##### **Análisis con herramientas de línea de comandos**:
- **Extraer texto de la captura** y buscar la flag:
  ```bash
  strings captura.pcap | grep 'FLAG{'
  ```
- **Usar `tshark` para filtrar peticiones HTTP**:
  ```bash
  tshark -r captura.pcap -Y "http.request.method == POST" -T fields -e http.file_data
  ```
