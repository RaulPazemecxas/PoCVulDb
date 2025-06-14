VIDEO POC: https://youtu.be/BCqqmDk0pH8

📄 PoC for Exploitation: Stored XSS in WeGIA 3.4.0
Vulnerability Type: Stored Cross-Site Scripting (XSS)

Affected Application: WeGIA 3.4.0

Vulnerable Endpoint: /html/atendido/Cadastro_Atendido.php?cpf={CPF}

Vulnerable Parameters: Nome and Sobrenome

Impact: Persistent execution of arbitrary JavaScript code in the context of the application

🔧 Step-by-step PoC for Stored XSS in WeGIA
1 - Log in to the platform.

![image](https://github.com/user-attachments/assets/64fd1586-9787-4d70-a9a2-2ada710fc98a)
![image](https://github.com/user-attachments/assets/b9839e9f-e4ef-43b4-8291-dade94ffcf67)

Access the application with valid credentials.

2 - Go to the page /html/atendido/pre_cadastro_atendido.php and register a new user.
Insert a valid CPF (you can use an online Brazilian CPF generator) and proceed to pre-register the user.

![image](https://github.com/user-attachments/assets/4a5ce371-cbbd-41e3-8e2e-790a88ca4333)
![image](https://github.com/user-attachments/assets/303e8f1a-5b02-4f61-b220-881ba00b9478)

3 - Go to the page /html/atendido/Cadastro_Atendido.php?cpf={CPF} and insert the following payload in the "Nome" and "Sobrenome" fields, then click "Enviar":

<script>alert('Poc VulDBeeee')</script>
![image](https://github.com/user-attachments/assets/5bbca0e7-bf6f-4018-bebd-07de2051a6a6)
![image](https://github.com/user-attachments/assets/84adf54c-4550-466a-bc3b-1c7460343df4)
![image](https://github.com/user-attachments/assets/edd43c56-972e-4daf-864c-308a2c474c26)


4 - The HTTP request, if intercepted using a proxy such as Burp Suite, will look like this:

**#GET /WeGIA/controle/control.php?nome=%3Cscript%3Ealert%28%27PoC+VulDB%27%29%3C%2Fscript%3E&sobrenome=%3Cscript%3Ealert%28%27PoC+VulDB%27%29%3C%2Fscript%3E&sexo=m&telefone=%2899%2927199-81&nascimento=2000-10-10&intStatus=3&intTipo=1&nomeClasse=AtendidoControle&cpf=040.550.340-75&metodo=incluir HTTP/1.1**
**#Host: sec.wegia.org:8000**
**#Cookie: PHPSESSID=...**
**#User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)**
**#Referer: https://sec.wegia.org:8000/WeGIA/html/atendido/Cadastro_Atendido.php?cpf=CPF**

5 - Go to “Pessoas” > “Atendidos” > “Cadastrar ocorrência”.

![image](https://github.com/user-attachments/assets/a5696a46-f608-40ba-a9ae-9817b7a61921)

This section loads the stored data of the registered person.

6 - The JavaScript payload will be executed every time the page /html/atendido/cadastro_ocorrencia.php is accessed, confirming that the application is vulnerable to Stored XSS.

![image](https://github.com/user-attachments/assets/24de9736-66ad-42ec-97bd-01dd8d097d6f)
