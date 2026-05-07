**Dokumentation: Fas 7 – Slutgiltig Säkerhet \& Verifiering**



**1. Sammanfattning**





Den sista fasen har fokuserat på att verifiera systemets integritet och säkerställa att infrastrukturen följer "Best Practices" för säkerhet och dokumentation.







**2. Säkerhetsåtgärder**



* **Nätverksisolering:** PostgreSQL-databasen är konfigurerad att endast acceptera anslutningar från det interna nätverket (192.168.56.0/24).



* **Brandväggshärdning:** Varje server (LB, Web, DB, Monitor) kör UFW med strikta regler för att minimera attackytan.



* **Automatisering:** Genom att använda Ansible har vi eliminerat risken för mänskliga konfigurationsfel vid driftsättning.







**3. Slutsats**



Laborationen har resulterat i en robust och skalbar 3-tier arkitektur. Systemet är redundant tack vare lastbalanseraren och övervakas i realtid för att säkerställa hög tillgänglighet.

