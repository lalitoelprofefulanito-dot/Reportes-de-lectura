# Control de Reportes de Lectura · Biblioteca Viva

Aplicación de una sola página para llevar el control de entregas y la revisión de los reportes de lectura de 1.° a 6.° de primaria. Funciona en el navegador y guarda los registros, cifrados, en el mismo repositorio de GitHub.

Contenido del repositorio:

- `index.html`: la aplicación. Trae cifradas la lista de 100 alumnos, la ruta lectora de seis libros de cada uno y los apartados de su guía personalizada de la Ronda 1.
- `lib/`: librerías para exportar a PDF (jsPDF y jsPDF-AutoTable) y a Word (docx), con sus licencias MIT. Solo se cargan al exportar.
- `portadas/`: 190 portadas de los libros de las rutas, una por folio (`BV-0001.jpg`…). No contienen datos de alumnos.
- `CRITERIOS.md`: criterios de revisión y su razón.

## Lo más importante: los nombres van cifrados

Las listas de los seis grupos (100 alumnos) vienen **dentro de `index.html`, cifradas** (AES-GCM de 256 bits con clave derivada de una contraseña, PBKDF2 de 600,000 vueltas). En GitHub solo se ve una cadena ilegible. La contraseña se escribe una vez por navegador. Los registros se guardan en ese equipo y, con el guardado automático activo, en `datos/reportes_lectura.cifrado.json` de la rama `datos`, cifrados con la misma contraseña.

La contraseña no está en el repositorio. Compártala solo con quien deba capturar.

Nunca suba al repositorio:

- `roster_privado.json` (la lista sin cifrar)
- los respaldos `INTERNO_*.json` o `INTERNO_*.csv`
- fotografías de cuadernos

El archivo `.gitignore` ya bloquea esos nombres, pero conviene revisar antes de cada subida.

## Publicarlo en GitHub Pages, paso a paso

1. Entre a <https://github.com> con su cuenta.
2. Arriba a la derecha, pulse el botón **+** y elija **New repository**.
3. En **Repository name** escriba, por ejemplo, `reportes-lectura`. Deje marcada la opción **Public** (GitHub Pages gratuito la necesita; recuerde que el código no lleva nombres). Pulse **Create repository**.
4. En la página del repositorio nuevo, busque el enlace **uploading an existing file** (o el botón **Add file → Upload files**).
5. Descomprima el zip en su computadora. Arrastre a esa ventana `index.html`, `README.md`, `CRITERIOS.md`, `.gitignore` **y las carpetas `portadas` y `lib` completas** (en Chrome o Edge puede arrastrar la carpeta tal cual). Si su computadora oculta `.gitignore`, puede omitirlo.
6. Abajo, en **Commit changes**, pulse el botón verde **Commit changes**.
7. Vaya a la pestaña **Settings** del repositorio (arriba, con ícono de engrane).
8. En el menú de la izquierda, pulse **Pages**.
9. En **Build and deployment → Source** elija **Deploy from a branch**. En **Branch** elija `main` y la carpeta `/ (root)`. Pulse **Save**.
10. Espere uno o dos minutos y recargue la página de **Pages**: aparecerá la dirección, parecida a `https://SU-USUARIO.github.io/reportes-lectura/`.

## Primer uso

1. Abra la dirección de su página.
2. Escriba la contraseña de las listas y pulse **Abrir listas**. Se abre directamente **Entregas por grupo**.
3. Calendario de rondas (pestaña **Rondas y lista**). La **Ronda 1** tuvo límite 09/10/2026 y extemporáneo hasta el lunes 12/10/2026, por única ocasión. Desde la **Ronda 2** el libro se presta en lunes, el reporte y el libro se reciben juntos el viernes de la tercera semana (si ese viernes no hay clases, el último día hábil anterior) y todos tienen **una semana más de prórroga**. La siguiente ronda empieza el primer lunes hábil después de la fecha límite:

   | Ronda | Préstamo | Fecha límite | Prórroga hasta |
   |---|---|---|---|
   | 2 | lun 26/10/2026 | vie 13/11/2026 | vie 20/11/2026 |
   | 3 | lun 23/11/2026 (el 16/11 es descanso obligatorio) | vie 11/12/2026 | vie 18/12/2026 |
   | 4 | lun 14/12/2026 | vie 15/01/2027 (tercera semana de clases, contando las vacaciones del 21/12 al 5/01) | vie 22/01/2027 |
   | 5 | lun 18/01/2027 | vie 05/02/2027 | vie 12/02/2027 |
   | 6 | lun 08/02/2027 | jue 25/02/2027 (el viernes 26/02 es Consejo Técnico) | vie 05/03/2027 |

   Si un navegador ya tenía las fechas anteriores, la app las cambia por estas una sola vez al abrirla. Después pueden corregirse a mano en esa pestaña. «Agregar ronda» usa la misma regla.
