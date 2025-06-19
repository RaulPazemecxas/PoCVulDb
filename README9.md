VIDEO POC: https://youtu.be/FhPQLGorbqA

📄 Proof of Concept (PoC) for Stored XSS in Student Result Management System 1.0 (SRMS 1.0)

Vulnerability Type: Stored Cross-Site Scripting (XSS)
Affected Application: Student Result Management System 1.0 (SRMS 1.0)
Vulnerable Endpoint: /script/admin/system
Vulnerable Parameter: School Name
Impact: Persistent execution of arbitrary JavaScript in the application context, including for unauthenticated users

🔧 Steps to Reproduce (PoC):

1 - Log in to the SRMS 1.0 application using an administrator account.

![image](https://github.com/user-attachments/assets/8ab669e5-702d-4def-bfdc-e2917de6b793)

2 - Navigate to /srms/script/admin/system.

![image](https://github.com/user-attachments/assets/34910412-bf5c-4cd1-95de-fb534b43f560)

3 - In the School Name input field, insert the following payload:

![image](https://github.com/user-attachments/assets/58896899-5e19-4542-ae1d-1ddff4f182ec)

<script>alert('PoC VulDB SRMS')</script>

Save the changes.

4 - Visit the root path /srms/script/. The injected JavaScript payload will automatically execute.

![image](https://github.com/user-attachments/assets/45a97d17-5e25-4eac-9420-79537f0dac6d)


5 - Since this endpoint is accessible without authentication, the XSS is triggered for any user accessing the page, increasing the potential attack surface and confirming the presence of a high-impact Stored XSS vulnerability.

![image](https://github.com/user-attachments/assets/94b1931d-232e-46a2-9b76-bbe773c3d4e4)


