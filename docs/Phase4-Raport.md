**Dokumentation: Fas 4 – Webbkonfiguration \& Databaskoppling**



**1. Sammanfattning**



I denna fas har fokus legat på att driftsätta applikationslagret. Målet var att konfigurera `web1` och `web2` med en Python-baserad Flask-applikation och säkerställa att dessa kan kommunicera säkert med databasservern (`database`) över det privata nätverket.



**2. Arkitektur**



Systemet använder nu en klassisk



&#x20;**\*3-tier arkitektur\***



* **Applikationslager:** Flask-applikationer körs på Web1 och Web2 via WSGI-servern Gunicorn.
* **Databaslager:** PostgreSQL på en dedikerad server (Fas 3).
* **Kommunikation:** Sker via port 5432 (DB) och port 5000 (Webb).





**3. Genomförande**



Följande steg har utförts via automatisering och manuell finjustering:



**3.1 Ansible-automatisering**



En ny playbook, `web\_setup.yml`, skapades och kördes för att:



\- Installera Python-miljöer och nödvändiga bibliotek (`psycopg2` för databaskoppling).



\- Skapa applikationsstrukturen i `/var/www/my\_app`.



\- Driftsätta en `app.py` som dynamiskt hämtar data från databasen.



\- Starta webbtjänsten i bakgrunden med \*\*Gunicorn\*\*.



**3.2 Brandväggskonfiguration (UFW)**



**- Webbservrar:** Öppnade port 5000 för att tillåta inkommande webbtrafik.

**- SSH:** Säkerställde att port 22 förblev öppen för framtida underhåll.



**4. Felsökning \& Optimering (Viktigt)**



Under driftsättningen stötte vi på ett kritiskt anslutningsfel: \*“Connection refused”\* från databasservern. Detta löstes genom att:



**Listen Addresses:** Ändrade PostgreSQL-konfigurationen (`postgresql.conf`) från `localhost` till `\*` för att tillåta nätverksanslutningar.



**Access Control:** Uppdaterade `pg\_hba.conf` för att tillåta trafik från lab-nätverket (`192.168.56.0/24`) med MD5-autentisering.



**Inventory Match:** Korrigerade gruppnamnet i Ansible från `web` till `webservers` för att matcha projektets inventory-fil.



**5. Verifiering**



Verifiering utfördes genom att anropa servrarna internt via `curl http://localhost:5000`. 



**Resultatet bekräftade en lyckad koppling:**

"Succé från web1! Webbservern har kontakt med databasen."





