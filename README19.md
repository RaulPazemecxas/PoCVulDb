### VIDEO POC: https://youtu.be/RtXMxNLuAx8

Stored XSS — i-Educar Turma Module

Vulnerability Type: Stored Cross-Site Scripting (XSS)

Affected Application: i-Educar

Module: Turma (intranet/educar_turma_tipo_det.php?cod_turma_tipo=ID)

Vulnerable Field: Turma Tipo (nm_tipo)

🧪 Proof of Concept (PoC) Steps

1 - Log in Authenticate to the i-Educar platform using valid credentials.

2 - Go to " Início / Escola / Editar tipo de turma" Access the Turma via: Escola > Cadastro > Tipo > Turma > Tipo de Turma

/intranet/educar_turma_tipo_lst.php

3 - Edit or Create an "Turma Tipo"
![image](https://github.com/user-attachments/assets/017816e6-7151-4870-9a4c-55908657249d)

Insert the XSS payload in the "Turma Tipo" (nm_tipo) field:

<script>alert('PoC VulDB i-Educar PaCXXX')</script>
![image](https://github.com/user-attachments/assets/49a616ec-4e65-4e2d-acae-724b0161a0b4)

4 - Click "Salvar"
![image](https://github.com/user-attachments/assets/577a336f-7bd6-4df8-ac2e-a85fe8832140)

5 - Trigger the Payload Reopen the page — the script will execute.

![image](https://github.com/user-attachments/assets/9129110b-5bd1-4d97-b6cc-14c84e876792)


🔥 Impact
Any user with access to the agenda module will have the malicious script executed in their browser context, potentially allowing: 
Session hijacking 
Redirection to malicious sites 
Credential theft 
Browser exploitation via further scripts





