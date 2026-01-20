# CLIMAECUADOR

Página simple en PHP que muestra datos climatológicos para ciudades de Ecuador (Quito, Guayaquil, Cuenca, Manta). Usa OpenWeatherMap cuando se configura la API key; de lo contrario usa datos de ejemplo.

Instalación y ejecución local

1. Opcional: obtener una API key gratuita en https://openweathermap.org/.
2. En PowerShell, exporta la variable de entorno (opcional):

```powershell
$env:OWM_API_KEY = "TU_API_KEY"
php -S localhost:8000
```

3. Abrir en el navegador: http://localhost:8000/

Archivos relevantes


Notas


# CLIMAECUADOR

Página en PHP que muestra datos climatológicos para algunas ciudades de Ecuador (Quito, Guayaquil, Cuenca, Manta). En local se ejecuta con PHP; GitHub Pages solo sirve contenido estático (no ejecuta PHP). Si subes solo los archivos PHP a GitHub Pages, no se verá igual que en `localhost`.

Demo local

- Inicia un servidor PHP en tu máquina y abre `http://localhost:8000/`:

```powershell
$env:OWM_API_KEY = "TU_API_KEY"   # opcional
php -S localhost:8000
```

- Abre: http://localhost:8000/

Archivos principales

- [index.php](index.php) — página principal (requiere PHP para ejecutarse).
- [assets/style.css](assets/style.css) — estilos con paleta cálida.
- [assets/sun.svg](assets/sun.svg) — gráfico decorativo.

Por qué no se ve igual en GitHub Pages

GitHub Pages solo sirve archivos estáticos (HTML, CSS, JS, imágenes). No ejecuta PHP en el servidor. Por eso, cuando abres la página en GitHub Pages verás el código fuente PHP o solo archivos estáticos, pero no la versión renderizada que ves en `localhost`.

Opciones para publicar la página y que se vea como en `localhost`

1) Usar un hosting que soporte PHP (recomendado)

	- Proveedores: Render, Hostinger, o cualquier hosting compartido con PHP.
	- Subes los archivos y configuras la variable `OWM_API_KEY` en el panel del servidor.

2) Convertir la versión dinámica a estática y publicar en GitHub Pages

	- Genera los archivos estáticos desde `localhost` y súbelos a la rama/`docs/` que uses para GitHub Pages.
	- Ejemplo con `wget` (WSL o si tienes wget en Windows):

```bash
php -S localhost:8000
wget --mirror --convert-links --adjust-extension --page-requisites --no-parent http://localhost:8000/ -P output_static
# luego mueve el contenido de output_static/localhost:8000/ a la carpeta docs/ del repo
```

	- Alternativa manual: abrir la página en el navegador, "Guardar como..." → `index.html`, y repetir para las rutas necesarias. Luego subir los archivos HTML/CSS/imagenes a la carpeta `docs/`.

3) Usar un servicio que ejecute PHP y desplegar allí (Heroku, Render, etc.)

Instrucciones rápidas para publicar en GitHub Pages (estático)

1. Genera la versión estática (ver método `wget` arriba o guarda manualmente).
2. Crea una carpeta `docs/` en la raíz del repo y coloca `index.html`, `assets/` y demás archivos estáticos dentro.
3. En la configuración del repositorio → Pages, selecciona la rama `main` y la carpeta `/docs` como fuente.

Notas sobre la API key

- La variable `OWM_API_KEY` solo se usa en tiempo de ejecución (PHP). Si creas una versión estática, los datos son los que se generaron en el momento del volcado; para datos en tiempo real necesitas un servidor que ejecute PHP.

¿Qué puedo hacer por ti ahora?

- Puedo generar la versión estática de la página y copiarla a `docs/` para que GitHub Pages la muestre tal cual la ves en `localhost`.
- O puedo preparar instrucciones paso a paso para desplegar en un hosting con PHP.
