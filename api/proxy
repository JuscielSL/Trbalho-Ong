<?php
session_start();

$api_url = "http://localhost/Ong";
$endpoint = $_GET['endpoint'] ?? '';
$method = $_SERVER['REQUEST_METHOD'];

$url = $api_url . '/' . ltrim($endpoint, '/');

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, $method);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

$headers = [];
if(isset($_SESSION['token'])) {
    $headers[] = 'Authorization: Bearer ' . $_SESSION['token'];
}

if($method == 'POST' || $method == 'PUT') {
    if(isset($_FILES) && !empty($_FILES)) {
        // Multipart form data
        $headers[] = 'Content-Type: multipart/form-data';
        curl_setopt($ch, CURLOPT_POSTFIELDS, $_POST + $_FILES);
    } else {
        $headers[] = 'Content-Type: application/json';
        $input = file_get_contents('php://input');
        curl_setopt($ch, CURLOPT_POSTFIELDS, $input);
    }
}

curl_setopt($ch, CURLOPT_HTTPHEADER, $headers);

$response = curl_exec($ch);
$http_code = curl_getinfo($ch, CURLINFO_HTTP_CODE);
curl_close($ch);

http_response_code($http_code);
header('Content-Type: application/json');
echo $response;
?>
