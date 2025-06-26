#VIDEO POC: https://youtu.be/Pe33X_zm_TQ #

##Stored XSS in i-Educar##

**Vulnerability Type:** Stored Cross-Site Scripting (XSS)

**Affected Application:** i-Educar

**Vulnerable Endpoint:**  /intranet/educar_aluno_beneficio_cad.php

**Vulnerable Parameter:** "Benefício" field (stored via /intranet/educar_aluno_beneficio_cad.php)

**Impact:** Arbitrary JavaScript execution in the browser of authenticated users. This can lead to session hijacking, malicious redirection, UI defacement, or other harmful actions. The script persists and is triggered every time the benefit listing page is loaded.

🔧 PoC Step-by-Step

1 - Authentication:
Log into the i-Educar platform with valid credentials.

2 - Access the Module:
Navigate to:
Cadastro > Tipo > Aluno > Tipo de benefício

![image](https://github.com/user-attachments/assets/cde8eeb9-7ce9-4549-9711-8650f8480381)

3 - Create New Benefit Entry:
Click on the "Novo" button at:
/intranet/educar_aluno_beneficio_lst.php

![image](https://github.com/user-attachments/assets/90e0f458-5cca-42ff-bf5a-f90410569a1f)

4 - Insert Payload:
In the “Benefício” field at
/intranet/educar_aluno_beneficio_cad.php,
insert the following payload:

<script>alert('PoC VulDB PacXXX')</script>

![image](https://github.com/user-attachments/assets/0b51d953-01c7-403d-86ad-c6230083c816)

![image](https://github.com/user-attachments/assets/e9429ef7-ad63-4db6-bf03-bae23ca96894)

5 - Automatic Execution:
Every time the listing page (educar_aluno_beneficio_lst.php) is loaded, the injected JavaScript will execute automatically.

![image](https://github.com/user-attachments/assets/cb6692d4-866d-4539-a29a-20e614dff202)


⚠️ Recommendations & Mitigations

Input Sanitization: Strip HTML/script tags and escape special characters in all input fields.
HTML Encoding: Ensure all dynamic content is safely encoded when rendered on the page.
Use of XSS Protection Libraries: Leverage libraries like OWASP Java Encoder or DOMPurify to filter untrusted data.

