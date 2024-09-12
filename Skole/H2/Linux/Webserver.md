---
tags:
  - H2
  - Linux
  - Webserver
  - Skole
---
Bruger [[Master]] som grundlag

### Technical
IPv4 Config:
Manual
Addresse: 10.0.0.5
Gateway: 10.0.0.1
DNS Server: 10.0.0.2
Search Domain: denlange.local

## Opsætning
#### WebServer
lav følgende mapper i /var/www/html:
- patrick
- pedersen
Inde i hver mappe skal der laves en index.html med følgende indhold:
```HTML
<!DOCTYPE html>
<html>
<head>
	<title>Mappens Navn</title>
	<meta charset="utf-8" />
</head>
<body>
	<h1>Hej Verden</h1>
	<h1>Mappens Navn</h1>
</body>
</html>
```

Lav eller Ændre følgende fil i "/etc/httpd/conf.d/vhost.conf" og tilføj følgende for hver vhost og ret navne ind:
```sh
<VirtualHost *:80 *:443>
	ServerAdmin webmaster@denlange.local
	DocumentRoot /var/www/html/denlange
	ServerName www.denlange.local
	ServerAlias www
	ErrorLog logs/denlange-error_log
	CustomLog logs/denlange-access_log common
	<Directory "/var/www/html/denlange">
		DirectoryIndex index.html index.php
		Options FollowSymLinks
		AllowOverride All
		Require all granted
	</Directory>
</VirtualHost>
```
#### DNS
Tilføj følgende til bunden af denlange.local.db:
- www.patrick.local IN CNAME www
- www.pedersen.local IN CNAME www

### SSL
Country: DK
State: Zealand
Locality: Copenhagen
Organization: denlange
Common Name: denlange.local
Email: webmaster@denlange.local

Pass: Passw0rd

### Firewall
Inde i OPNsense, skal vi port-forwarde 80 og 443 til webserver.

## PHP Site
Et site der kan indsætte et medlem med deres adresse og postnummer.
Samt returnere en liste over disse medlemmer.
Det består af 4 filer:

#### Index.php
```PHP
<?php
ini_set('display_errors', 'On');
error_reporting(E_ALL);
?>

<html>
    <head>
        <title>Form</title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width">
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
        <style>
            table {
                border-collapse: collapse;
                width: 50%;
                margin: 0 auto;
                border-spacing: 0;
                display: table;
                border: 1px solid #ccc;
            }
            table tr {
                border-bottom: 1px solid #ddd
            }
            table tr:nth-child(2n+1) {
                background-color: #fff;
            }
            table tr:nth-child(2n) {
                background-color: #E7E9EB;
            }
            table th {
                padding-top: 11px;
                padding-bottom: 11px;
                background-color: #4CAF50;
                color: white;
            }
            th {
                text-align: left;
                padding: 8px;
                display: table-cell;
                vertical-align: top;
            }
            td {
                text-align: left;
                padding: 8px;
                display: table-cell;
                vertical-align: top;
            }
            form {
                width: 100%;
                max-width: 500px;
                margin: 0 auto;
                padding: 10px;
                border: 1px solid #ccc;
                margin-bottom: 2rem;
            }
            input[type=text] {
                width: 100%;
                padding: 12px 20px;
                margin: 8px 0;
                box-sizing: border-box;
                border: 1px solid #ccc;
                border-radius: 4px;
            }
            input[type=submit] {
                width: 100%;
                background-color: #4CAF50;
                color: white;
                padding: 14px 20px;
                margin: 8px 0;
                border: none;
                cursor: pointer;
            }
        </style>
    </head>
    <body>
        <form action="form.php" method="post">
            <input type="text" name="name" placeholder="Name" required>
            <input type="text" name="address" placeholder="Address" required>
            <input type="text" name="postnr" placeholder="Postnr" required>
            <input type="submit" value="Submit">
        </form>
        <table>
            <tr>
                <th>Name</th>
                <th>Address</th>
                <th>Postnr</th>
                <th>Byen</th>
            </tr>
            <?php
                require 'retur.php';
            ?>
        </table>
    </body>
</html>
```

#### Retur.php
```PHP
<?php
require_once 'config.php';

$connection = new mysqli($serverhost, $dbuser, $dbpass, $dbname, $serverport);
if ($connection->connect_error) {
    die("Connection failed: " . $connection->connect_error);
} 

// Get data from database
$sql = "
SELECT
    *
FROM Medlemmer
INNER JOIN Postnumre ON Medlemmer.Postnr = Postnumre.Postnr
";
$result = $connection->query($sql);

// Print data from database
if ($result->num_rows > 0) {
    while($row = $result->fetch_assoc()) {
        echo "<tr>";
        echo "<td>" . $row["Navn"] . "</td>";
        echo "<td>" . $row["Adresse"] . "</td>";
        echo "<td>" . $row["Postnr"] . "</td>";
        echo "<td>" . mb_convert_encoding($row["Byen"], 'UTF8', 'ISO-8859-1') . "</td>";
        echo "</tr>";
    }
} else {
    echo "<tr>";
    echo "<td colspan='4'>No data</td>";
    echo "</tr>";
}
```

#### Form.php
```PHP
<?php
require_once 'config.php';

$connection = new mysqli($serverhost, $dbuser, $dbpass, $dbname, $serverport);
if ($connection->connect_error) {
    die("Connection failed: " . $connection->connect_error);
}

$name = $_POST["name"];
$address = $_POST["address"];
$postnr = $_POST["postnr"];
  
$sql = "INSERT INTO Medlemmer (Navn, Adresse, Postnr) VALUES ('$name', '$address', '$postnr')";
$result = $connection->query($sql);
  
header("Location: index.php");
```

#### Config.php
```PHP
<?php
// Turn on error reporting
ini_set('display_errors', 'On');
error_reporting(E_ALL);

// Connect to database
$serverhost = "sql.denlange.local";
$serverport = "3306";
$dbname = "tec";
$dbuser = "patrick";
$dbpass = "Passw0rd";
```