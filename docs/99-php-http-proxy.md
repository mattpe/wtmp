# Simple PHP HTTP proxy

Example of quick'n'dirty HTTP CORS proxy solution with PHP.

NOTE: Use only for temporary testing of apps. Not designed for production use.

Usage: `GET https://my.server.net/proxy.php/<API-SUBPATH-AND-PARAMS>`

File _proxy.php_:

```php
<?php
// Set CORS headers, by default accepts everything
header("Access-Control-Allow-Origin: *");
// Set correct api url here, e.g. https://www.example.com/api
$api_uri = "<API_URI>";
if( !$_SERVER['PATH_INFO'] ) {
  die('404 sorry');
}
$req_uri = $_SERVER['REQUEST_URI'];
$req_api = explode("proxy.php", $req_uri)[1];
$jsonurl = $api_uri.$req_api;
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $jsonurl);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true );
$result = curl_exec($ch);
if( !$result ) {
  die('Error: "' . curl_error($ch) . '" - Code: ' . curl_errno($ch));
}
curl_close($ch);
echo $result;
```
