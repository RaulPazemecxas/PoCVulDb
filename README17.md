### VIDEO POC:  https://youtu.be/dOwcn_k2iTE ###

**Stored XSS — i-Educar Agenda Module**
**Vulnerability Type: Stored Cross-Site Scripting (XSS)**
**Affected Application: i-Educar**
**Module: Agenda (/intranet/agenda.php)**
**Vulnerable Field: agenda_rap_titulo (title of the appointment)**

**🧪 Proof of Concept (PoC) Steps**
1. Log in
Authenticate to the i-Educar platform using valid credentials.

2. Go to "Agenda"
Access the agenda via:
![image](https://github.com/user-attachments/assets/6d01e8c6-6827-4147-8f2b-71be48f91b20)

/intranet/agenda.php

4. Edit or Create an Appointment
![image](https://github.com/user-attachments/assets/2df3daa1-65fb-4233-b94a-ea410f95d505)

Insert the XSS payload in the "Título" (novo_titulo) field:

<script>alert('PoC VulDB i-Educar PaCXXX')</script>
![image](https://github.com/user-attachments/assets/cce03bb2-15b4-4962-b0c2-21292b5aadba)
![image](https://github.com/user-attachments/assets/731d28c5-aa39-46c9-94dd-63bcb13303e9)

4. Save the Appointment
Click "Salvar".
![image](https://github.com/user-attachments/assets/17c3df44-369c-4a66-bbc4-5a093b87a996)

5. Trigger the Payload
Reopen the agenda — the script will execute.
![image](https://github.com/user-attachments/assets/b5f17e49-f6e0-4cea-a56e-e153e424afb4)



### 🔥 Impact ###
Any user with access to the agenda module will have the malicious script executed in their browser context, potentially allowing:
Session hijacking
Redirection to malicious sites
Credential theft
Browser exploitation via further scripts















![image](https://github.com/user-attachments/assets/07995e43-775a-4211-8023-78e9732de2fd)
![image](https://github.com/user-attachments/assets/ace8a51e-e71e-4635-94c5-0e1ec2f68319)
![image](https://github.com/user-attachments/assets/7efc5562-9700-4ea7-8956-17c57f5f6ba2)
![image](https://github.com/user-attachments/assets/26e2a811-a86f-469b-ab50-9e6e0e14b541)

