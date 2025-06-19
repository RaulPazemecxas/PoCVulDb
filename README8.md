VIDEO POC: https://youtu.be/rBtUzvmoIxc

📄 Proof of Concept (PoC) for Stored XSS in Student Result Management System 1.0 (SRMS 1.0)

Vulnerability Type: Stored Cross-Site Scripting (XSS)

Affected Application: Student Result Management System 1.0 (SRMS 1.0)

Vulnerable Endpoint: /srms/script/admin/students

Vulnerable Parameter: First Name

Impact: Persistent execution of arbitrary JavaScript in the application context

🔧 Steps to Reproduce (PoC):

1 - Log in to the SRMS 1.0 application with valid administrative credentials.

![image](https://github.com/user-attachments/assets/cc7de1e7-28b8-4208-be0f-509705d1d4f8)

2 - Navigate to: Students > Manage Students > /srms/script/admin/manage_students, then select a term or session.

![image](https://github.com/user-attachments/assets/6ca692ab-28b7-428f-b3fb-aa9b00630ec3)

3 - On the page /srms/script/admin/students, click the Edit button for an existing student record.

![image](https://github.com/user-attachments/assets/2a286bf0-e426-4060-b81f-bc2c4c50e5b6)

4 - In the First Name field, inject the following XSS payload:

![image](https://github.com/user-attachments/assets/b8e48672-2b7d-43ca-bca1-42b77b6c7fa6)
![image](https://github.com/user-attachments/assets/0f94fb9c-9ab2-41fd-b4c4-f81e775c2031)

<script>alert('PoC VulDB SRMS')</script>

Save the changes.

5 - Whenever the /srms/script/admin/students page is accessed, the injected JavaScript will execute in the browser context, confirming a Stored Cross-Site Scripting (XSS) vulnerability.

![image](https://github.com/user-attachments/assets/13fc9140-aca2-4af9-afae-e766bc4f68e5)
![image](https://github.com/user-attachments/assets/9be0cc54-ec67-403a-8d81-e89f80ba4f4c)
