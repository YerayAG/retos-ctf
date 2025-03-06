### **Reto 5: Flag en MakerNotes de EXIF (Metadata de Cámara)**  
**Tema**: Forense digital + Metadata avanzada.  

---

#### **Implementación Paso a Paso**:
1. **Crear una Imagen con MakerNotes Personalizados**:  
   - Usa una imagen JPEG tomada con una cámara real (o genera una con Python).  
   - **Insertar la flag en MakerNotes**:  
     ```bash
     exiftool -MakerNotes="FLAG{Pr0pr13t4ry_M3t4d4t4}" imagen.jpg
     ```
   - **Obfuscación**: Codifica la flag en Base64 antes de insertarla:  
     ```bash
     echo "FLAG{Pr0pr13t4ry_M3t4d4t4}" | base64 | exiftool -MakerNotes="-<@/dev/stdin" imagen.jpg
     ```

2. **Añadir Pistas Falsas en EXIF Estándar**:  
   - Llena otros campos con metadata engañosa:  
     ```bash
     exiftool -Copyright="© Find the flag in the light" -Artist="NoFlagHere" imagen.jpg
     ```

3. **Ocultar en una Partición de Firmware**:  
   - **Variante avanzada**: Incrusta la imagen en un firmware de cámara (ej. `.bin`) usando `dd`:  
     ```bash
     dd if=imagen.jpg of=firmware.bin bs=1 seek=512 conv=notrunc
     ```
   - Los jugadores necesitarán usar `binwalk` para extraerla.  

---

#### **Cómo lo Resolverían los Jugadores**:
1. **Inspeccionar EXIF con Herramientas Específicas**:  
   - Comandos clave:  
     ```bash
     exiftool -U -G1 -MakerNotes imagen.jpg  # -U muestra datos "desconocidos"
     ```
   - **Salida esperada**:  
     ```
     [MakerNotes]               : FLAG{Pr0pr13t4ry_M3t4d4t4}
     ```

2. **Buscar en Hexadecimal**:  
   - Si el campo MakerNotes no es legible directamente, usar `hexedit` o `xxd`:  
     ```bash
     strings -n 5 imagen.jpg | grep "FLAG{"
     ```

3. **Firmware Hacking (Variante)**:  
   - Extraer la imagen del firmware:  
     ```bash
     binwalk -e firmware.bin
     ```
   - Luego analizar la imagen extraída como arriba.  

---

#### **Herramientas Clave**:  
- Para creadores: `exiftool`, `dd`, `binwalk`.  
- Para jugadores: `exiftool -U`, `hexedit`, `strings`.  

---

#### **Variantes para Mayor Dificultad**:  
- **Flag en MakerNotes Codificada**: Usar XOR con clave 0x5A y dejar una pista en otro campo EXIF: *"La clave es 0x5A"*.  
- **Requiere Ordenar Bytes**: Dividir la flag en 2 partes en MakerNotes y otro campo privado.  
- **Falsa Corrupción**: Dañar la imagen para que solo ciertas herramientas (ej: `exiftool`) lean bien los metadatos.  

---

### **Notas Finales**:  
- **Pruebas**: Asegúrate de que:  
  - En el reto 5, los permisos permitan ver comentarios antiguos.  
  - En el reto 12, el MakerNotes no sea eliminado por herramientas como Photoshop al abrir/guardar.  
- **Pistas Opcionales**:  
  - Para el reto 5: *"El pasado siempre guarda secretos"*.  
  - Para el reto 12: *"Los fabricantes a veces esconden cosas..."*.  

¡Estos retos enseñan habilidades de investigación en entornos colaborativos y forense avanzado! ¿Necesitas más detalles o ajustes? 🔍