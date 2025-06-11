# VIDEO POC: https://www.youtube.com/watch?v=X7DJmOtNqxU
#📄 PoC for Exploitation: Stored XSS in WeGIA **3.4.0**
#Vulnerability Type: Stored Cross-Site Scripting (XSS)
#Affected Application: **WeGIA 3.4.0**
#Vulnerability Location: /html/matPat/adicionar_unidade.php
#Impact: Persistent execution of arbitrary JavaScript code in the context of the application


PoC for exploitation stored XSS in WeGIA

1 - Login in platform:

![image](https://github.com/user-attachments/assets/64fd1586-9787-4d70-a9a2-2ada710fc98a)
![image](https://github.com/user-attachments/assets/b9839e9f-e4ef-43b4-8291-dade94ffcf67)


2 - Vá para a seção "Material e Patrimonio > Entrada > Registrar Entrada":
![image](https://github.com/user-attachments/assets/553b4364-b14d-4edd-871f-75ffb84172b7)
![image](https://github.com/user-attachments/assets/1f91f1ef-c5c1-4d0e-8dc1-332e64882a5e)

3 - Na seção /html/matPat/cadastro_entrada.php, clique no "+" da aba Produto

![image](https://github.com/user-attachments/assets/50e0aa4e-52ee-4bad-b34a-68352c64e2fe)

4 - Na seção /html/matPat/cadastro_produto.php, clique no "+" da aba Unidade 

![image](https://github.com/user-attachments/assets/5a8dcfdf-448b-432b-825f-e1b842974b1d)

5 - Na seção /html/matPat/adicionar_unidade.php, cadastre uma unidade com o payload XSS, <script>alert('Poc VulDB')</script> e clique no primeiro botão "enviar"

![image](https://github.com/user-attachments/assets/ae94694a-e110-4554-81ae-dbbea852bc03)
![image](https://github.com/user-attachments/assets/456c411a-ffe9-41fd-885e-19341d61eb12)

6 - o payload vai ficar armazenado e será carregado sempre que a seção /html/matPat/cadastro_produto.php for carregada, caracterizando um Stored Cross Site Scripting.

![image](https://github.com/user-attachments/assets/94093c93-ce08-4c36-97a4-27a2e7a273b1)



