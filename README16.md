###VIDEO POC: https://youtu.be/2N25n832O88  ###

**Stored XSS in i-Educar**

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:** /intranet/educar_deficiencia_cad.php?cod_deficiencia=

**Vulnerable Parameter:** “Deficiência ou transtorno” field (stored via /intranet/educar_deficiencia_lst.php)

🔧 PoC Step-by-Step

1 - Authentication:
Log in to i-Educar with valid credentials.

2 - Access the "Pessoas" module:
Navigate to:
Pessoas > Cadastro > Tipo > Tipo de deficiencia e transtorno
URL: /intranet/educar_deficiencia_lst.php

![image](https://github.com/user-attachments/assets/2a1030c9-e060-41ef-8cfb-a2c83b68518b)

3 - Create or Edit "Deficiencia e Transtorno" Entry:
Either create a new "Deficiencia e Transtorno" or edit an existing one.

![image](https://github.com/user-attachments/assets/376eb463-2608-4de4-a934-84db14412935)
![image](https://github.com/user-attachments/assets/8a679cf0-6989-40f9-addd-513cd5bc6ea5)

4  - Edit Vulnerable Field "Deficiencia ou Transtorno":
Go to:
/intranet/educar_deficiencia_cad.php?cod_deficiencia=ID

![image](https://github.com/user-attachments/assets/6d07d5bb-a78e-4f7f-9cae-6ab08549b7a7)

5 - Insert Payload:
In the “Deficiencia ou Transtorno” field, insert:

<script>alert('PoC VulDB i-Educar Pacxxx')</script>
![image](https://github.com/user-attachments/assets/f152d9eb-37e7-496c-80e9-570d3f594339)


6 - Save and Trigger:
![image](https://github.com/user-attachments/assets/11929f8b-5469-4b5f-b4d2-6502610e9d04)


⚠️ Recommendations & Mitigations

Input Sanitization: Reject or neutralize input containing scripts or HTML.

Output Encoding: Properly encode all user input before rendering in HTML.
