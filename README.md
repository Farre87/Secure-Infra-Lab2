\# Secure Infrastructure Lab 2 – Full Stack Automation \& 3-Tier Architecture



\## 1. Projektöversikt

Detta projekt omfattar design, implementering och automatisering av en skalbar och säker IT-infrastruktur. Genom att använda \*\*Infrastructure as Code (IaC)\*\* har en komplett miljö byggts upp från grunden, bestående av lastbalansering, redundanta webbservrar, en härdad databasserver och ett realtidsövervakningssystem.



\## 2. Arkitektur \& Design

Systemet är uppbyggt enligt en \*\*3-tier arkitektur\*\* för att separera logik, data och presentation, vilket ökar både säkerheten och skalbarheten.



\* \*\*Load Balancer (Nginx):\*\* Fungerar som "dörrvakt" och distribuerar trafik mellan webbservrarna.

\* \*\*Web Tier (Flask/Gunicorn):\*\* Två noder (`web1`, `web2`) som hanterar applikationslogiken.

\* \*\*Database Tier (PostgreSQL):\*\* En isolerad databasnod för säker lagring.

\* \*\*Monitoring Tier (Netdata):\*\* En dedikerad nod för prestandaövervakning.

\* \*\*Control Node (Ansible):\*\* Den centrala noden varifrån all konfiguration styrs.







\## 3. Verktyg \& Teknologier

Följande verktyg har varit centrala för projektets genomförande:

\* \*\*Vagrant \& VirtualBox:\*\* För virtualisering och hantering av lokala servrar.

\* \*\*Ansible:\*\* För automatiserad konfiguration och mjukvaruinstallation.

\* \*\*Nginx:\*\* Som Reverse Proxy och Load Balancer.

\* \*\*Python Flask \& Gunicorn:\*\* För webbapplikation och WSGI-server.

\* \*\*PostgreSQL:\*\* Som relationell databas.

\* \*\*Netdata:\*\* För realtidsövervakning av systemresurser.

\* \*\*Git \& GitHub:\*\* För versionshantering och dokumentation.



\## 4. Genomförande i Faser

Projektet har genomförts systematiskt i sju faser:

1\.  \*\*Fas 1-2:\*\* Setup av Vagrant-miljö, SSH-nycklar och Ansible Inventory.

2\.  \*\*Fas 3:\*\* Installation och härdning av PostgreSQL-databasen.

3\.  \*\*Fas 4:\*\* Driftsättning av Flask-applikationer på Web1 och Web2.

4\.  \*\*Fas 5:\*\* Konfiguration av Nginx som Load Balancer med Round Robin-algoritm.

5\.  \*\*Fas 6:\*\* Implementering av övervakningssystemet Netdata på en dedikerad nod.

6\.  \*\*Fas 7:\*\* Slutgiltig säkerhetsverifiering och sammanställning av dokumentation.



\## 5. Säkerhetsimplementering

Säkerhet har varit en röd tråd genom hela projektet:

\* \*\*Brandvägg (UFW):\*\* Varje server har en "Default Deny"-policy. Endast nödvändiga portar (80, 5000, 5432, 22) har öppnats för specifika nätverkssegment.

\* \*\*Databasrestriktioner:\*\* PostgreSQL har konfigurerats (`pg\_hba.conf`) för att endast tillåta anslutningar från webbservrarnas specifika IP-adresser.

\* \*\*Nätverksisolering:\*\* Databasen är inte nåbar från det publika nätverket, utan endast via det interna nätverket (192.168.56.0/24).

\* \*\*SSH-härdning:\*\* All kommunikation sker via säkra SSH-nycklar (Ed25519).



\## 6. Felsökning (Troubleshooting)

Under projektets gång löstes flera tekniska utmaningar:

\* \*\*Anslutningsfel (Connection Refused):\*\* Databasen lyssnade ursprungligen bara på `localhost`. Detta löstes genom att uppdatera `postgresql.conf` till `listen\_addresses = '\*'` och konfigurera tillträdesregler i `pg\_hba.conf`.

\* \*\*Ansible Inventory mismatch:\*\* Playbooks misslyckades initialt på grund av namnkonflikter. Detta rättades genom att synkronisera gruppnamnen i `inventory`-filen med playbook-inställningarna.

\* \*\*Nätverksrouting:\*\* Brandväggsregler i UFW blockerade inter-VM kommunikation, vilket löstes genom att explicit tillåta trafik på relevanta portar för det interna nätverket.



\## 7. Verifiering

Systemets funktion har verifierats genom:

1\.  Besök på `http://192.168.56.21` (Load Balancer) där trafik framgångsrikt växlar mellan Web1 och Web2.

2\.  Bekräftelse på att webbservrarna kan hämta och skriva data till PostgreSQL.

3\.  Realtidsdata i Netdata-dashboarden på port 19999.





