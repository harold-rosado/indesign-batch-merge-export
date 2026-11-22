# Batch Merge Export (InDesign)

Script para Adobe InDesign que genera **un archivo por cada fila de un CSV**
(certificados, credenciales, tarjetas…) y le pone a cada archivo un nombre
tomado de los datos, por ejemplo `María José Fernández.pdf`.

- **Autor:** Harold
- **Versión:** 1.0.0
- **Requiere:** Adobe InDesign CS6 o posterior.
- **Licencia:** Propietaria — software de Harold.

---

## ¿Para qué sirve?

La Combinación de datos de InDesign genera un PDF con todas las páginas, o
páginas sueltas llamadas `plantilla1.jpg`, `plantilla2.jpg`… Lo que **no** hace
es dar a cada archivo el nombre de su dueño ni avisar de los problemas por
persona. Este script entrega **los archivos listos para enviar, cada uno con
el nombre de sus datos**, y te avisa de lo que está mal **antes** de generar
el lote. Con 100 certificados, son 100 archivos que no hay que renombrar ni
revisar uno por uno.

---

## Cómo funciona

```
 plantilla.indd  +  datos.csv
        │               │
        └──── Script ───┘
                │
   1. Revisa la plantilla (fuentes, vínculos, texto desbordado)
   2. Lee el CSV (detecta  ,  ;  o tabulación y la codificación)
   3. Combina TODOS los registros de una vez (Data Merge de InDesign)
   4. Exporta las páginas de cada registro con su propio nombre
   5. Cierra el documento combinado sin guardarlo
                │
                ▼
   salida_2026-11-14_1725/
     ├── María José Fernández.pdf
     ├── Juan Carlos Pérez.pdf
     └── Juan Carlos Pérez (2).pdf   ← los nombres repetidos se numeran
```

**Tu plantilla y tu CSV nunca se modifican.**

---

## Instalación (Windows)

1. Abre InDesign → **Ventana → Utilidades → Scripts**.
2. En el panel Scripts, clic derecho en la carpeta **Usuario** → **Mostrar en el Explorador**.
3. Copia el archivo del script (`BatchMergeExport.jsxbin`) dentro de esa carpeta.
   En InDesign 2026 en español suele ser:
   `%APPDATA%\Adobe\InDesign\Version 26.0\es_ES\Scripts\Scripts Panel`
4. Vuelve a InDesign: el script aparece en el panel, dentro de **Usuario**.

Para actualizarlo, reemplaza el archivo por la versión nueva. No hace falta
reiniciar InDesign.

---

## Uso paso a paso

1. Abre tu plantilla `.indd`.
2. **Ventana → Utilidades → Combinación de datos** → menú del panel →
   **Seleccionar origen de datos…** → elige tu CSV.
3. Arrastra los campos (por ejemplo `NOMBRE`) a los marcos de texto.
   **No** hagas "Crear documento combinado": de eso se encarga el script.
4. Doble clic en el script en el panel Scripts.
5. En la ventana, de arriba abajo:
   - **Registros:** **Todos**, o un **Rango** escrito como en InDesign:
     `2, 5, 8-10`. Sirve para volver a exportar solo los que corregiste.
   - **Formato** (uno a la vez; para otro, vuelve a ejecutar el script):
     - **PDF** con el **preset** que elijas.
     - **JPG** / **PNG** con la **resolución** (72, 150 o 300 ppp; 150 va bien
       para pantalla y WhatsApp). Si la plantilla tiene varias páginas, sale
       `Nombre_1.jpg`, `Nombre_2.jpg`. PNG necesita InDesign CC 2015 o posterior.
     - **InDesign (.indd):** un documento por registro, para retocar a mano.
     - **Además, un PDF con todos los registros** (opcional, con cualquier
       formato): un solo PDF para imprenta.
   - **Nombre del archivo:** campos entre llaves y, si quieres, texto fijo:
     `{NOMBRE}`, `{NOMBRE} - {CURSO}`, `Certificado {CI}`. `{#}` inserta el
     número de registro. Sin campos, los archivos se numeran solos.
   - **Destino:** dentro se crea una subcarpeta nueva `salida_fecha_hora`, así
     que nunca se sobrescribe nada.
6. Pulsa **Exportar**. Si algún registro tiene problemas (texto desbordado,
   imagen que falta, fila o nombre repetido), el script lo avisa **antes** de
   crear archivos, con el número de registro. Puedes **Volver**, **Cancelar**
   o **Exportar igualmente**.
7. Al terminar, el botón **Abrir carpeta** muestra los archivos.

---

## Preparar el CSV

La primera fila lleva los nombres de las columnas (`NOMBRE`, `CURSO`, `CI`…).
Las columnas de imagen empiezan con `@` (por ejemplo `@FOTO`).

Cómo guardar desde **Excel** (Archivo → Guardar como):

| Opción | ¿Sirve? |
|---|---|
| **CSV (delimitado por comas)** | ✅ **Recomendado** |
| **Texto Unicode (\*.txt)** | ✅ |
| **Texto (delimitado por tabulaciones)** | ✅ |
| CSV UTF-8 (delimitado por comas) | ❌ InDesign lo rechaza |

**Google Sheets** (Archivo → Descargar → CSV o TSV) siempre descarga en UTF-8,
que InDesign en Windows lee mal ("MarÃ­a"). El script lo detecta y lo corrige
con un clic: crea una copia que InDesign lee bien, la conecta a la plantilla y
sigue. Tu archivo original no se toca.

En el nombre del archivo se cambian por `_` los caracteres que Windows no
permite (`\ / : * ? " < > |`). Las tildes, la ñ, los espacios y los apóstrofes
se conservan.

---

## Compatibilidad

- **InDesign CS6 (2012) o posterior.** En versiones anteriores, el script avisa
  y no se ejecuta.
- **PNG** solo existe desde InDesign CC 2015.
- El mismo archivo sirve para **Windows y Mac**

---
