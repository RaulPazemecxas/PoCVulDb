### VIDEO POC: https://youtu.be/-mlmTZ-3PzM

*Vendor: Mercusys*
*Product : Router MW301R*
*Version: 1.0.2 Build 190726 Rel.59423n (4252)*

**Vulnerability:** Authentication Bypass in reset password.

In authenticated sessions, it is possible to completely bypass the password‑change workflow without knowing the current admin password. 
On the Mercusys MW301R, the official recovery method for a forgotten password is to perform a factory reset—which requires physical access—or, within a valid session, to supply the existing password. 
The discovered bypass allows an attacker who is already authenticated to intercept the HTTP request and simply modify the code parameter to invoke the reset endpoint directly. 
This enables the administrator password to be changed remotely, without any physical interaction with the device or knowledge of the previous credential.


1 - Log in to the web interface

-Navigate to http://192.168.1.1/ and authenticate using the administrator password.

Note: If you forget the password, the only official recovery method is a factory reset via the physical Reset button (press and hold until all LEDs light up, then release), as there is no “forgot password” flow in the web UI.

![image](https://github.com/user-attachments/assets/962bd223-129f-4eae-9559-d56e592a6efc)

![image](https://github.com/user-attachments/assets/c6df40c3-389a-417f-a859-936f930c14bd)

![image](https://github.com/user-attachments/assets/be579659-57b2-40e5-b4e9-9e14746aad6f)


2 - Intercept a valid request

-While authenticated, perform any action in the interface that issues a POST request containing code=<value> and id=<token> (for example, a session‑keepalive or status check).

-In your proxy, capture that request to obtain a valid session id.

![image](https://github.com/user-attachments/assets/a1010727-4c33-4ff2-87db-7c9459dfbc82)


3 - Change the code parameter to 5

-In the intercepted request, edit the query string so that code=<original> becomes code=5.
Forward the modified request to the router.

![image](https://github.com/user-attachments/assets/4cf64f7d-86be-4ad2-8205-a23684f5e83e)

4 - Reload the page and set new credentials

-Refresh http://192.168.1.1/ in your browser.

5 - The UI will now display the “Create a Login Password” form—no current password is requested.

Enter and confirm your new password to complete the reset remotely, without any physical access or knowledge of the previous credential.

![image](https://github.com/user-attachments/assets/4c937df0-83ed-4f05-be2c-2533c4727c0b)


