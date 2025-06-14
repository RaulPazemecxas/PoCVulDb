# VIDEO POC: https://youtu.be/jaXQWdMOVek

#📄 PoC for Exploitation: Stored XSS Automad flat-file CMS **2.0.0-alpha.37**

#Vulnerability Type: Stored Cross-Site Scripting (XSS)

#Affected Application: **Automad flat-file CMS**

#Vulnerability Location: 

#Impact: Persistent execution of arbitrary JavaScript code in the context of the application

Step by Step

1 - Access the Automad CMS dashboard at:

/dashboard/page
![image](https://github.com/user-attachments/assets/7f8c8993-7273-45fd-b877-07d20c574b30)
![image](https://github.com/user-attachments/assets/50290977-ea36-4b30-87e8-5a5c4158b936)


2 - Go to "General Data and Files"
![image](https://github.com/user-attachments/assets/b1d9a100-8989-472a-9691-a5f44ea42673)
![image](https://github.com/user-attachments/assets/507052d7-f705-4815-82a9-095d6bd96972)

3 -Inject the payload in the Brand field:

![image](https://github.com/user-attachments/assets/a4e10370-3487-40bc-a8cd-9954e3ae6add)

<script>alert('PoC VulDB Automad CMS')</script>

4 - Click "Publish"

![image](https://github.com/user-attachments/assets/f03e3ebc-1073-4f51-889d-987b8b4e1ca9)

5 - Navigate to the public-facing page of the new content:

![image](https://github.com/user-attachments/assets/72623244-7041-40fb-9991-9b5ade42efb9)

/demo/{page-id}/

The JavaScript code will be executed immediately, confirming the stored XSS.
![image](https://github.com/user-attachments/assets/df1c8e8e-5551-4280-a928-bd2597098c7c)
![image](https://github.com/user-attachments/assets/f3faf1cd-ad25-41d7-b0f3-7b3b12cd6d94)


VENDOR: https://github.com/automadcms
