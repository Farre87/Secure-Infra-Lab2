**Dokumentation: Fas 5 – Lastbalansering och Publik Access**



**1. Introduktion**



Målet med Fas 5 var att implementera en "Front-end" för vår infrastruktur. Istället för att användare ansluter direkt till enskilda webbservrar på höga portnummer (t.ex. :5000), introducerar vi en \*Load Balancer\* (Lastbalanserare) som fungerar som systemets ansikte utåt.



**2. Arkitektur och Design**



I denna fas konfigurerades noden `nginx` (192.168.56.21) att agera som en Reverse Proxy.



**2.1 Komponenter**



**- Nginx:** Valdes som mjukvara för lastbalansering på grund av dess höga prestanda och pålitlighet.



**- Upstream Cluster:** En logisk grupp bestående av `web1` (192.168.56.22) och `web2` (192.168.56.23).

**- Algoritm:** Vi använder \*Round Robin\* (standard), vilket innebär att varannan förfrågan skickas till Web1 och varannan till Web2.







**3. Genomförande \& Automatisering**



Konfigurationen utfördes helt via Ansible-playbooken `nginx\_setup.yml`.



**3.1 Viktiga konfigurationssteg:**



* **Installation:**`apt install nginx`.



* **Proxy-inställningar:** Konfigurerade Nginx att lyssna på port 80 och skicka trafiken vidare (`proxy\_pass`) till vår `my\_web\_cluster`.



* **Header Management:** Vidarebefordrade klientens IP-adress via `X-Real-IP` så att webbservrarna kan se vem den faktiska besökaren är.
* **Cleanup:** Tog bort standardkonfigurationen (`default`) för att undvika konflikter.





**4. Säkerhet och Brandvägg**



För att skydda lastbalanseraren konfigurerades UFW (Uncomplicated Firewall) enligt följande:



**- Port 80 (HTTP):** Öppen för publik trafik så att användare kan nå hemsidan.

**- Port 22 (SSH):** Öppen endast för administration via Ansible/Vagrant.

**- Default Policy:** Deny (all annan trafik blockeras).





**5. Verifiering och Testning**



Verifiering skedde genom att anropa lastbalanserarens IP-adress: `http://192.168.56.21`.



**Resultat:**



\- Vid upprepade siduppdateringar (F5) växlade svaret mellan:



&#x20; `<h1>Succé från web1!</h1>`

&#x20; `<h1>Succé från web2!</h1>`



\- Detta bekräftar att lastbalanseraren framgångsrikt fördelar trafiken och att båda backend-servrarna har en fungerande koppling till databasen.



**6. Lärdomar**



Under denna fas blev det tydligt hur en lastbalanserare ökar systemets \*tillgänglighet\* (High Availability). Om en webbserver skulle stängas ner, märker användaren ingenting då Nginx automatiskt skickar all trafik till den kvarvarande servern.











