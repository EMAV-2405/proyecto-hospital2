Para integrar Firebase en tu aplicación Flutter de forma profesional, el camino estándar es utilizar **Node.js** para gestionar las herramientas de línea de comandos (CLI).

Aquí tienes la guía paso a paso para configurar tu entorno en Windows.

---

## 1. Software necesario para Node.js y npm
Para instalar **npm** (Node Package Manager) de manera global en Windows, no necesitas descargar npm por separado; este viene incluido automáticamente al instalar **Node.js**.

* **Instalador oficial:** Descarga el instalador `.msi` desde [nodejs.org](https://nodejs.org/). 
* **Versión recomendada:** Elige la versión **LTS** (Long Term Support), ya que es la más estable para desarrollo.

---

## 2. Procedimiento de Instalación y Verificación

### Cómo verificar si ya están instalados
Antes de instalar nada, abre tu terminal (PowerShell o CMD) y escribe:

1.  `node -v` (Muestra la versión de Node.js)
2.  `npm -v` (Muestra la versión de npm)

> **Nota:** Si ves un número de versión (ej. `v20.12.0`), ya lo tienes. Si recibes un error de "comando no reconocido", procede con la instalación.

### Instalación paso a paso (si no lo tienes)
1.  **Ejecuta el instalador:** Abre el archivo `.msi` descargado.
2.  **Siguiente, Siguiente:** Acepta los términos.
3.  **Configuración crucial:** Asegúrate de que la opción **"Add to PATH"** esté seleccionada (esto permite que los comandos funcionen de manera "global").
4.  **Herramientas adicionales:** El instalador te preguntará si quieres instalar herramientas para módulos nativos (Chocolatey). Generalmente no es obligatorio para Firebase, pero es recomendable aceptarlo.
5.  **Reinicia la terminal:** Una vez finalizado, cierra y vuelve a abrir tu terminal para que reconozca los nuevos comandos.

---

## 3. Instalación de Firebase CLI (firebase-tools)
Con Node.js ya en tu sistema, instalaremos las herramientas de Firebase para que estén disponibles en cualquier carpeta de tu computadora.

### Comando de instalación global
Ejecuta este comando en tu terminal:
```bash
npm install -g firebase-tools
```
* El parámetro `-g` significa **global**, lo que te permite usar el comando `firebase` en cualquier proyecto de Flutter.

---

## 4. Cómo usar Firebase CLI en Flutter

### Acceder a Firebase con tu Cuenta de Google
Para vincular tu terminal con tu cuenta real de Firebase:
1.  En la terminal, escribe:
    ```bash
    firebase login
    ```
2.  Se abrirá automáticamente una ventana en tu navegador web.
3.  Selecciona tu cuenta de Google y otorga los permisos necesarios.
4.  Al terminar, verás un mensaje en la terminal diciendo: **"Success! Logged in as [tu-correo]"**.

### Comandos esenciales de firebase-tools
| Comando | Propósito |
| :--- | :--- |
| `firebase projects:list` | Muestra todos tus proyectos creados en la consola de Firebase. |
| `firebase init` | Inicia la configuración de servicios (Hosting, Functions, etc.) en tu carpeta actual. |
| `firebase logout` | Cierra la sesión de tu cuenta en la computadora. |

---

## 5. El toque final para Flutter: FlutterFire CLI
Aunque ya tienes Firebase CLI, para Flutter necesitas una herramienta adicional que automatice la conexión de los archivos `.dart`:

1.  **Instala FlutterFire CLI:**
    ```bash
    dart pub global activate flutterfire_cli
    ```
2.  **Configura tu App:** Dentro de la carpeta de tu proyecto Flutter, ejecuta:
    ```bash
    flutterfire configure
    ```
    Este comando te permitirá seleccionar tu proyecto de Firebase y generará automáticamente el archivo `firebase_options.dart` con todas las credenciales necesarias para Android, iOS y Web.

¿Ya tienes creado tu proyecto en la consola web de Firebase o necesitas ayuda para crear el ID del proyecto primero?
