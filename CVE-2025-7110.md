##VIDEO POC: https://youtu.be/N3pu_GJHjCw ##
**Stored XSS in i-Educar (School Module)**

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:** /intranet/educar_escola_lst.php

**Vulnerable Parameter:** “Escola” field (stored via /intranet/educar_escola_det.php)

🔧 PoC Step-by-Step

1 - Authentication:
Log in to i-Educar with valid credentials.

2 - Access the School Module:
Navigate to:
Cadastro > Escola
URL: /intranet/educar_escola_lst.php

![image](https://github.com/user-attachments/assets/7fed51b8-54b7-47dc-8b7f-9fa48e72d082)


3 - Create or Edit School Entry:
Either create a new school or edit an existing one.
![image](https://github.com/user-attachments/assets/4834851c-39ca-4df5-aa40-b729b7ba0da7)

4  - Edit Vulnerable Field:
Go to:
/intranet/educar_escola_det.php?cod_escola=ID
Click on Edit.
![image](https://github.com/user-attachments/assets/047d06f6-fb6c-4c72-8ef5-88ff017e990b)

5 - Insert Payload:
In the “Escola” field, insert:

<script>alert('PoC VulDB i-Educar Pacxxx')</script>
Save and Trigger:
![image](https://github.com/user-attachments/assets/0444bb5d-4035-410e-af05-9eb534a9c296)

6 - After saving, the script will execute automatically every time the school list page is loaded.
![image](https://github.com/user-attachments/assets/6d188322-bbc2-4c3b-8018-8677e012c75c)

⚠️ Recommendations & Mitigations

Input Sanitization: Reject or neutralize input containing scripts or HTML.

Output Encoding: Properly encode all user input before rendering in HTML.

Use of XSS Mitigation Libraries: Tools like OWASP Java Encoder, HTMLPurifier, or DOMPurify should be employed.

