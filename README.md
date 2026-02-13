https://piezasparawebsite.github.io/status/

# como se usa ? :

primero este repositorio (proyecto)
va alojado en un dominio.

## otro server

en otro dominio creas un archivo 
llamado : check_status.php y en su 
interior copia y pega el siguiente 
script php :

```php
<?php
header("Access-Control-Allow-Origin: *");
header('Content-Type: application/json');

if (!isset($_GET['url'])) {
    echo json_encode(['status' => 'error']);
    exit;
}

$url = filter_var($_GET['url'], FILTER_VALIDATE_URL);
if (!$url) {
    echo json_encode(['status' => 'invalid']);
    exit;
}

$ch = curl_init($url);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_NOBODY, true); // Solo queremos el encabezado para ir rápido
curl_setopt($ch, CURLOPT_TIMEOUT, 5);
curl_exec($ch);
$code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

// Si el código es 200, 301 o 302, lo consideramos Online
$is_online = ($code >= 200 && $code < 400);

echo json_encode(['status' => $is_online ? 'online' : 'offline']);
```

## modificacion del index.html obligatoria

en tu index.html busca la linea:
```const response = await fetch(`https://tu-nombre.dominio/check_status.php?url=${encodeURIComponent(url)}`);```
y modifica el url colocando tu nombre de dominio 