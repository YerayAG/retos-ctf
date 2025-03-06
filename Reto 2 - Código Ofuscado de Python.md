### **Reto 2: Código Ofuscado (Python/JavaScript)**
**Tema principal**: Ingeniería inversa + análisis estático/dinámico + manipulación de argumentos.

---

#### **Implementación Paso a Paso**:

##### **Versión Python**:
1. **Código Ofuscado**:
  ```python
  import sys, base64

  def xor_encrypt_decrypt(data, key):
      return ''.join(chr(ord(c) ^ ord(k)) for c, k in zip(data, key * (len(data) // len(key) + 1)))

  def m1():
      return "JQAAER4aABEMLQAHPhYCACkDMxICPBYAKxERGQQUBCI="  # Flag cifrada con XOR y luego Base64

  def m2():
      return "Flag{Esta_no_es_la_flag}"

  if __name__ == "__main__":
      key = "clave_secreta"
      if len(sys.argv) > 1 and sys.argv[1] == "--descifra":
          decoded = base64.b64decode(m1()).decode('utf-8')
          flag = xor_encrypt_decrypt(decoded, key)
          print(f"¡Flag encontrada!: {flag}")
      else:
          print("Error: Modo no activado. Usa '--descifra' si eres valiente.")
  ```
   - **Flag codificada**: `JQAAER4aABEMLQAHPhYCACkDMxICPBYAKxERGQQUBCI=` → Decodifica a `Flag{Esto_es_una_flag_de_prueba}` (en Base64).
   - **Trucos**:
     - Usar nombres de funciones engañosos (`m1`, `m2`).
     - Incluir código muerto (como `m2()`, que nunca se usa).
     - Pista en el mensaje de error ("usa --descifra").

---

#### **Cómo lo resolverían los jugadores**:
1. **Análisis dinámico**:
   - **Python**:
     ```bash
     python3 reto.py --descifra  # Resultado directo.
     ```

2. **Decodificación manual**:
   - Si está cifrada con XOR (ejemplo Python), usar **CyberChef** o un script inverso.

---

#### **Variantes para aumentar la dificultad**:
- **Ofuscación con múltiples pasos**:
  - En Python: Codificar la flag en Base64 → rot13 → hexadecimal.
  - En JS: Usar `eval` con una cadena construida dinámicamente:
    ```javascript
    const c = [102, 108, 97, 103]; // "flag" en ASCII
    console.log(String.fromCharCode(...c));
    ```
- **Requiere un cálculo matemático**:
  - Pedir un número específico como argumento (ej: `1337 * 7`) cuya solución sea la clave.
- **Condicionales encriptados**:
  - Usar una comparación cifrada (ej: `hashlib.sha256(argv[1].encode()).hexdigest() == "a4d8..."`).