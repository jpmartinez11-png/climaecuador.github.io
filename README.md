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

- `index.php` - página principal.
- `assets/style.css` - estilos con paleta cálida.
- `assets/sun.svg` - gráfico decorativo.

Notas

- Si no configuras `OWM_API_KEY`, la página mostrará datos de ejemplo.
- Para producción, configura la variable de entorno en tu servidor web y no dejes la clave en el código.
