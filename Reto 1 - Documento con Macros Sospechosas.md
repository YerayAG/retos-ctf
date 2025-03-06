### **Reto 1: Documento con Macros "Sospechosas"**
**Tema principal**: Ingeniería inversa de macros + ofuscación básica.

---

#### **Implementación Paso a Paso**:

1. **Crear el documento (Word/Excel)**:
   - Usa Word o Excel para crear un documento que parezca inofensivo (ej. `CTF_Instructions.docm`).
   - En el cuerpo del documento, escribe algo llamativo como:  
     *"Habilita las macros para ver el contenido secreto... si te atreves."*.

2. **Insertar el código VBA ofuscado**:
   - Abre el editor de VBA (Alt + F11) y crea un módulo nuevo.
   - Escribe código que decodifique la flag de forma no evidente. Ejemplo:
     ```vba
     Sub RevealFlag()
         Dim encodedFlag As String
         Dim realFlag As String
         encodedFlag = "}3lgn1s_3s_3st3_m4l0{FLAG"
         realFlag = StrReverse(encodedFlag)
         MsgBox "¡La flag es: " & realFlag, vbInformation, "¡Éxito!"
     End Sub
     ```
   - **Explicación**: La flag está escrita al revés y se usa `StrReverse()` para revertirla. El resultado sería `FLAG{m4l0_3st3_3s_1ngl3s}`.

3. **Ofuscar más (opcional)**:
   - Usa **codificación Base64** o **hexadecimal** para complicarlo:
     ```vba
     encodedFlag = "ZmxhZ3ttNGwwXzNzdDNfM3NfMW5nbDNzfQ=="
     realFlag = VBA.StrConv(VBA.CreateObject("ADODB.Stream").ReadText, Base64Decode(encodedFlag))
     ```
   - **Nota**: Tendrías que implementar la función `Base64Decode` en VBA o usar un método alternativo.

4. **Vincular el macro a un botón o evento**:
   - Inserta un botón en el documento que ejecute `RevealFlag()` al hacer clic.
   - O usa un evento como `AutoOpen()` para que se ejecute al abrir el documento (pero advierte que requiere habilitar macros).

5. **Añadir pistas engañosas**:
   - En el documento, incluye textos como:  
     *"La verdad está en el código, pero cuidado con los leones de Tsavo"* (referencia a `StrReverse` si usaste inversión).  
     *"¿Qué operación matemática haría 'm4l0' convertirse en 'm4gi4'?"* (si usaste XOR u otro cifrado).

---

#### **Cómo lo resolverían los jugadores**:
1. **Descargar el documento y analizar advertencias de seguridad**:
   - Al abrirlo, verán que las macros están deshabilitadas (pista implícita de que ahí hay algo).

2. **Inspeccionar el código VBA**:
   - Si el jugador habilita macros (en un entorno seguro), verá un mensaje con la flag.  
   - Si no las habilita (recomendado en CTFs), extraerá el código VBA manualmente:
     - Herramientas como `olevba` (de oletools) o `oledump.py` permiten extraer macros sin abrir el archivo:
       ```bash
       olevba CTF_Instructions.docm
       ```
     - Buscarán variables sospechosas como `encodedFlag` o funciones como `StrReverse`.

3. **Decodificar la flag**:
   - Si la flag está invertida (`}3lgn1s...{FLAG`), aplicarán reverso de strings.  
   - Si está en Base64/Hex, usarán herramientas como `cyberchef` o comandos como:
     ```bash
     echo "ZmxhZ3ttNGwwXzNzdDNfM3NfMW5nbDNzfQ==" | base64 -d
     ```

---

#### **Variantes para hacerlo más difícil**:
- **Cifrado XOR**: Usar un XOR simple con una clave (ej. `0x42`) y retar a descubrirla.
  ```vba
  encodedFlag = "GKUWQzM9QTA9QzMxQzA="  # Base64 de la flag XOR-eada
  ```
- **Password en el documento**: Pedir una contraseña que esté escondida en los metadatos (usando `exiftool`).
- **Llamadas a APIs externas**: Que el macro haga una petición HTTP a una URL local con la flag (requiere ingeniería inversa de red).