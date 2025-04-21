# PoCVulDb
# VIDEO POC: https://www.youtube.com/watch?v=VSlHH0Ecfp8

PoC for exploitation XSS in plataform Sabio Virtual

1 - Register for an account on the platform.

![image](https://github.com/user-attachments/assets/bebeb485-ffe2-41a6-9571-843440fbf1bb)

![image](https://github.com/user-attachments/assets/dbad2a18-5c9b-41e9-a6fd-f0398c3ce592)

![image](https://github.com/user-attachments/assets/6156359d-6131-4562-82cc-40816954db06)


2 - Go to “Knowledge Base” (BASE DE CONHECIMENTO) from the sidebar menu (magnifying glass icon).

![image](https://github.com/user-attachments/assets/c0940b68-8f25-4ed8-94d2-35aaeb82d219)

3 - Click “New Article” ("Novo Artigo").

![image](https://github.com/user-attachments/assets/f15ccfe2-9386-4e36-87ed-34b7adf411aa)

4 - In both the title and content fields, insert the following payload:

<script>alert('Stored XSS POC VulDB')</script>

![image](https://github.com/user-attachments/assets/622c1767-05a0-439e-ad44-a1e0546e992e)

5 - Click “Save”.

![image](https://github.com/user-attachments/assets/0bdb1290-2210-4664-ba3d-d0349066237f)

![image](https://github.com/user-attachments/assets/30d53aa9-2f15-49b7-ba92-f66f9684b359)

6 - Navigate to “Records > Requests > Subjects Attended” (CADASTROS>CHAMADOS>ASSUNTOS ATENDIDOS).

![image](https://github.com/user-attachments/assets/9b062955-015f-4132-ae39-9a238a60d260)

![image](https://github.com/user-attachments/assets/eb53bc9c-32e8-4b1b-bf4a-c7f7b0051d19)

7 - The payload will execute automatically

![image](https://github.com/user-attachments/assets/886057a8-45b8-4be3-a944-48af4f00b88c)

