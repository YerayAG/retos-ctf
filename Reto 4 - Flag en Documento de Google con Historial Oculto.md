### **Reto 4: Flag en Documento de Google con Historial Oculto**  
**Tema**: OSINT + Manipulación de documentos colaborativos.  

---

#### **Implementación Paso a Paso**:
1. **Crear el Documento de Google**:  
   - Escribe un documento trivial, como una receta de cocina o una lista de compras.  
   - **Texto visible**: "Este documento no contiene flags. Sigan buscando."  
   - **Texto oculto**:  
     - Escribe la flag en **blanco sobre blanco** (color de fuente #FFFFFF).  
     - Añade un comentario antiguo (en el historial) con la flag: Ve a *Archivo → Historial de versiones → Ver historial de versiones*, restaura una versión antigua y escribe un comentario como *"Revisión por seguridad: BORRAR FLAG{...}"*.  

2. **Configurar Permisos**:  
   - Comparte el documento como **"Cualquiera con el enlace puede comentar"**.  
   - **Truco**: Si el jugador solo tiene acceso de "lector", no podrá ver comentarios antiguos. Si tiene permiso de "comentarista", sí.  

3. **Pistas Engañosas**:  
   - En el documento actual, añade un comentario visible: *"¿Seguro que no hay nada oculto? ¡Revisa el pasado!"*.  
   - Usa un hipervínculo en una palabra que lleve a una imagen de un reloj (símbolo de "historial").  

---

#### **Cómo lo Resolverían los Jugadores**:
1. **Acceder al Historial de Versiones**:  
   - Si el jugador tiene permisos de "comentarista" o "editor", irá a *Archivo → Historial de versiones* y revisará versiones antiguas.  
   - **Solución rápida**: Buscará comentarios eliminados o texto en versiones anteriores.  

2. **Inspeccionar el Código Fuente (HTML)**:  
   - Si el jugador exporta el documento como HTML (usando *Archivo → Descargar → Página web*), buscará texto oculto:  
     ```html
     <span style="color:#ffffff">FLAG{...}</span>
     ```
   - **Herramientas**: `wget` + `grep "FLAG{"`, o simplemente "Ver código fuente" en el navegador.  

3. **Google Takeout (Variante Avanzada)**:  
   - Si el reto simula un usuario real, el jugador podría pedir un archivo de Google Takeout del documento y buscar en los archivos JSON de historial.  

---

#### **Herramientas Clave**:  
- Para creadores: Google Docs + historial de versiones.  
- Para jugadores: Navegador web, `grep`, `curl`, o scripts para extraer texto oculto.  

---

#### **Variantes para Mayor Dificultad**:  
- **Flag en Tabla Borrada**: En una versión antigua, insertar una tabla con la flag y luego borrarla.  
- **Hipervínculo Engañoso**: Un enlace a otro documento que requiera resolver un acertijo.  
- **Permisos Jerárquicos**: La flag está en un comentario de un usuario específico (ej: "admin@ctf.com"), obligando a impersonar roles.  

---