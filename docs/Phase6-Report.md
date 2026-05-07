**Dokumentation: Fas 6 – Systemövervakning och Realtidsanalys**





**1. Syfte och Mål**



Målet med Fas 6 var att implementera en centraliserad övervakningslösning för hela infrastrukturen. I en professionell miljö är det kritiskt att kunna identifiera prestandaproblem, flaskhalsar eller serverfel innan de påverkar slutanvändaren.









**2. Verktygsval: Netdata**



För denna laboration valdes \*Netdata\* som övervakningsverktyg på noden `monitor` (192.168.56.25). 



* **Fördelar:** Netdata ger extremt hög upplösning (data per sekund) och kräver ingen komplex databaskonfiguration för att visa realtidsgrafer.



* **Visualisering:** Verktyget erbjuder en interaktiv webb-dashboard som automatiskt upptäcker systemresurser.









**3. Implementering (Ansible)**



Installationen automatiserades med en Ansible-playbook (`monitor\_setup.yml`) som utförde följande steg:



**1. Kickstart:** Körde Netdatas officiella installationsskript direkt på monitor-noden.



**2. Brandväggshantering:** Konfigurerade UFW för att tillåta inkommande trafik på port \*\*19999\*\*, vilket är standardporten för Netdatas webbgränssnitt.



**3. Tjänstehantering:** Säkerställde att `netdata`-tjänsten startar automatiskt vid systemstart (enabled).









**4. Övervakade Parametrar**



Genom den nya dashboarden kan administratören nu följa:



* **CPU \& RAM:** Se belastningen på monitor-noden i realtid.



* **Nätverkstrafik:** Övervaka inkommande och utgående paket (viktigt för att se belastningen från lastbalanseraren).



* **Disk-I/O:** Kontrollera att databasanrop inte skapar för långa väntetider på hårddisken.







**5. Verifiering**



Installationen verifierades genom att besöka `http://192.168.56.25:19999` från en extern webbläsare. Systemet visade omedelbart live-data, vilket bekräftar att nätverkskommunikationen mellan Windows-värden och Linux-gästservern fungerar korrekt genom brandväggen.















