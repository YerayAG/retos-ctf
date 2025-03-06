### **Reto 3: Flag en Tráfico de Red**
**Tema principal**: Captura y análisis de tráfico de red + ofuscación de endpoints.

---

#### **Implementación Paso a Paso**:

##### **1. Crear un Servidor Web con la Flag**:
- Usa **Python** y Flask para crear un servidor que sirva la flag bajo ciertas condiciones.
- **Código del servidor** (`server.py`):
  ```python
  from flask import Flask, request, Response

  app = Flask(__name__)
  FLAG = "FLAG{tr4ff1c_4n4ly515_p0w3r}"

  @app.route('/flag')
  def get_flag():
      # Solo envía la flag si el User-Agent es "CTF_Agent" y hay un parámetro 'key' correcto.
      user_agent = request.headers.get('User-Agent')
      key = request.args.get('key', '')
      
      if user_agent == "CTF_Agent" and key == "0xDEADBEEF":
          return FLAG
      else:
          return "Acceso denegado. 🚫", 403

  if __name__ == '__main__':
      app.run(port=1337, debug=False)
  ```

##### **2. Crear un Cliente/Script que Envíe la Petición**:
- El script debe enviar una solicitud HTTP **encubierta** al servidor.
- **Código del cliente** (`client.py`):
  ```python
  import requests
  import time

  def send_request():
      try:
          # Headers personalizados y parámetros secretos
          headers = {'User-Agent': 'CTF_Agent'}
          response = requests.get(
              'http://localhost:1337/flag',
              params={'key': '0xDEADBEEF'},
              headers=headers,
              timeout=2
          )
          print("Respuesta del servidor:", response.text)
      except:
          pass  # Silencia errores para no dar pistas

  if __name__ == '__main__':
      send_request()
      time.sleep(1)  # Da tiempo a capturar el tráfico
  ```

##### **3. Preparar Archivos para el Jugador**:
- Entrega **solo el cliente** (`client.py`) al jugador.
- **Opcional**: Proporciona un archivo `captura.pcap` pre-grabado con el tráfico (para quienes no quieran ejecutar código).

---

#### **Cómo lo resolverían los jugadores**:

##### **Opción 1: Capturar Tráfico en Vivo**:
1. **Ejecutar el cliente y capturar tráfico**:
   ```bash
   # En una terminal, inicia Wireshark/tcpdump:
   tcpdump -i lo -w captura.pcap port 1337

   # En otra, ejecuta el cliente:
   python3 client.py
   ```

2. **Analizar la captura**:
   - Usando **Wireshark**:
     - Filtrar por `http && tcp.port == 1337`.
     - Seguir el flujo HTTP (TCP Stream) para ver la solicitud/respuesta.
     - Buscar la flag en la **respuesta del servidor** (si el jugador recreó el servidor) o en los **parámetros de la solicitud** (ej. `key=0xDEADBEEF`).

   - Usando `strings` o `ngrep`:
     ```bash
     strings captura.pcap | grep 'FLAG{'
     ngrep -d lo -q 'FLAG{' port 1337
     ```

##### **Opción 2: Ingeniería Inversa del Cliente**:
- Si el jugador no quiere capturar tráfico, puede analizar el código del cliente:
  ```python
  # Verá el User-Agent 'CTF_Agent' y el parámetro 'key=0xDEADBEEF'
  # Podría intentar acceder directamente a http://localhost:1337/flag?key=0xDEADBEEF
  # ...¡Pero necesitará el User-Agent correcto!
  curl http://localhost:1337/flag?key=0xDEADBEEF -H "User-Agent: CTF_Agent"
  ```

---

#### **Variantes para Aumentar la Dificultad**:
- **Flag en Headers Custom**:
  - Enviar la flag en una cabecera HTTP oscura, ej. `X-Flag: RkxBR3t...` (Base64).
- **Fragmentación de Paquetes**:
  - Dividir la flag en múltiples paquetes ICMP/UDP (requiere reconstrucción).
- **Cifrado AES en el Tráfico**:
  - Usar HTTPS con un certificado autofirmado incluido en los archivos del reto.
- **Exfiltración via DNS**:
  - Codificar la flag en subdominios (ej. `FLAG123.evil.com` → `466C61677B...`).