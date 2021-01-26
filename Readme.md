
Klicke hier für die [deutsche Beschreibung!](#deutsch)

<br><br>

# What is myAJAX?
myAJAX is a function which you can help to call up AJAX calls in a simple way!

You can integrate files (.html, .php, .txt etc.) at a defined point (ID) in the DOM (HTML).

## Benefits!
When using this feature:
* files are integrated in a defined location (ID),
* all Javascript scripts will executed for the called HTML files,
* PHP / HTML and Javascript scripts are executed in the PHP files,
* the src attribute is taken into account. The named file is integrated and executed, e.g .:
 
    `<script src="myScript.js"><script>` 

* Form data will be transmitted with "GET" or "POST" when called,
* a callback function can be called which has access to all essential data (see example 3 below)!<br><br><br><br>
# Installation
## for testing purposes
Simply link the script tag as follows:
```
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/myajaxcall@1.0.23/myajax_bundle.min.js"></script>
</head>
<body>
</body>
</html>
```
## locale installation
1. create a package.json

     `npm init` <br> <br>
2. Install the package

     `npm i myajaxcall` <br> <br>
3. Include (e.g. in the index.html)
```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
</body>
</html>
```
<br> <br>
# Working method and examples
The function is called with:

`lib.myAJAX(filename, id, form, callback)`

Not all parameters are necessary! If a parameter has to be skipped, you have to fill this up with <b>null</b>. Example:

`lib.myAJAX('JSON.php', null, null, myCallback)`<br><br>

# simple examples and explanations <br> <br>
## Example 1: Filename and ID
The <i>index.html</i> calls the <i>test.html</i> and inserts its content in the h1 tag in place of the span tag with the ID <i>content</i>.
___
<b>test.html</b>
```
<i>Welt</i>
```
<br><br>
___
<b>index.html</b>

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <h1>Hallo <span id="content"></span>!</h1>

    <script>
        new lib.myAJAX("test.html","content");
    </script>
</body>
</html>
```
## Example 2: Transfer of form data to a PHP script
After the <i>submit</i> button has been pressed, the form data is transferred to the PHP script <i>showInput.php</i> and the result is then saved in the <i>index.html</i> in the div tag with the ID <i>showData</i>.

<b> Important: </b> please do not forget the command <i><b>event.preventDefault()</b> </i>!
___
<b>showInput.php</b>
```
<?php
  $user = $_GET['user'];
  $psw = $_GET['pwd'];
  echo "Input user: $user <br>";
  echo "Input passw: $psw"; 
?>
```
<br><br>
___
<b> Important: </b> please do not forget the command <i><b>event.preventDefault()</b> </i>!<br>
<b>index.html</b> 

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <form method="GET" action="showInput.php" id="form1">
        <label for="user"></label>
        <input type="text" name="user">
        <label for="pwd"></label>
        <input type="text" name="pwd">
        <input type="submit">
    </form>
    <div id="showData"></div>
   
    <script>
      document.getElementById("form1").addEventListener("submit",
        function(event){
            event.preventDefault();
            const FORM = document.getElementById("form1");
            const ACTION_SCRIPT = FORM.getAttribute("action");

            new lib.myAJAX(ACTION_SCRIPT,"showData", FORM);    
        })
</body>
</html>
```
The transmission method (POST / GET) is determined automatically by <i> lib.myAJAX () </i> from the form tag!
# Callback function
There is the possibility of the function <i> libmyAJAX () </i> to integrate a callback function, which has access to all information that the executed AJAX call has determined.
## Structure of the callback function
`functionsname(response, responseText, responseXML, responseURL, responseType)`
* <b>response:</b> property returns the response's body content as an ArrayBuffer, Blob, Document, JavaScript Object, or DOMString, depending on the value of the request's responseType property.
* <b>responseText:</b> get the response data as a string
* <b>responseXML:</b> get the response data as XML data
* <b>responseURL:</b>  property returns the serialized URL of the response or the empty string if the URL is null. If the URL is returned, any URL fragment present in the URL will be stripped away. The value of responseURL will be the final URL obtained after any redirects. 
* <b>responseType:</b>  is an enumerated string value specifying the type of data contained in the response.

## Example 3: receives JSON data from a PHP file.
___
<b>JSON.php</b>
```
<?php
    $ary=[
        ["name"=>"Otto", "adresse"=>["str"=>"Otto Braun Str.", "PLZ"=>"10409"]],
        ["name"=>"Torsten", "adresse"=>["str"=>"Müllerstr.", "PLZ"=>"13086"]],
        ["name"=>"Melly", "adresse"=>["str"=>"Thomas-Mann-Str.", "PLZ"=>"10551"]]
    ];
    echo json_encode($ary);
?>
```
<br><br>
___

<b>index.html</b> 

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <button id="btn">Data</button> 
    <div id="data"></div>
    <script>
      document.getElementById("btn").addEventListener("click",
        function(){
            new myAJAX("JSON.php",null, null, myCallback);    
        })
       
        function myCallback(res,resTxt,resXML,resURL,resType){
            arr=JSON.parse(res);
            myData = `Name:${arr[1]["name"]}<br> Location:${arr[1]["adresse"]["str"]} <br>`
            myData += `URL: ${resURL}`
            document.getElementById("data").innerHTML=myData;
        }
    </script>
</body>
</html>
```


# Deutsch

# Was ist myAJAX?
myAJAX ist eine Funktion mit dessen Hilfe Sie, auf eine einfache Weise, AJAX-Calls aufrufen können!

Sie können Dateien (.html, .php, .txt etc.) in den Scope einer aufgerufenen Webseite an einen definierten Punkt (ID) im DOM-Content (HTML) einbinden.


## Vorteile!
Bei der Verwendung dieser Funktion: 
* werden Dateien an einen definierten Platz (ID) eingebunden, 
* bei der gerufenen HTML-Dateien alle Javascript-Scripte ausgeführt,
* in der PHP-Dateien werden PHP/HTML und Javascript-Scripte ausgeführt,
* Das src-Attribut wird berücksichtigt. Die benannte Datei wird eingebunden und ausgeführt, z.B.: 

    `<script src="myScript.js"><script>` 
* Formulardaten werden mit "GET" oder "POST" beim Aufruf übermittelt,
* kann eine Callback-Funktion aufgerufen werden, die Zugriff auf alle wesentlichen Daten hat (siehe Unten)!<br><br><br><br>
# Installation
## für Testzwecke
Verlinken Sie den Script-Tag einfach wie folgt:  
```
<html>
<head>
    <script src="https://cdn.jsdelivr.net/npm/myajaxcall@1.0.22/myajax_bundle.min.js"></script>
</head>
<body>
</body>
</html>
```
## locale Installation
1. erstelle eine package.json

    `npm init`<br><br>
2. Installieren des Packages

    `npm i myajaxcall`<br><br>
3. Einbinden (z.B. in die index.html)

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
</body>
</html>
```
<br><br>
# Arbeitsweise und Beispiele
Die Funktion wird aufgerufen mit:

`lib.myAJAX(filename, id, form, callback)`

Nicht alle Parameter sind notwendig! Falls ein Parameter übersprungen werden muss, wird dieser mit <b>null</b> aufgefüllt. Beispiel:

`lib.myAJAX('JSON.php', null, null, myCallback)`<br><br>

# einfache Beispiele und Erklärungen<br><br>
## Beispiel 1: Filename und ID
Die <i>index.html</i> ruft die <i>test.html</i> und fügt deren Inhalt im h1-Tag an der Stelle des span-tag mit der ID <i>content</i> ein.
___
<b>test.html</b>
```
<i>Welt</i>
```
<br><br>
___
<b>index.html</b>

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <h1>Hallo <span id="content"></span>!</h1>

    <script>
        new lib.myAJAX("test.html","content");
    </script>
</body>
</html>
```
## Beispiel 2: Übergabe von Formulardaten an ein PHP-Script
Nach dem der Button <i>submit</i> gedrückt wurde, werden die Formulardaten an das PHP-Script <i>showInput.php</i> übergeben und anschließend in der <i>index.html</i> im div-Tag mit der ID <i>showData</i>, angezeigt.

<b>Wichtig:</b> bitte nicht das <i><b>event.preventDefault()</b></i> vergessen!
___
<b>showInput.php</b>
```
<?php
  $user = $_GET['user'];
  $psw = $_GET['pwd'];
  echo "Input user: $user <br>";
  echo "Input passw: $psw"; 
?>
```
<br><br>
___
bitte nicht das <i><b>event.preventDefault()</b></i> vergessen!<br>
<b>index.html</b> 

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <form method="GET" action="showInput.php" id="form1">
        <label for="user"></label>
        <input type="text" name="user">
        <label for="pwd"></label>
        <input type="text" name="pwd">
        <input type="submit">
    </form>
    <div id="showData"></div>
   
    <script>
      document.getElementById("form1").addEventListener("submit",
        function(event){
            event.preventDefault();
            const FORM = document.getElementById("form1");
            const ACTION_SCRIPT = FORM.getAttribute("action");

            new lib.myAJAX(ACTION_SCRIPT,"showData", FORM);    
        })
</body>
</html>
```

Die Übertragungsmethode (POST/GET) ermittelt <i>lib.myAJAX()</i> selbständig aus dem form-Tag!

# Callback-Function
Es besteht die Möglichkeit der Funktion <i>libmyAJAX()</i> eine Callback-Funktion zu übergeben, welche Zugriff hat auf alle Informationen, die der ausgeführte AJAX-Call ermittelt hat. 
## Aufbau der Callback-Funktion
`functionsname(response, responseText, responseXML, responseURL, responseType)`

* <b>response:</b> gibt den Hauptinhalt der Antwort als ArrayBuffer, Blob, Dokument, JavaScript-Objekt oder DOMString zurück, abhängig vom Wert der responseType-Eigenschaft der Anforderung.
* <b>responseText:</b> gibt den Wert als String zurück.
* <b>responseXML:</b> gibt den Wert als XML-Data zurück.
* <b>responseURL:</b> Die Eigenschaft gibt die serialisierte URL der Antwort oder die leere Zeichenfolge zurück, wenn die URL null ist. Wenn die URL zurückgegeben wird, wird jedes in der URL vorhandene URL-Fragment entfernt. Der Wert von responseURL ist die endgültige URL, die nach Weiterleitungen erhalten wird.
* <b>responseType:</b> ist eine Zeichenfolge, die den in der Antwort enthaltenen Datentyp angibt. 

## Beispiel 3: empfängt JSON-Daten von einer PHP-Datei.
___
<b>JSON.php</b>
```
<?php
    $ary=[
        ["name"=>"Otto", "adresse"=>["str"=>"Otto Braun Str.", "PLZ"=>"10409"]],
        ["name"=>"Torsten", "adresse"=>["str"=>"Müllerstr.", "PLZ"=>"13086"]],
        ["name"=>"Melly", "adresse"=>["str"=>"Thomas-Mann-Str.", "PLZ"=>"10551"]]
    ];
    echo json_encode($ary);
?>
```
<br><br>
___

<b>index.html</b> 

```
<html>
<head>
    <script src="node_modules/myajaxcall/myajax_bundle.js"></script>
</head>
<body>
    <button id="btn">Data</button> 
    <div id="data"></div>
    <script>
      document.getElementById("btn").addEventListener("click",
        function(){
            new myAJAX("JSON.php",null, null, myCallback);    
        })
       
        function myCallback(res,resTxt,resXML,resURL,resType){
            arr=JSON.parse(res);
            myData = `Name:${arr[1]["name"]}<br> Location:${arr[1]["adresse"]["str"]} <br>`
            myData += `URL: ${resURL}`
            document.getElementById("data").innerHTML=myData;
        }
    </script>
</body>
</html>

