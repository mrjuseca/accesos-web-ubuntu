# accesos-web-ubuntu
Playbook para gestionar accesos directos a aplicaciones web en Ubuntu usando Brave.

# Playbook: Gestión de accesos directos a aplicaciones web en Ubuntu con Brave

## З Objetivo

Este manual técnico tiene como propósito documentar el proceso para crear, personalizar y mantener accesos directos (`.desktop`) a aplicaciones web (PWA) en Ubuntu, utilizando el navegador Brave. Está orientado a usuarios técnicos que deseen integrar sus aplicaciones web al entorno gráfico de forma limpia, segura y funcional.

---

##  Estructura recomendada

| Elemento              | Ruta sugerida                                      |
|-----------------------|----------------------------------------------------|
| Archivos `.desktop`   | `~/.local/share/applications/`                    |
| Iconos personalizados | `~/.local/share/icons/webapps/`                   |

---

##  Pasos para crear un acceso directo personalizado

### 1. Crear carpeta para iconos (si no existe)

```bash
mkdir -p ~/.local/share/icons/webapps
```

### 2. Descargar el icono oficial desde el sitio web
1. Abre Brave y navega al sitio deseado.
2. Presiona Ctrl + Shift + I para abrir las herramientas de desarrollador.
3. Busca el favicon o logo (.png, .svg, .ico) en la pestaña Elements.
4. Copia la URL del recurso y descárgalo:

```bash
wget -O ~/.local/share/icons/webapps/nombre_app.png https://URL-del-icono
```

Si el icono está en formato .ico, conviértelo a .png:
```bash
sudo apt install imagemagick
convert icono.ico icono.png
```

### 3. Crear el archivo .desktop
```bash
nano ~/.local/share/applications/nombre_app.desktop
```

Contenido base:
```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=Nombre de la Aplicación
Comment=Descripción breve
Exec=brave --app=https://URL-de-la-app
Icon=/home/[usuario]/.local/share/icons/webapps/nombre_app.png
Terminal=false
Categories=Network;WebBrowser;
StartupWMClass=Brave-browser
```

Reemplaza usuario por tu nombre de usuario real.

#### Categorías aceptadas en archivos .desktop
A continuación te presento las más comunes y recomendadas, agrupadas por tipo de aplicación:

 Aplicaciones de sistema

System
Settings
Utility
FileTools
FileManager
TerminalEmulator

 Internet y comunicación

Network
WebBrowser
Email
InstantMessaging
Chat
News

Educación y ciencia

Education
Science
Math
Dictionary

 Multimedia

Audio
Video
Graphics
Photography
Music
Player
Recorder

 Desarrollo

Development
IDE
Debugger
Building
Profiling
RevisionControl

 Oficina y productividad

Office
WordProcessor
Spreadsheet
Presentation
Calendar
ContactManagement

 Juegos

Game
ActionGame
AdventureGame
ArcadeGame
BoardGame
CardGame

З Ejemplo práctico
Para una aplicación como ChatGPT, podrías usar:
```bash
Categories=Network;ArtificialIntelligence;
```

Aunque ArtificialIntelligence no es una categoría estándar, puedes incluirla como personalizada. Las categorías estándar deben ir primero para asegurar compatibilidad.

Buenas prácticas

Usa mínimo una categoría estándar reconocida por freedesktop.
Puedes añadir categorías personalizadas, pero no serán interpretadas por todos los entornos.
Separa las categorías con punto y coma ; y no uses espacios.
No pongas una categoría vacía ni termines con punto y coma innecesario.

### 4. Asignar permisos de ejecución

```bash
chmod +x ~/.local/share/applications/nombre_app.desktop
```

### 5. Actualizar la base de datos de accesos

```bash
update-desktop-database ~/.local/share/applications/
```

И Verificación

El acceso debe aparecer en el men煤 de aplicaciones.
Al hacer clic, se abrirá Brave en modo ventana independiente.
El icono debe mostrarse correctamente.


洜锔?Gestión de accesos Brave instalados como PWA
Listar accesos generados automáticamente

```bash
ls ~/Escritorio/brave-*.desktop
```

Extraer App IDs desde los accesos
El --app-id es un identificador 煤nico que Brave asigna a cada aplicación web instalada como PWA. Este ID permite lanzar la aplicación en una ventana independiente, reutilizando su configuración, cookies y estado.
Cuando instalas una PWA desde Brave, se genera un archivo .desktop en tu Escritorio con una línea como esta:

```bash
Exec=/snap/brave/567/opt/brave.com/brave/brave-browser --profile-directory=Default --app-id=endnfacgbanmehmndpogddlbiheemmdo
```

El valor de --app-id= es el identificador que necesitas.

Opción 1: Extraer todos los App IDs desde los accesos en el Escritorio

```bash
grep -hoP 'app-id=\K\S+' ~/Escritorio/brave-*.desktop
```

Opción 2: Ver App ID junto con el nombre del acceso

```bash
grep -h 'Exec=' ~/Escritorio/brave-*.desktop | awk -F '--app-id=' '{print $2}' | while read id; do
  name=$(grep -h "Name=" ~/Escritorio/brave-$id-*.desktop | head -n1 | cut -d= -f2)
  echo "$name: $id"
done
```

Buenas prácticas

Usar nombres claros y consistentes para los accesos.
Mantener iconos actualizados y en formato .png.
Evitar rutas absolutas de Snap (/snap/brave/...) por posibles cambios tras actualizaciones.
Documentar los accesos en un archivo README.md si se publica en GitHub.
Validar que el comando brave esté disponible en el sistema (which brave).


 Recursos adicionales

brave://apps/
https://icons8.com
https://www.flaticon.com
https://imagemagick.org


Autor
Este playbook fue elaborado para facilitar la integración de aplicaciones web al entorno gráfico de Ubuntu, promoviendo buenas prácticas de administración en sistemas Linux.

 Licencia
Este documento puede ser reutilizado bajo la licencia https://opensource.org/licenses/MIT.