4. En **Entregas por grupo**, marque cada cuaderno con **Recibido hoy** o escriba la fecha en que lo recibió.
5. Cuando tenga tiempo, entre a **Revisar** para valorar el contenido.
6. Active una sola vez el **guardado automático en GitHub** (siguiente apartado). Desde ese momento ya no hace falta exportar.

## Guardado automático en GitHub, paso a paso

1. Cree un token: en GitHub pulse su foto → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
2. **Token name**: «Reportes de lectura». **Expiration**: **Custom**, fin de ciclo (31/07/2027).
3. **Repository access**: **Only select repositories** → elija el repositorio de esta página.
4. **Permissions → Repository permissions → Contents**: **Read and write**.
5. Pulse **Generate token** y copie el texto que empieza con `github_pat_`.
6. En la aplicación, vaya a **Exportar y respaldos**. Usuario y repositorio se llenan solos cuando la abre desde GitHub Pages. Pegue el token y pulse **Conectar y guardar**.
7. Arriba, bajo el título, verá «Guardado en GitHub · hora». Cada cambio se sube a los pocos segundos.

En otra computadora o en el celular: abra la página, escriba la contraseña, pegue el mismo token en **Exportar y respaldos** y todo aparece. Si capturan en dos equipos, se combina registro por registro y gana el cambio más reciente.

Los datos se guardan en la rama `datos`, no en `main`, para que la página no se reconstruya con cada registro. Sin internet, la aplicación sigue funcionando y sube los cambios al reconectar.

## Cómo entran los datos de las fotografías

Cuando comparta fotografías de los reportes con Claude, recibirá un archivo `registros_*.json`. En **Exportar y respaldos → Restaurar o combinar respaldo** se combina con lo que ya tiene. Si un registro ya existe, se conserva el más reciente.

Formato del archivo de registros:

```json
{
  "tipo": "registros_reporte_lectura",
  "registros": [
    {
      "alumnoId": "6A-01",
      "ronda": "R1",
      "numReporte": "1",
      "fechaRecepcion": "2026-10-09",
      "libroDevuelto": "2026-10-09",
      "libro": {"titulo": "", "autor": "", "folio": "", "deRuta": ""},
      "produccion": "autonomo",
      "secciones": {"0": 2, "1": 2, "2": 1},
      "dimensiones": {"D1": 3, "D2": 2},
      "personalizacion": [0, 2],
      "siguiente": [0],
      "observacion": "",
      "fotos": "20261009_101500.jpg",
      "actualizado": "2026-10-09T18:00:00.000Z"
    }
  ]
}
```

`alumnoId` es grupo + número de lista (`6A-01`). Los nombres solo están en la lista privada.

## Formato usado y paquetes de corrección (versión 7.7)

- En **Revisar reporte**, «¿Con qué formato lo hizo?» permite valorar con la guía personalizada o con el formato de cuaderno del grado (campo `formato`: `"grado"` en los paquetes).
- La completitud se calcula solo con los apartados revisados; la ficha dice cuántos quedan por revisar en el cuaderno.
- Al importar un paquete, cada zona (`anclas`) se combina campo por campo: un paquete puede corregir la `razon` sin borrar el recuadro; una zona en `null` se borra. Las fechas ya capturadas (`fechaRecepcion`, `libroDevuelto`, `limitePropio`) no se sobrescriben salvo que el paquete traiga `"forzarFechas": true`.

## Reporte y libro: dos entregas distintas

