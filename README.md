<?php
// CLIMAECUADOR - Página principal
// Configuración de la API: coloca tu API key de OpenWeatherMap en la variable $apiKey
$apiKey = getenv('OWM_API_KEY') ?: 'YOUR_API_KEY';
$cities = ['Quito','Guayaquil','Cuenca','Manta'];

function fetch_weather($city, $apiKey) {
    if (!$apiKey || $apiKey === 'YOUR_API_KEY') return null;
    $url = "https://api.openweathermap.org/data/2.5/weather?q=" . urlencode($city) . ",EC&units=metric&appid=" . $apiKey . "&lang=es";
    $opts = ["http" => ["timeout" => 5]];
    $context = stream_context_create($opts);
    $json = @file_get_contents($url, false, $context);
    if (!$json) return null;
    $data = json_decode($json, true);
    return $data ?: null;
}

// Datos de ejemplo en caso de no tener API key o fallo de la API
$sample_data = [
    'Quito' => [
        'name' => 'Quito',
        'main' => ['temp' => 14, 'humidity' => 72],
        'weather' => [['main' => 'Clouds','description'=>'nubes dispersas','icon'=>'03d']],
        'wind' => ['speed' => 3.5]
    ],
    'Guayaquil' => [
        'name' => 'Guayaquil',
        'main' => ['temp' => 28, 'humidity' => 65],
        'weather' => [['main' => 'Clear','description'=>'despejado','icon'=>'01d']],
        'wind' => ['speed' => 2.1]
    ],
    'Cuenca' => [
        'name' => 'Cuenca',
        'main' => ['temp' => 16, 'humidity' => 78],
        'weather' => [['main' => 'Rain','description'=>'lluvias ligeras','icon'=>'10d']],
        'wind' => ['speed' => 4.0]
    ],
    'Manta' => [
        'name' => 'Manta',
        'main' => ['temp' => 26, 'humidity' => 70],
        'weather' => [['main' => 'Clouds','description'=>'parcialmente nublado','icon'=>'02d']],
        'wind' => ['speed' => 5.2]
    ]
];

function recommend($weather, $temp){
    $tips = [];
    $main = strtolower($weather);
    if (strpos($main, 'rain') !== false || strpos($main,'lluv') !== false) $tips[] = 'Lleva paraguas o impermeable.';
    if (strpos($main, 'clear') !== false || strpos($main,'despej') !== false) $tips[] = 'Protector solar recomendado en horas de sol intenso.';
    if ($temp >= 30) $tips[] = 'Hidratate y evita el sol directo en las horas pico.';
    if ($temp <= 10) $tips[] = 'Abrígate, las noches pueden ser frías.';
    if (empty($tips)) $tips[] = 'Clima agradable, disfruta tu día.';
    return implode(' ', $tips);
}

$results = [];
foreach ($cities as $c) {
    $data = fetch_weather($c, $apiKey);
    if (!$data) $data = $sample_data[$c];
    $results[] = $data;
}
?>
<!doctype html>
<html lang="es">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>CLIMAECUADOR</title>
    <link rel="stylesheet" href="assets/style.css">
    <link rel="preconnect" href="https://fonts.gstatic.com">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;500;700&display=swap" rel="stylesheet">
</head>
<body>
<header class="hero">
    <div class="hero-inner">
        <h1>CLIMAECUADOR</h1>
        <p class="subtitle">Datos climatológicos en tiempo real — Ecuador</p>
    </div>
    <div class="sun-deco">
        <?php echo file_get_contents(__DIR__ . '/assets/sun.svg'); ?>
    </div>
</header>
<main class="container">
    <section class="cards">
        <?php foreach ($results as $r):
            $name = $r['name'] ?? '—';
            $temp = isset($r['main']['temp']) ? round($r['main']['temp']) : '—';
            $desc = $r['weather'][0]['description'] ?? '—';
            $icon = $r['weather'][0]['icon'] ?? '01d';
            $hum = $r['main']['humidity'] ?? '—';
            $wind = $r['wind']['speed'] ?? '—';
            $rec = recommend($desc, $temp);
        ?>
        <article class="card">
            <div class="card-top">
                <h2><?php echo htmlspecialchars($name); ?></h2>
                <img class="icon" src="https://openweathermap.org/img/wn/<?php echo $icon; ?>@2x.png" alt="icon">
            </div>
            <div class="card-body">
                <div class="temp"><?php echo $temp; ?>°C</div>
                <div class="desc"><?php echo htmlspecialchars(ucfirst($desc)); ?></div>
                <ul class="meta">
                    <li>Humedad: <strong><?php echo $hum; ?>%</strong></li>
                    <li>Viento: <strong><?php echo $wind; ?> m/s</strong></li>
                </ul>
                <p class="rec"><strong>Recomendación:</strong> <?php echo htmlspecialchars($rec); ?></p>
            </div>
        </article>
        <?php endforeach; ?>
    </section>

    <aside class="tips">
        <h3>Consejos generales</h3>
        <ul>
            <li>Revisa la previsión antes de viajar entre regiones — el clima cambia con la altitud.</li>
            <li>Lleva capas: en la sierra la temperatura puede bajar de forma rápida.</li>
            <li>En la costa, protege tu piel y mantente hidratado.</li>
        </ul>
        <div class="small-note">Para usar datos en tiempo real agrega tu clave OWM en la variable `OWM_API_KEY` (o reemplaza `YOUR_API_KEY` en el archivo).</div>
    </aside>
</main>
<footer class="footer">
    <div>Hecho con ☀️ y colores cálidos — CLIMAECUADOR</div>
</footer>
</body>
</html>
