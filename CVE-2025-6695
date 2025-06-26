VIDEO POC: https://youtu.be/VZs4hmHYaXQ

#📄 PoC for Exploitation: Stored XSS in WeGIA 3.4.0

#Vulnerability Type: Stored Cross-Site Scripting (XSS)

#Affected Application: WeGIA 3.4.0

#Vulnerability Location: /html/matPat/adicionar_categoria.php

#Impact: Persistent execution of arbitrary JavaScript code in the context of the application

PoC for exploitation stored XSS in WeGIA


1 - Log in to the platform.:

![image](https://github.com/user-attachments/assets/76cecfdf-459b-46e0-ba6f-eb770523416f)
![image](https://github.com/user-attachments/assets/cea6f28e-44cd-43df-8e81-917bd8c82b71)

2 - Go to the section "Material e Patrimonio > Entrada > Registrar Entrada":

![image](https://github.com/user-attachments/assets/1ff05216-feaf-4b55-a2c7-023a411f5672)
![image](https://github.com/user-attachments/assets/f04358a6-4318-477e-856b-251678c1f6ae)

3 - On the page /html/matPat/cadastro_entrada.php, click the "+" button under the "Produto" tab.

![image](https://github.com/user-attachments/assets/10afc996-7d9b-4f47-8f47-4bd3f88ffe5b)

4 - On the page /html/matPat/cadastro_produto.php, click the "+" button under the "Categoria" tab.

![image](https://github.com/user-attachments/assets/72f85f07-fc52-43b2-91b4-0c57b5de6a08)

5 - On the page /html/matPat/adicionar_categoria.php, register a new unit using the following XSS payload:

<script>alert('Poc VulDB')</script>
Then, click the first "Enviar" button to submit the form.

![image](https://github.com/user-attachments/assets/f17bd916-e809-4ae7-91e3-9f97422b4244)

6 - The payload will be stored in the system and will be executed every time the page /html/matPat/cadastro_produto.php is loaded, confirming the presence of a Stored Cross-Site Scripting (XSS) vulnerability.

![image](https://github.com/user-attachments/assets/03776c16-a3f3-4b05-9df5-0f45610c6d5b)

