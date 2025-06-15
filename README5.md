VIDEO POC: https://youtu.be/BRqtS1octSQ

#📄 PoC for Exploitation: Stored XSS in WeGIA 3.4.0

#Vulnerability Type: Stored Cross-Site Scripting (XSS)

#Affected Application: WeGIA 3.4.0

#Vulnerability Location: /html/matPat/adicionar_tipoEntrada.php

#Impact: Persistent execution of arbitrary JavaScript code in the context of the application

PoC for exploitation stored XSS in WeGIA


1 - Log in to the platform.:

![image](https://github.com/user-attachments/assets/76cecfdf-459b-46e0-ba6f-eb770523416f)
![image](https://github.com/user-attachments/assets/cea6f28e-44cd-43df-8e81-917bd8c82b71)

2 - Go to the section "Material e Patrimonio > Entrada > Registrar Entrada":

![image](https://github.com/user-attachments/assets/1ff05216-feaf-4b55-a2c7-023a411f5672)
![image](https://github.com/user-attachments/assets/f04358a6-4318-477e-856b-251678c1f6ae)

3 - On the page /html/matPat/cadastro_produto.php, click the "+" button under the "Tipo" tab.

![image](https://github.com/user-attachments/assets/05620740-cc79-4a4c-8274-d712f52e22e5)

4 - On the page /html/matPat/adicionar_tipoEntrada.php, register a new unit using the following XSS payload:

<script>alert('Poc VulDB')</script>
Then, click the first "Enviar" button to submit the form.

![image](https://github.com/user-attachments/assets/bff90fa2-91ed-44f6-8209-a03a34e1a44a)

5 - The payload will be stored in the system and will be executed every time the page /html/matPat/cadastro_entrada.php is loaded, confirming the presence of a Stored Cross-Site Scripting (XSS) vulnerability.

![image](https://github.com/user-attachments/assets/3910b7a5-b25c-4ba5-ad58-67cff686469b)
