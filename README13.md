###VIDEO POC:  ###
**Stored XSS in i-Educar**

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:** /intranet/educar_curso_det.php?cod_curso=ID

**Vulnerable Parameter:** “Curso” field (stored via /intranet/educar_curso_lst.php?busca=)

🔧 PoC Step-by-Step

1 - Authentication:
Log in to i-Educar with valid credentials.

2 - Access the "Curso" module:
Navigate to:
Cadastro > Curso
URL: /intranet/educar_curso_lst.php?busca=S

![image](https://github.com/user-attachments/assets/1d83b270-1975-44eb-845a-0fd33e8742a5)


3 - Create or Edit "Curso" Entry:
Either create a new "Curso" or edit an existing one.
![image](https://github.com/user-attachments/assets/28021473-bdac-4135-8dfe-09992afbb861)


4  - Edit Vulnerable Field:
Go to:
/intranet/educar_curso_cad.php?cod_curso=ID

5 - Insert Payload:
In the “Curso” field, insert:

<script>alert('PoC VulDB i-Educar Pacxxx')</script>
Save and Trigger:

![image](https://github.com/user-attachments/assets/e8dd9133-aa06-46d1-8db5-ce544d8c0589)
![image](https://github.com/user-attachments/assets/b9192971-dd94-4f8e-8640-6b6c6550f0e9)


⚠️ Recommendations & Mitigations

Input Sanitization: Reject or neutralize input containing scripts or HTML.

Output Encoding: Properly encode all user input before rendering in HTML.

Use of XSS Mitigation Libraries: Tools like OWASP Java Encoder, HTMLPurifier, or DOMPurify should be employed.