El reporte y el libro se registran por separado, porque no siempre llegan juntos: hay familias que regresan el libro sin reporte y alumnos que entregan el reporte y se quedan con el libro unos días más.

- **Entregas por grupo**: la columna **Reporte recibido** guarda la fecha del cuaderno; la columna **Libro devuelto** guarda la fecha en que regresó el libro (botón **Libro hoy** o escriba la fecha). Debajo aparece la situación del alumno.
- **Revisar reporte**: el campo **Libro devuelto** está junto a la fecha de recepción.
- **Fecha límite acordada**: en **Revisar reporte**, junto a la fecha de recepción. Úsela cuando se acuerde con la familia otra fecha para ese alumno (por ejemplo, porque recibió tarde el libro): mientras no llegue esa fecha, el alumno aparece como «Pendiente» y no como «No entregado» ni «Prórroga». En los paquetes `.json` el campo es `limitePropio` (`AAAA-MM-DD`). Si existe, manda sobre la fecha que marca su guía.
- En los paquetes `.json` el campo es `libroDevuelto` (fecha `AAAA-MM-DD`; vacío = libro pendiente).

Con las dos fechas, cada alumno queda en una de cuatro situaciones por ronda (la ronda que «no aplica» por libro de dos rondas no entra):

| Situación | Reporte | Libro |
|---|---|---|
| Entregó reporte y libro | recibido (a tiempo, extemporáneo o fuera de plazo) | devuelto |
| Entregó reporte · libro pendiente | recibido | sin fecha |
| Entregó libro · sin reporte | no recibido (incluye prórroga o justificado) | devuelto |
| Sin reporte ni libro | no recibido | sin fecha |

En el **Panel**, la tarjeta **Reporte y libro** muestra el conteo por grupo y, en **Lista para exportar**, se elige el tipo de lista (una de las cuatro o «Las cuatro listas») y el grupo (o todos). Se descarga en **PDF**, **Word** o **CSV (Excel)**, o se copia para mandarla por mensaje. El PDF y el Word del Panel, el de cada grupo, el informe completo (sección 9) y el CSV interno también traen la fecha del libro.

Razón pedagógica: separar las dos entregas evita leer «no entregó» como un solo hecho. Un libro devuelto sin reporte pide una conversación sobre el reporte (¿qué apoyo faltó en casa o en el aula?); un reporte sin libro pide un recordatorio a la familia para que el libro siga circulando. Son acciones distintas para la escuela, la familia y, si hace falta, el especialista.

## Exportar para trabajar con 5.° y 6.°

En **Exportar y respaldos** hay dos descargas sin nombres:

- **Tabla resumida por grado** (recomendada): conteos por estado, niveles por dimensión, decisiones propias y forma de producción.
- **Una fila por reporte**: sin nombres, número de lista, fechas ni observaciones; filas revueltas y códigos al azar en cada descarga. Avisa cuando un grado tiene menos de 5 reportes, porque con tan pocas filas se puede adivinar de quién es cada una.

Ambas se abren en Excel o en Hojas de cálculo de Google para graficar.

## Para otros docentes

Para usarla con otra escuela basta con cambiar, dentro de `index.html`, la constante `SECCIONES` (apartados del formato de cada grado) y cargar su propia lista. Los criterios y su razón están en `CRITERIOS.md` y dentro de la pestaña **Criterios**.

## Exportar a PDF o Word

Arriba de cada pestaña hay una barra «Exportar esta vista: PDF · Word». Descarga lo que está viendo:

- **Panel**: indicadores (incluidos libros que sí y que no se entregaron), entregas por grupo con gráfica, calidad por dimensión y lista por recibir.
- **Entregas por grupo**: la tabla del grupo y la ronda elegidos.
- **Revisar reporte**: el reporte del alumno elegido, siempre en dos hojas carta (ver «El reporte de cada alumno» más abajo).
- **Criterios** y **Rondas y lista**.

El **informe completo** está en el Panel (botones oscuros a la derecha de la barra) y en «Exportar y respaldos». Reúne todas las rondas con reportes:

