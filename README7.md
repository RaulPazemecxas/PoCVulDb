VIDEO POC: 

📄 PoC for Exploitation: Stored XSS in WeGIA 3.4.0
Vulnerability Type: Stored Cross-Site Scripting (XSS)

Affected Application: WeGIA 3.4.0

Vulnerable Endpoint: /html/funcionario/cadastro_funcionario.php?cpf=CPF

Vulnerable Parameters: Nome and Sobrenome

Impact: Persistent execution of arbitrary JavaScript code in the context of the application

🔧 Step-by-step PoC for Stored XSS in WeGIA

1 - Log in to the platform.

![image](https://github.com/user-attachments/assets/64fd1586-9787-4d70-a9a2-2ada710fc98a)
![image](https://github.com/user-attachments/assets/b9839e9f-e4ef-43b4-8291-dade94ffcf67)

Access the application with valid credentials.

2 - Go to the page Pessoas > Funcionarios > Cadastrar Funcionario in the /html/funcionario/pre_cadastro_funcionario.php page. Insert a valid CPF.

![image](https://github.com/user-attachments/assets/50b25679-4add-4318-8a5b-2a8e165f9e2b)
![image](https://github.com/user-attachments/assets/ef3cad6b-4b5b-4eed-93ae-4a14ccebbf44)

3 - Go to the page html/funcionario/cadastro_funcionario.php?cpf={CPF} and insert the following payload in the "Nome" and "Sobrenome" fields, then click "Enviar":

<script>alert('Poc VulDB Teste cadastro_funcionario')</script>

![image](https://github.com/user-attachments/assets/89b804cc-82b1-4707-a096-a9e3a6eeea60)
![image](https://github.com/user-attachments/assets/6384a2b4-d019-4e98-87ea-e18b5c1098f3)


4 - Go to “Memorando” > “Criar Memorando” and register a new Memorando 'test' (the page is /html/memorando/novo_memorandoo.php) and click in "Criar Memorando" buttom.

![image](https://github.com/user-attachments/assets/c5246c40-db50-41d7-b548-02d7bd716253)

This section loads the stored data of the registered in html/funcionario/cadastro_funcionario.php?cpf={CPF}.

5 - The JavaScript payload will be executed every time the page /html/memorando/insere_despacho.php?id_memorando=ID&msg=success&sccs=Memorando%20criado%20com%20sucesso is accessed, confirming that the application is vulnerable to Stored XSS.

![image](https://github.com/user-attachments/assets/bd44fb3e-d179-4302-8338-d17acb953358)
