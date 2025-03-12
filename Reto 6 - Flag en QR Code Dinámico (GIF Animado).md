### **Reto 6: Flag en QR Code Dinámico (GIF Animado)**
**Tema**: Esteganografía + Manipulación de medios + Scripting.

---

#### **Implementación Paso a Paso**:

##### **1. Generar el QR Code Original**:
- **Texto/Flag**: `FLAG{qr_d1n4m1c0_c0n_fr4gm3nt0s}`.
- **Herramienta**: Usa Python con la librería `qrcode`:
  ```python
  import qrcode

  qr = qrcode.QRCode(
      version=5,  # Tamaño del QR (1-40)
      box_size=10,
      border=2
  )
  qr.add_data("FLAG{qr_d1n4m1c0_c0n_fr4gm3nt0s}")
  img = qr.make_image(fill_color="black", back_color="white")
  img.save("flag_qr.png")
  ```

##### **2. Dividir el QR en Fragmentos**:
- **Estrategia**: Cortar el QR en **4 partes verticales** (columnas) usando Python (PIL/Pillow):
  ```python
  from PIL import Image

  img = Image.open("flag_qr.png")
  width, height = img.size
  fragment_width = width // 4  # Divide en 4 partes iguales

  fragments = []
  for i in range(4):
      left = i * fragment_width
      right = (i + 1) * fragment_width
      fragment = img.crop((left, 0, right, height))
      fragments.append(fragment)
      fragment.save(f"fragment_{i}.png")
  ```

##### **3. Crear el GIF Animado**:
- **Herramienta**: Usa `ffmpeg` para animar los fragmentos en bucle:
  ```bash
  ffmpeg -framerate 1 -i fragment_%d.png -loop 0 -r 2 animated_qr.gif
  ```
  - **Explicación**:
    - `-framerate 1`: Cada imagen se muestra 1 segundo.
    - `-loop 0`: Bucle infinito.
    - `-r 2`: 2 fps para fluidez.

---

#### **Cómo lo Resolverían los Jugadores**:

##### **1. Analizar el GIF**:
- **Extraer fotogramas** con `ffmpeg`:
  ```bash
  ffmpeg -i animated_qr.gif -vsync 0 frame_%d.png
  ```
- **Verificar fragmentos**: Cada fotograma es una columna del QR original.

##### **2. Reconstruir el QR**:
- **Opción 1 (Manual)**:
  - Usar un editor de imágenes (GIMP/Photoshop) para unir los 4 fragmentos en orden.

- **Opción 2 (Script Python)**:
  ```python
  from PIL import Image

  fragments = [Image.open(f"frame_{i}.png") for i in range(1, 5)]
  width = sum(frag.width for frag in fragments)
  height = fragments[0].height

  combined = Image.new("RGB", (width, height))
  x_offset = 0
  for frag in fragments:
      combined.paste(frag, (x_offset, 0))
      x_offset += frag.width

  combined.save("reconstructed_qr.png")
  ```

##### **3. Leer el QR**:
- **Herramientas**:
  - Usar lectores estándar como `zbarimg`:
    ```bash
    zbarimg reconstructed_qr.png --raw
    ```