1. entregas por grupo, con gráfica y tabla;
2. completitud de los apartados;
3. calidad por dimensión y grupo;
4. distribución de niveles;
5. decisiones propias;
6. siguientes pasos;
7. forma de producción e ideas «a tu estilo»;
8. concentrado por alumno;
9. lista de quienes no han entregado;
10. la ficha de cada reporte, en el mismo formato de dos hojas que el reporte individual.

Formato de todos los documentos: hoja carta, márgenes estrechos (1.27 cm), tablas a todo lo ancho y con los mismos anchos de columna en PDF y Word. Antes de crear el archivo, la aplicación mide cada sección: si a la última hoja le sobraría un poco de texto (menos de un tercio de la hoja), reduce la letra y los espacios, hasta un 76 %, para que todo quepa en una hoja menos. Los títulos no quedan solos al pie de una hoja, las filas no se parten y las tablas cortas no se dividen.

### El reporte de cada alumno

- **Hoja 1, el reporte.** Portada del libro, datos del libro y del alumno, apartados de su guía con su valoración («Completo», «En construcción» o «Aún no aparece») y lo que se observa en cada uno, las cinco dimensiones de lectura, el acompañamiento y el comentario general «Lo que muestra este reporte». La aplicación reduce la letra lo necesario (hasta 55 %) para que esta hoja quepa siempre en una sola página.
- **Hoja 2, las evidencias.** Hasta cuatro fotografías del cuaderno con los recuadros marcados y una tabla que dice, para cada recuadro, en qué parte de la hoja está, quién lo escribió y qué se lee. Los documentos no mencionan nombres de archivo de las fotos. Si el reporte no tiene fotos, solo sale la hoja 1.

Los textos están redactados para que cualquier lector, también las familias, entienda lo que el reporte muestra. El campo **Nota para el docente** de «Revisar» no sale en ningún documento: es el lugar para lo que se conversa en privado con el alumno o con la familia.

Todos estos documentos llevan nombres: son internos. Las portadas aparecen cuando la página se abre desde GitHub Pages; si abre el archivo directamente desde su computadora, el documento sale sin portadas.

## Cambiar la contraseña

En **Rondas y lista → Contraseña de las listas** escriba la actual, la nueva dos veces y pulse **Cambiar contraseña**. Necesita tener activo el guardado en GitHub. La aplicación:

1. vuelve a cifrar las listas dentro de `index.html` del repositorio (GitHub publica la página nueva en uno o dos minutos);
2. guarda la llave de los datos envuelta con la contraseña nueva en `datos/llave.cifrada.json` (rama `datos`).

Los registros y las fotos ya guardados no se vuelven a cifrar y siguen funcionando. Los equipos con la sesión abierta no notan el cambio; en un equipo nuevo se escribe la contraseña nueva, también al conectar GitHub.

## Evidencias fotográficas

En **Revisar reporte → Evidencias del reporte** se agregan las fotos del cuaderno (JPG o PNG; desde el celular abre la cámara). Se guardan comprimidas en el navegador y, con GitHub activo, cifradas en `datos/evidencias/`.

Cada apartado y cada dimensión tiene un botón **📷 Evidencia**. Abre la foto con la zona marcada y tres textos: **Dónde está en la hoja y quién lo escribió** (por ejemplo «En la parte superior de la página derecha, con su letra; al lado, un adulto escribió con pluma azul…»), **Lo que se lee o se ve ahí** y **Lo que se observa**. Si deja «Dónde» vacío, la aplicación lo describe por la posición del recuadro («En la parte superior derecha de la fotografía»). Ahí mismo se puede marcar otra zona (**✏️ Marcar zona** y arrastrar sobre la foto) y corregir los textos. Las zonas de otros apartados aparecen punteadas con su número (E = encabezado, D1–D5 = dimensiones).

El paquete `evidencias_ronda1_*.json` que prepara Claude a partir de las fotos se importa en **Exportar y respaldos → Restaurar o combinar respaldo**: agrega las fotos y las zonas sin borrar lo que usted ya capturó. Un paquete también puede corregir: dejar «Sin marcar» un apartado o una dimensión que se había valorado sin foto, o borrar un registro hecho por error.

El PDF o Word de un reporte individual lleva sus fotos con las zonas marcadas en la hoja 2. El respaldo `.json` no lleva las fotos: viven en el navegador y en GitHub.
