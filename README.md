# My First Linux Project
===

Introduction
===
<i class="fa fa-file-text"></i> The primary objective is to employ Nmap, a robust network scanning tool, for the systematic exploration of IP addresses and ports within a given network. Through this initiative, I intend to demonstrate my proficiency in network reconnaissance and security auditing, laying a foundation for my hands-on experience in the Linux domain.

Step
===
## Set Up Your Environment
```Linux
sudo apt-get update
sudo apt-get nmap
sudo apt-get install apache2 php libapache2-mod-php
```

## identify the port
```linux
ifconfig
```

## code for application
```linux
mkdir ~/network_scanner
cd ~/network_scanner
nano index.html 
```
Add the below code to index.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Network Scanner</title>
</head>
<body>
  <h1>Simple Network Scanner</h1>
  <form id="scanForm">
    <label for="target">Enter IP or Range:</label>
    <input type="text" id="target" name="target" required>
    <button type="button" onclick="scanNetwork()">Scan</button>
  </form>

  <h2>Scan Results:</h2>
  <pre id="scanResults"></pre>

  <script>
    function scanNetwork() {
      var target = document.getElementById('target').value;
      var resultsContainer = document.getElementById('scanResults');

      // Perform AJAX request to a backend script
      // Pass the target to your server-side script that interacts with Nmap

      // Example AJAX request using Fetch API
      fetch('/scan?target=' + encodeURIComponent(target))
        .then(response => response.text())
        .then(data => resultsContainer.textContent = data)
        .catch(error => console.error('Error:', error));
    }
  </script>
</body>
</html>
```

## code for scan
```linux
mkdir ~/network_scanner/backend
cd ~/network_scanner/backend
nano scan.php 
```
Add the code to scan.php
```php
<?php
$target = $_GET['target'];
$escapedTarget = escapeshellarg($target);

// Execute Nmap and capture output
exec("nmap $escapedTarget", $output);

// Return Nmap output
echo implode("\n", $output);
?>
```
## configure for webserver
```linux
sudo nano /etc/apache2/sites-available/000-default.conf
```
Add within the <VirtualHost> block add
<Directory "/path/to/your/project">
    AllowOverride All
</Directory>

## restart the web application
```linux
sudo service apache2 restart
```

## Cron Job Configuration
```linux
sudo crontab -e
```
Add this at the bottom
```
*/10 * * * * nmap 192.168.1.0/24 -oN /var/www/html/nmap.html
```



