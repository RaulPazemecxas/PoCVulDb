###VIDEO POC: https://youtu.be/Dd4RdfomMms  ###

**Stored XSS in i-Educar**

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:** /module/ComponenteCurricular/edit?id=ID

**Vulnerable Parameter:** “Nome” field (stored via /intranet/educar_componente_curricular_lst.php)

🔧 PoC Step-by-Step

1 - Authentication:
Log in to i-Educar with valid credentials.

2 - Access the "Escola" module:
Navigate to:
Escola > Cadastro > Componentes Curriculares
URL: /intranet/educar_componente_curricular_lst.php

![image](https://github.com/user-attachments/assets/e83cc5b2-fc93-46a0-a0c0-60ea43cee924)


3 - Create or Edit "Componentes curriculares" Entry:
Either create a new "Componentes curriculares" or edit an existing one.

![image](https://github.com/user-attachments/assets/12dd4215-c4b6-4dd3-a77f-4d15156c3163)
![image](https://github.com/user-attachments/assets/8e32e3d8-752c-430b-963e-88b656d4996f)


4  - Edit Vulnerable Field "Nome":
Go to:
module/ComponenteCurricular/edit?id=ID
![image](https://github.com/user-attachments/assets/fc5db1b8-0e3e-4f0a-a097-1fe70d590c63)


5 - Insert Payload:
In the “Nome” field, insert:

<script>alert('PoC VulDB i-Educar Pacxxx')</script>
![image](https://github.com/user-attachments/assets/cbdc6b0b-c779-4c7e-9af8-00fae1455717)


6 - Save and Trigger:
![image](https://github.com/user-attachments/assets/ea3118c1-286d-446a-a18f-f7a84fc7fc37)


⚠️ Recommendations & Mitigations

Input Sanitization: Reject or neutralize input containing scripts or HTML.

Output Encoding: Properly encode all user input before rendering in HTML.

Use of XSS Mitigation Libraries: Tools like OWASP Java Encoder, HTMLPurifier, or DOMPurify should be employed.
