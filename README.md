# Control de Reportes de Lectura · Biblioteca Viva

Aplicación de una sola página para llevar el control de entregas y la revisión de los reportes de lectura de 1.° a 6.° de primaria. Funciona en el navegador y guarda los registros, cifrados, en el mismo repositorio de GitHub.

Contenido del repositorio:

- `index.html`: la aplicación. Trae cifradas la lista de 100 alumnos, la ruta lectora de seis libros de cada uno y los apartados de su guía personalizada de la Ronda 1.
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
5. Descomprima el zip en su computadora. Arrastre a esa ventana `index.html`, `README.md`, `CRITERIOS.md`, `.gitignore` **y la carpeta `portadas` completa** (en Chrome o Edge puede arrastrar la carpeta tal cual). Si su computadora oculta `.gitignore`, puede omitirlo.
6. Abajo, en **Commit changes**, pulse el botón verde **Commit changes**.
7. Vaya a la pestaña **Settings** del repositorio (arriba, con ícono de engrane).
8. En el menú de la izquierda, pulse **Pages**.
9. En **Build and deployment → Source** elija **Deploy from a branch**. En **Branch** elija `main` y la carpeta `/ (root)`. Pulse **Save**.
10. Espere uno o dos minutos y recargue la página de **Pages**: aparecerá la dirección, parecida a `https://SU-USUARIO.github.io/reportes-lectura/`.

## Primer uso

1. Abra la dirección de su página.
2. Escriba la contraseña de las listas y pulse **Abrir listas**. Se abre directamente **Entregas por grupo**.
3. La **Ronda 1** ya trae: límite 09/10/2026 y extemporáneo hasta el lunes 12/10/2026, **por única ocasión**. La app lo recuerda con un aviso en el Panel y en Entregas por grupo, y las rondas nuevas se crean sin extemporáneo.
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

## Exportar para trabajar con 5.° y 6.°

En **Exportar y respaldos** hay dos descargas sin nombres:

- **Tabla resumida por grado** (recomendada): conteos por estado, niveles por dimensión, decisiones propias y forma de producción.
- **Una fila por reporte**: sin nombres, número de lista, fechas ni observaciones; filas revueltas y códigos al azar en cada descarga. Avisa cuando un grado tiene menos de 5 reportes, porque con tan pocas filas se puede adivinar de quién es cada una.

Ambas se abren en Excel o en Hojas de cálculo de Google para graficar.

## Para otros docentes

Para usarla con otra escuela basta con cambiar, dentro de `index.html`, la constante `SECCIONES` (apartados del formato de cada grado) y cargar su propia lista. Los criterios y su razón están en `CRITERIOS.md` y dentro de la pestaña **Criterios**.
