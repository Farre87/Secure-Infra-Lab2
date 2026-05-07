**Rapport: Fas 3 – Säkring och Automatisering av Databasserver**





**Inledning**



I denna fas har fokus legat på att sätta upp en robust och säker databasmiljö med PostgreSQL. Målet var att automatisera installationen via Ansible samt att tillämpa "Principle of Least Privilege" genom att skapa en specifik databasanvändare med begränsade rättigheter.



**Genomförande**



**1. Automatisering med Ansible**



En Playbook (db\_setup.yml) skapades för att utföra följande steg på servern database (192.168.56.24):



* &#x20;   Installation av postgresql och nödvändiga Python-bibliotek (python3-psycopg2).



* &#x20;   Installation av acl för att möjliggöra säker växling mellan systemanvändare under körning.



* &#x20;   Skapande av en dedikerad databasanvändare: farre\_user.



* &#x20;   Skapande av applikationsdatabasen: secure\_db.





**2. Nätverkssäkerhet och Brandvägg**



För att skydda datan konfigurerades UFW (Uncomplicated Firewall) med en strikt policy:



* &#x20;   Port 5432 (PostgreSQL): Endast tillåten från det interna labbnätverket (192.168.56.0/24).



* &#x20;   Port 22 (SSH): Tillåten för att bibehålla åtkomst via Ansible och Vagrant.







**Felsökning (Viktiga lärdomar)**



Under arbetets gång stötte jag på tekniska hinder som krävde djupare analys av PostgreSQL:s säkerhetslager.





**Utmaning A: Brandväggs-lockout**



När brandväggen aktiverades stängdes SSH-porten av misstag, vilket bröt anslutningen till servern.



&#x20;   **Lösning:** Använde VirtualBox seriella konsol för att manuellt köra sudo ufw allow 22/tcp. Playbooken uppdaterades för att säkerställa att SSH alltid är öppet.





**Utmaning B: Remote Connection Refused**



Trots att brandväggen var öppen vägrade PostgreSQL att svara på anrop från webbservrarna.



&#x20;   **Analys:** PostgreSQL lyssnar som standard endast på localhost (127.0.0.1).



&#x20;   **Lösning:** Modifierade postgresql.conf och ändrade listen\_addresses till '\*'.





**Utmaning C:** Autentisering och pg\_hba.conf



Webbservrarna fick kontakt men nekades tillträde av databasens interna säkerhetspolicy.



&#x20;   **Lösning:** Konfigurerade pg\_hba.conf genom att lägga till en rad som tillåter trafik från nätverket:



&#x20;   host    all    all    192.168.56.0/24    md5





**Resultat**



Databasservern är nu i full drift och i ett läge där den endast accepterar anslutningar från behöriga servrar i labbmiljön. Verifiering har utförts genom att kontrollera status på databasklustret (pg\_lsclusters) vilket visar status online.









