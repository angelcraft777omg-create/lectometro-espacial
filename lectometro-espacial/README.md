# Lectómetro Espacial

Web para registrar la lectura de una clase de 5.º de Primaria: mapa espacial con la nave de cada tripulante, cuaderno de lecturas, retos de grupo, reseñas que el maestro acepta y una pestaña de seguimiento.

Es una web estática (HTML, CSS y JavaScript, sin compilación). Los datos se guardan en **Firebase** (plan gratuito), que es lo que permite que cada niño entre desde su dispositivo y todos vean lo mismo.

## Qué hay en la carpeta

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La página (estructura y textos). |
| `css/styles.css` | El diseño. |
| `js/app.js` | Toda la lógica. |
| `js/firebase-config.js` | Los datos de conexión a tu base de datos. **Es el único archivo que tienes que rellenar.** |
| `firestore.rules` | Las reglas de seguridad de la base de datos. Se pegan en la consola de Firebase. |
| `.nojekyll`, `.gitignore`, `.vscode/` | Ajustes para GitHub Pages, Git y VS Code. |

## Dos modos de funcionamiento

- **Modo de prueba** (mientras `js/firebase-config.js` esté vacío): los datos se guardan solo en el navegador donde abras la web. Sirve para trastear en tu ordenador. Eliges un código de maestro de 4 cifras la primera vez.
- **Modo real** (con Firebase configurado): los datos se comparten entre todos los dispositivos. Tú entras con **correo y contraseña**; cada tripulante entra con su **código de 4 cifras**.

## 1. Abrirlo en VS Code y probarlo

1. Descomprime la carpeta y ábrela en VS Code (`Archivo > Abrir carpeta`).
2. VS Code te sugerirá instalar la extensión **Live Server**. Acéptala.
3. Clic derecho sobre `index.html` > **Open with Live Server**. Se abre en modo de prueba.

## 2. Subirlo a GitHub

Crea un repositorio vacío en GitHub (por ejemplo `lectometro-espacial`) y, en la terminal de VS Code:

```bash
git init
git add .
git commit -m "Lectómetro Espacial"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/lectometro-espacial.git
git push -u origin main
```

El repositorio no contiene ningún dato del alumnado: los nombres, libros y reseñas viven en la base de datos, no en el código.

## 3. Crear la base de datos (Firebase)

Los nombres de los menús de la consola pueden variar un poco.

1. Entra en <https://console.firebase.google.com> y **crea un proyecto** (puedes desactivar Google Analytics).
2. **Firestore Database > Crear base de datos.** Elige una ubicación europea (por ejemplo `eur3`) y el **modo de producción**.
3. **Authentication > Método de acceso:** activa **Correo electrónico/contraseña**.
4. **Authentication > Usuarios > Agregar usuario:** crea tu cuenta de maestro con tu correo y una contraseña larga. Esta es la única cuenta que podrá gestionar la clase.
5. **Configuración del proyecto (rueda dentada) > Tus apps > App web (`</>`):** registra la app y copia los valores de `firebaseConfig` en `js/firebase-config.js`.
6. **Firestore Database > Reglas:** borra lo que haya, pega el contenido de `firestore.rules`, **cambia `TU_CORREO@ejemplo.com` por el correo del paso 4** y pulsa **Publicar**.

Guarda, y sube el cambio:

```bash
git add .
git commit -m "Conectar con Firebase"
git push
```

## 4. Publicarla (GitHub Pages)

1. En el repositorio de GitHub: **Settings > Pages**.
2. En *Build and deployment*, elige **Deploy from a branch**, rama **main**, carpeta **/(root)**, y guarda.
3. En un par de minutos estará en `https://TU_USUARIO.github.io/lectometro-espacial/`.
4. En Firebase, **Authentication > Configuración > Dominios autorizados**, añade `TU_USUARIO.github.io`.

En las cuentas gratuitas de GitHub, Pages necesita que el repositorio sea público.

## 5. Primer uso

1. Abre la web, pulsa **Soy el maestro** y entra con tu correo y contraseña.
2. En **Tripulación**, pega la lista de clase. Cada tripulante recibe su código de 4 cifras; con **Copiar lista de códigos** te la llevas para repartirla.
3. En **Retos de grupo**, crea los retos (hay un botón para crear cuatro sugeridos).
4. Da a las familias la dirección de la web y a cada niño su código.

## Cómo actualizar la web

Cambia lo que quieras en VS Code y vuelve a subirlo. GitHub Pages se actualiza solo:

```bash
git add .
git commit -m "Describe el cambio"
git push
```

## Seguridad y privacidad: qué protege y qué no

Conviene tenerlo claro porque son datos de menores.

**Lo que está protegido** (por `firestore.rules`):

- Solo tu cuenta de maestro puede añadir o quitar tripulantes, registrar libros y páginas, crear retos, y aceptar, editar o borrar reseñas.
- Nadie puede ver la lista de códigos salvo tú. Para entrar, la web comprueba un único código cada vez.
- Un tripulante solo puede enviar reseñas pendientes; no se publican ni suman puntos hasta que tú las aceptas.

**Lo que no está protegido:**

- La web es pública: cualquiera que tenga la dirección puede **leer** los nombres de la tripulación, sus libros y sus reseñas (aunque sin código no entra en la web, los datos se pueden consultar con conocimientos técnicos). Usa solo el **nombre de pila o iniciales**, nunca apellidos completos ni otros datos personales.
- Un código de 4 cifras es fácil de adivinar probando. Sirve para organizar una clase, no como contraseña. Lo peor que puede hacer alguien con un código ajeno es enviar una reseña pendiente, que tú puedes rechazar.
- Los valores de `js/firebase-config.js` no son secretos; la seguridad depende de las reglas. No publiques las reglas sin cambiar el correo.

Antes de usarla con la clase, coméntalo con la dirección del centro o su responsable de protección de datos: es quien debe decidir si hace falta informar a las familias o pedir su consentimiento.

## Límites del plan gratuito

El plan gratuito de Firebase (Spark) tiene límites diarios de lecturas y escrituras, de sobra para una clase. Puedes ver el consumo en la consola, en **Firestore Database > Uso**. Si algún día se superan, la web deja de guardar hasta el día siguiente; no se cobra nada.

## Problemas frecuentes

- **Pone «Modo de prueba: solo en este dispositivo»:** `js/firebase-config.js` está vacío o no se ha subido.
- **«No tienes permiso para guardar ese cambio» siendo el maestro:** el correo de `firestore.rules` no coincide exactamente con el de tu cuenta, o no has pulsado **Publicar** en las reglas.
- **«Correo o contraseña incorrectos»:** revisa el usuario en *Authentication > Usuarios*; desde ahí puedes restablecer la contraseña.
- **Un tripulante ha olvidado su código:** en *Tripulación* lo ves, o pulsa **Nuevo código** para darle otro.
- **Pantalla «Sin conexión»:** no se han podido cargar las librerías de Firebase; comprueba internet y recarga.
- **Actualizar Firebase:** las tres líneas `firebasejs/10.12.2/` de `index.html` indican la versión de la librería. Para cambiarla, sustituye el número en las tres.

## Personalizar

En `js/app.js`, al principio, están los valores que más querrás tocar:

- `BONUS`: puntos de regalo por libro terminado (50).
- `MIN_WORDS`: palabras mínimas de una reseña (20).
- `STATIONS`: paradas del mapa, con sus puntos y rangos.
- `GENRES`: tipos de libro.
- `SUGGESTED`: los cuatro retos sugeridos (pensados para 22 tripulantes).
