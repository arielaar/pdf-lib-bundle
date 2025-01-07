# pdf-lib-bundle
Build javascript of pdf-lib

Para construir `pdf-lib.bundle.js` y `pdf-lib.min.js` utilizando Node.js y asegurarte de que `PDFLib` esté definido en el ámbito global, puedes seguir estos pasos. Utilizaremos herramientas como `browserify` y `uglify-js` para crear los archivos bundle y minificados respectivamente.

### Paso 1: Instalar Node.js y npm

Si aún no tienes Node.js y npm instalados, puedes descargarlos e instalarlos desde [nodejs.org](https://nodejs.org/).

### Paso 2: Crear un Proyecto Node.js

1. **Crear un Directorio para el Proyecto**:
   ```sh
   mkdir pdf-lib-bundle
   cd pdf-lib-bundle
   ```

2. **Inicializar un Proyecto Node.js**:
   ```sh
   npm init -y
   ```

### Paso 3: Instalar Dependencias

1. **Instalar `pdf-lib`**:
   ```sh
   npm install pdf-lib
   ```

2. **Instalar `browserify` y `uglify-js`**:
   ```sh
   npm install -g browserify uglify-js
   ```

### Paso 4: Crear un Archivo de Entrada

Crea un archivo `index.js` en el directorio del proyecto con el siguiente contenido:

```javascript
const { PDFDocument, rgb, degrees, StandardFonts } = require('pdf-lib');
window.PDFLib = { PDFDocument, rgb, degrees, StandardFonts };
```

### Paso 5: Crear el Archivo Bundle

1. **Usar `browserify` para Crear `pdf-lib.bundle.js`**:
   ```sh
   browserify index.js -o pdf-lib.bundle.js
   ```

### Paso 6: Minificar el Archivo Bundle

1. **Usar `uglify-js` para Crear `pdf-lib.min.js`**:
   ```sh
   uglifyjs pdf-lib.bundle.js -o pdf-lib.min.js
   ```

### Paso 7: Verificar los Archivos Generados

Ahora deberías tener dos archivos en tu directorio del proyecto:
- `pdf-lib.bundle.js`
- `pdf-lib.min.js`

### Paso 8: Usar los Archivos en tu Página HTML

Aquí tienes un ejemplo de cómo usar `pdf-lib.min.js` en tu página HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PDF-Lib Example</title>
    <script src="pdf-lib.min.js"></script>
</head>
<body>
    <h1>PDF-Lib Example</h1>
    <button id="createPdfButton">Create PDF</button>

    <script>
        document.getElementById('createPdfButton').addEventListener('click', async () => {
            const { PDFDocument, rgb, degrees, StandardFonts } = PDFLib;

            // Crear un nuevo documento PDF
            const pdfDoc = await PDFDocument.create();
            const page = pdfDoc.addPage();

            // Dibujar texto en la página con rotación
            page.drawText('Hola, mundo!', {
                x: 50,
                y: 750,
                size: 24,
                font: await pdfDoc.embedFont(StandardFonts.Helvetica),
                color: rgb(0, 0.53, 0.71),
                rotate: degrees(45), // Rotar el texto 45 grados
            });

            // Convertir el PDF a un array de bytes
            const pdfBytes = await pdfDoc.save();

            // Crear un blob y un enlace de descarga
            const blob = new Blob([pdfBytes], { type: 'application/pdf' });
            const link = document.createElement('a');
            link.href = URL.createObjectURL(blob);
            link.download = 'example.pdf';
            link.click();
        });
    </script>
</body>
</html>
```

### Explicación del Código

1. **Incluir `pdf-lib.min.js`**:
   ```html
   <script src="pdf-lib.min.js"></script>
   ```
   Esto incluye la biblioteca `pdf-lib` en tu página HTML.

2. **Botón para Crear PDF**:
   ```html
   <button id="createPdfButton">Create PDF</button>
   ```
   Este botón activará la función para crear el PDF cuando se haga clic en él.

3. **Script para Crear el PDF**:
   ```javascript
   document.getElementById('createPdfButton').addEventListener('click', async () => {
       const { PDFDocument, rgb, degrees, StandardFonts } = PDFLib;

       // Crear un nuevo documento PDF
       const pdfDoc = await PDFDocument.create();
       const page = pdfDoc.addPage();

       // Dibujar texto en la página con rotación
       page.drawText('Hola, mundo!', {
           x: 50,
           y: 750,
           size: 24,
           font: await pdfDoc.embedFont(StandardFonts.Helvetica),
           color: rgb(0, 0.53, 0.71),
           rotate: degrees(45), // Rotar el texto 45 grados
       });

       // Convertir el PDF a un array de bytes
       const pdfBytes = await pdfDoc.save();

       // Crear un blob y un enlace de descarga
       const blob = new Blob([pdfBytes], { type: 'application/pdf' });
       const link = document.createElement('a');
       link.href = URL.createObjectURL(blob);
       link.download = 'example.pdf';
       link.click();
   });
   ```
   - **Crear el Documento PDF**: Se crea un nuevo documento PDF y se añade una página.
   - **Dibujar Texto con Rotación**: Se dibuja el texto "Hola, mundo!" en la página usando la fuente Helvetica y se rota 45 grados.
   - **Guardar el PDF**: El PDF se guarda como un array de bytes.
   - **Crear un Blob y un Enlace de Descarga**: Se crea un blob a partir de los bytes del PDF y se genera un enlace de descarga que se activa automáticamente.

### Notas Adicionales

- **Compatibilidad del Navegador**: Asegúrate de que tu navegador soporte las funcionalidades modernas de JavaScript, como `async/await` y `Blob`.
- **Seguridad**: Ten en cuenta que ejecutar código en el lado del cliente puede tener implicaciones de seguridad. Asegúrate de validar y sanitizar cualquier entrada del usuario si planeas aceptar datos dinámicos.

Este ejemplo debería ayudarte a comenzar con `pdf-lib` en el lado del cliente utilizando HTML5 y `pdf-lib.min.js`, y muestra cómo usar la propiedad `degrees` para rotar texto en un documento PDF.