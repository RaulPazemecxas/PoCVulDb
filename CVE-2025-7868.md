##### VIDEO POC: https://youtu.be/RtXMxNLuAx8  #####

**Stored XSS — i-Educar Calendario Module**

**Vulnerability Type: Stored Cross-Site Scripting (XSS)**

**Affected Application: i-Educar**

**Module: Calendario (/intranet/educar_calendario_dia_motivo_cad.php?cod_calendario_dia_motivo=ID)**

**Vulnerable Field: Motivo (<span class="form">Motivo</span>)**

**🧪 Proof of Concept (PoC) Steps**
1. Log in
Authenticate to the i-Educar platform using valid credentials.

2. Go to "Tipos de evento do calendário"
Access the calendário via:
Escola > Cadastro > Tipo > Calendário

![image](https://github.com/user-attachments/assets/d3104c19-102b-4c1c-bd42-6512f9d7d7ea)

/intranet/educar_calendario_dia_motivo_lst.php

4. Edit or Create an "Calendário Dia Motivo - Listagem"

![image](https://github.com/user-attachments/assets/e4aac95e-179f-4d3f-b43a-03317542812a)
![image](https://github.com/user-attachments/assets/df6f35cf-51ac-49ab-ac28-f788b5626531)

Insert the XSS payload in the "Motivo" (nm_motivo) field:

<script>alert('PoC VulDB i-Educar PaCXXX')</script>

4. Save the Appointment
Click "Salvar".
![image](https://github.com/user-attachments/assets/c343ecc1-e287-44de-ab53-df628194e23d)

5. Trigger the Payload
Reopen the page — the script will execute.

![image](https://github.com/user-attachments/assets/550153c8-e571-4ec2-a71a-ae7d3fde5489)


### 🔥 Impact ###
Any user with access to the agenda module will have the malicious script executed in their browser context, potentially allowing:
Session hijacking
Redirection to malicious sites
Credential theft
Browser exploitation via further scripts
