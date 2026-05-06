**Dokumentation: Fas 2 – Ansible \& SSH-Automation**



**1. Syfte**



Målet med denna fas var att konfigurera control-noden som en central hanteringsenhet (Ansible Control Node) och etablera säker kommunikation med alla andra servrar i labbet utan krav på manuell lösenordsinmatning.





**2. Genomförande**



**Installation**



Ansible installerades manuellt på control-noden.



&#x20;   Kommando:



&#x20;   **sudo apt update**

&#x20;   **sudo apt install -y ansible**



En inventariefil (inventory) skapades i mappen \~/ansible/ för att mappa upp labbmiljön**:**



&#x20;   Kommando för att skapa filen: **nano \~/ansible/inventory**



&#x20;   **Struktur:**



&#x20;       Loadbalancer: nginx (192.168.56.21)



&#x20;       Webbservrar: web1, web2 (192.168.56.22-23)



&#x20;       Databas: database (192.168.56.24)



&#x20;       Monitor: monitor (192.168.56.25)





**SSH-Konfiguration**



För att möjliggöra automation genererades ett SSH-nyckelpar på control-noden.



&#x20;   Kommando för att skapa nyckel:

&#x20;   

&#x20;   **ssh-keygen -t ed25519 -N "" -f \~/.ssh/id\_ed25519**



Den publika nyckeln distribuerades därefter till samtliga noder för att tillåta lösenordsfri inloggning.





**3. Felsökning (Viktigt!)**



**Problem: Permission Denied (publickey)**



* Vid det första försöket att pinga servrarna via Ansible nekades anslutningen.





* **Orsak:** Målservrarna kände inte igen control-nodens nyckel, och filrättigheterna för .ssh-mappen var inte korrekt inställda.



**Lösning 1 (Sätta lösenord på noder):**



Användaren vagrant gavs ett tillfälligt lösenord via Windows-terminalen för att tillåta nyckelöverföring.



**vagrant ssh <servernamn> -c "echo 'vagrant:vagrant' | sudo chpasswd"**





**Lösning 2 (Överföra nyckel):**



Kommandot ssh-copy-id användes inifrån control för att föra över nyckeln och sätta rätt behörighet automatiskt.



**ssh-copy-id -i \~/.ssh/id\_ed25519.pub vagrant@<IP-ADRESS>**







**Lösning 3 (Säkra rättigheter manuellt):**



Rättigheterna säkrades för att SSH-tjänsten ska acceptera nycklarna.



**chmod 700 \~/.ssh**

**chmod 600 \~/.ssh/authorized\_keys**







**4. Verifiering**



Fasen avslutades med ett lyckat "Ad-hoc"-kommando från Ansible som bekräftade anslutning till alla noder:





**ansible all -i \~/ansible/inventory -m ping**



Resultat: Samtliga noder svarade med SUCCESS och pong.







































