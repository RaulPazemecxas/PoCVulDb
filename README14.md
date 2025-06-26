###VIDEO POC: https://youtu.be/R6vJIZnjdmE  ###

**Stored XSS in i-Educar**

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:** /intranet/educar_funcao_det.php?cod_funcao=COD&ref_cod_instituicao=COD

**Vulnerable Parameter:** “Funcão” field (stored via /intranet/educar_funcao_lst.php)

🔧 PoC Step-by-Step

1 - Authentication:
Log in to i-Educar with valid credentials.

2 - Access the "Servidores" module:
Navigate to:
Servidores > Cadastro > Tipos > Funções
URL: /intranet/educar_funcao_lst.php

![image](https://github.com/user-attachments/assets/1329b390-963d-4dc5-b373-0ec9bf0843da)


3 - Create or Edit "Função" Entry:
Either create a new "Função" or edit an existing one.

![image](https://github.com/user-attachments/assets/dc57f3c5-3736-4a21-a707-92ef4cd12ad5)


4  - Edit Vulnerable Field "Função":
Go to:
/intranet/educar_funcao_cad.php?cod_funcao=COD

![image](https://github.com/user-attachments/assets/1a25a98f-f9ef-4ad8-96dc-c70080b747b2)


5 - Insert Payload:
In the “Função” field, insert:

<script>alert('PoC VulDB i-Educar Pacxxx')</script>
![image](https://github.com/user-attachments/assets/9a0156c5-8613-4b0f-acbc-ffdea5a06cf0)


6 - Save and Trigger:

![image](https://github.com/user-attachments/assets/2fe4f80d-d859-4b33-be16-6b70cac8ef9c)
![image](https://github.com/user-attachments/assets/16c9f422-1dbf-4618-842d-09245e6881de)


⚠️ Recommendations & Mitigations

Input Sanitization: Reject or neutralize input containing scripts or HTML.

Output Encoding: Properly encode all user input before rendering in HTML.

Use of XSS Mitigation Libraries: Tools like OWASP Java Encoder, HTMLPurifier, or DOMPurify should be employed.
