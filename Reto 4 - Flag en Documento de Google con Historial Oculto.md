### **Reto 4: Flag en Documento de Google con Historial Oculto**  
**Tema**: OSINT + Manipulación de documentos colaborativos.  

---

#### **Implementación Paso a Paso**:
1. **Crear el Documento de Google**:  
   - Escribe un documento trivial, como una receta de cocina.  
   - **Texto oculto**:  
     - Escribe la flag en **blanco sobre blanco** (color de fuente #FFFFFF), y luego posteriormente borrarla para que solo quede en el historial.  

2. **Configurar Permisos**:  
   - Comparte el documento como **"Editor"**.  

---

#### **Cómo lo Resolverían los Jugadores**:
1. **Acceder al Historial de Versiones**:  
   - Si el jugador tiene permisos de "Editor", tendra que ir a a ver el historial y vera la flag oculta.  

2. **Inspeccionar el Código Fuente (HTML)**:  
   - Si el jugador exporta el documento como HTML (usando *Archivo → Descargar → Página web*), buscará texto oculto:  
     ```html
     <span style="color:#ffffff">FLAG{...}</span>
     ```  
   - **Herramientas**: `wget` + `grep "FLAG{"`, o simplemente "Ver código fuente" en el navegador.  

---

#### **Herramientas Clave**:  
- Para creadores: Google Docs + historial de versiones.  
- Para jugadores: Navegador web, `grep`, `curl`, o scripts para extraer texto oculto.
