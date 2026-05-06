**Dokumentation: Fas 1 – Infrastruktur som kod (IaC)**



**1. Sammanfattning**



I denna fas har jag lagt grunden för labbmiljön genom att definiera sex virtuella servrar som kod med hjälp av Vagrant och VirtualBox. Målet var att skapa en miljö som är identisk varje gång den startas.



**2. Genomförande**



**Projektstart och Versionshantering**



Projektet initierades lokalt och kopplades till ett repository på GitHub för versionshantering.



**Repository:** (https://github.com/Farre87/Secure-Infra-Lab2)



**Branch-strategi**: Använt feature/phase1-vagrant-setup för utveckling innan merge till main.



**Konfiguration av noder (Vagrantfile)**



Jag har skapat en Vagrantfile som definierar sex noder med specifika resurser (CPU/RAM) och statiska IP-adresser i ett privat nätverk (192.168.56.0/24).



**Noder i miljön:**



Namn	        IP	            Syfte	                  RAM


control   	192.168.56.20   	Ansible Control Node  	2048MB

nginx	      192.168.56.21	    Load Balancer	          1024MB

web1      	192.168.56.22	    Webserver 1           	1024MB

web2      	192.168.56.23    	Webserver 2	            1024MB

databas     192.168.56.24	    PostgreSQL	            2048MB

monitor    	192.168.56.25   	Wazuh Monitoring	      3072MB





**3. Felsökning och Lärdomar**



**Problem: Oönskade filer i Git**



Under den första incheckningen upptäcktes att 68 filer relaterade till Vagrants interna status (.vagrant/) inkluderades i commiten. Detta är en säkerhetsrisk eftersom mappen kan innehålla lokala sökvägar och temporära SSH-nycklar.



**Lösning: .gitignore**



**För att åtgärda detta genomfördes följande steg:**



1.Skapade en .gitignore-fil med innehållet .vagrant/.

2.Rensade Gits cache med kommandot: git rm -r --   cached .vagrant.

3.Genomförde en ny commit för att ta bort filerna från GitHub.



**Lärdom:** Kontrollera alltid status på filer innan git add . och använd alltid en .gitignore för miljöspecifika filer.





**4. Verifiering**



Miljön har verifierats med kommandot vagrant status, vilket bekräftar att alla sex maskiner körs korrekt i VirtualBox.



control                   running (virtualbox)

nginx                     running (virtualbox)

web1                      running (virtualbox)

web2                      running (virtualbox)

database                  running (virtualbox)

monitor                   running (virtualbox)



