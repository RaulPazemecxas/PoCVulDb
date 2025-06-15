VIDEO POC: https://youtu.be/7P5YT5MwCjg

#📄 PoC for Exploitation: Stored XSS in WeGIA 3.4.0

#Vulnerability Type: Stored Cross-Site Scripting (XSS)

#Affected Application: WeGIA 3.4.0

#Vulnerability Location:/html/matPat/adicionar_tipoSaida.php

#Impact: Persistent execution of arbitrary JavaScript code in the context of the application

PoC for exploitation stored XSS in WeGIA


1 - Log in to the platform.:

![image](https://github.com/user-attachments/assets/76cecfdf-459b-46e0-ba6f-eb770523416f)
![image](https://github.com/user-attachments/assets/cea6f28e-44cd-43df-8e81-917bd8c82b71)

2 - Go to the section "Material e Patrimonio > Entrada > Registrar Saida":

![image](https://github.com/user-attachments/assets/4ef502d9-906a-4a92-8d2a-57bdb3b0a711)


3 - On the page /html/matPat/cadastro_saida.php, click the "+" button under the "Tipo" tab.

![image](https://github.com/user-attachments/assets/b9501a56-763f-4242-a5fa-f1900fdb6022)


4 - On the page /html/matPat/adicionar_tipoSaida.php, register a new unit using the following XSS payload:

<script>alert('Poc VulDB')</script>
Then, click the first "Enviar" button to submit the form.

![image](https://github.com/user-attachments/assets/125aec50-ef0e-41fc-97da-94fea009595d)


5 - The payload will be stored in the system and will be executed every time the page html/matPat/cadastro_saida.php is loaded, confirming the presence of a Stored Cross-Site Scripting (XSS) vulnerability.

![image](https://github.com/user-attachments/assets/aded2182-305a-474f-a3ba-80259d1216d1)
