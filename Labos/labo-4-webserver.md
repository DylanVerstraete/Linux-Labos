# Labo 4: Webserver/LAMP stack

In dit labo zullen we een webserver opzetten in de VM die je in het vorige labo gemaakt hebt.
Een van de populairste toepassingen van Linux als server is de zgn. LAMP-stack. Deze afkorting staat voor Linux + Apache + MySQL + PHP. De combinatie vormt een platform voor het ontwikkelen van webapplicaties waar vele bekende websites gebruik van maken (bv. Facebook).

Als je gebruik maakt van andere bronnen (bv. blog-artikel of HOWTO die je op het Internet vond), voeg die dan toe aan je logboek. Zo kan je die later makkelijk terug vinden.

## De Apache webserver installeren

Het is belangrijk dat je controleert voordat je aan dit labo begint, dat je twee netwerkinterfaces hebt op je virtuele machine. De ene moet van het type *NAT* zijn. Deze heeft verbinding met het internet en heeft typisch als IP-adres 10.0.2.15. De andere netwerkinterface moet van het type *Host-only* zijn. Via deze kan je communiceren met de host-machine en je webserver testen. Als je niets hebt veranderd de standaardinstellingen van VirtualBox, is het IP-adres hoogstwaarschijnlijk 192.168.56.101.

1. Installeer Apache op je virtuele machine en verifieer dat hij draait en vanop je host-machine bereikbaar is.
2. Installeer ondersteuning voor PHP en verifieer dat dit werkt, bijvoorbeeld met een eenvoudige PHP-pagina

### Procedure

Beschrijf hier de exacte procedure hoe je dit uitgevoerd hebt. Zorg er voor dat je aan de hand van je beschrijving deze taken later heel vlot kan herhalen als dat nodig is. Test ook telkens na elke stap dat die correct verlopen is.

1. installeren via commando 'sudo dnf install httpd php"
2. commando 'sudo systemctl start httpd' uitvoeren om apache te starten
3. commando 'sudo systemctl status httpd' voor de status van apache
commando's van systemctl: start, stop, status, restart, enable
enable -> draaien bij start VM
4. 'sudo firewall-cmd --list-al' geeft overzicht van alle firewall regels
5. firewall correct configureren
6. sudo firewall-cmd --add-service=http om http to te voegen
belangrijk: na heropstarten vergeet firewall dit -> optie --permanent toeveogen aan commando:
sudo firewall-cmd -add-service=http --permanent
dit commando voert wijzigingen niet onmiddelijk door -> zowel met als zonder permanent eens uitvoeren
7. om poort toe te voegen: 'sudo firewall-cmd --add-port=80/tcp' (opnieuw zowel met optie permanent als zonder)
8. testen of netwerk service correct draait: 'ss' (nieuwere versie van netstat)
enkele opites: -l -> alle server poorten, -u -> open ip poorten, -p -> welke processen erachter draaien (voor deze optie is sudo nodig)
9. testen of php werk: web pagina's zitten onder /var/www
10. ga naar /var/www/html en maak in deze map een bestand 'index.php' aan (met sudo)
11. vul bestand met html pagina (en een lijnphp: <?php phpinfo(); ?>
12. surf naar pagina, kijk of het werkt (je ziet phpinfo)

## MariaDB (MySQL)

MariaDB is de naam van een variant (fork) van de bekende database MySQL. Onder Fedora is MySQL zelf niet meer beschikbaar, maar MariaDB is volledig compatibel. Installeer MariaDB op je virtuele machine. Voer daarna het script `mysql_secure_installation` uit om het root-wachtwoord voor MariaDB in te stellen. Installeer ook PHPMyAdmin, dit is een webinterface voor het beheer van MySQL/MariaDB.


### Procedure

Beschrijf hier de exacte procedure hoe je dit uitgevoerd hebt. Zorg er voor dat je aan de hand van je beschrijving deze taken later heel vlot kan herhalen als dat nodig is. Test ook telkens na elke stap dat die correct verlopen is.

1. naam package is mariadb-server, we zullen ook phpmyadmin installereni: sudo dnf install mariadb-server phpmyadmin
2. gebruik systemctl (mariadb heeft id 'mariadb') om service aan te passen
3. starten via 'sudo systemctl start mariadb' & status bekijken
bij installeren apache, wijzigt hij de configuratie (in /etc),
apache heeft directory httpd aangemaakt in /etc,
surf naar 192.168.56.101/phpmyadmin (op echte computer) -> not found
na installeren eerst eens httpd herstarten, zodat phpmyadmin werkt,
terug naar webpagina (op echte computer) -> forbidden, privileges instellen
sudo vim /etc/httpd/conf.d/phpMyAdmin.conf -> configuratiebestand phpmyadmin
note: kan wel vanaf begin aan phpmyadmin vanaf vmware machine
privilege instellen voor iedereen:
in bestand phpMyAdming.conf: aanpassen tussen RequireAny naar 'Require all granted'
dan apache server herstarten (systemctl restart httpd) om wijzigingen door te voeren

ga terug naar webpagina: om in te loggen eerst mariadb initialiseren:
'sudo mysql_secure_installation'
geef root wachtwoord? -> standaard leeg
set root password -> y
gekozen wachtwoord: letmein
remove anonymous users -> y
alle andere vragen : y

ga naar webpagina / phpmyadmin: user: root, pass: letmein

## Webapplicatie

Kies een webapplicatie gebaseerd op PHP en installeer op je webserver. Enkele voorbeelden die je kan gebruiken: Drupal, Wordpress, Joomla, MediaWiki, enz.,

 
### Procedure

Beschrijf hier de exacte procedure hoe je dit uitgevoerd hebt. Zorg er voor dat je aan de hand van je beschrijving deze taken later heel vlot kan herhalen als dat nodig is. Test ook telkens na elke stap dat die correct verlopen is.

1. kiezen om Wordpress te installeren
2. sudo dnf install wordpress
3. terug zelfde probleem (wordpress niet gevonden) -> apache herstarten
4. conf aanpassen: /etc/httpd/conf.d/wordpress.conf (onder apache 2.4, 'Require all granted')
5. terug naar pagina surfen: pagina/wordpress -> error establishing database connection
6. maak gebruiker aan in phpymadmin naam: wordpress, hostname: local (kan enkel lokaal inloggen), password: letmein, eerste checkbox aanvinken, dan onderaan op go klikken
7. werkt nog niet: aan wordpress laten weten, sudo vim /etc/wordpress/wp-config.php, aanpassen: onder define: DB_NAME ->  wordpress, user -> wordpress, pass -> letmein, zelfde pass van wordpress account die je in phpmyadmin aanmaakte
8. apache herstarten

zelfde werkwijze voor drupel

## Netwerkconfiguratie en troubleshooting

1. Welk(e) IP-adres(sen) heeft je VM? Vul onderstaande tabel aan (voeg rijen toe zoveel als nodig). Welk commando gebruikte je?

    ```
    $ ip a
    ...
    ```

    | Interface | IP-adres       | Netwerkmasker |
    | :---      | :---           | :---          |
    | lo        | 127.0.0.1      | 8             |
    | enp0s3    | 10.0.2.15      | 24            |
    | enp0s8    | 192.168.56.101 | 24              |

2. Via welke router/default gateway kan je VM naar het Internet? Welk commando gebruikte je?

    ```
    $ route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         10.0.2.2        0.0.0.0         UG    100    0        0 enp0s3
    10.0.2.0        0.0.0.0         255.255.255.0   U     100    0        0 enp0s3
    192.168.56.0    0.0.0.0         255.255.255.0   U     100    0        0 enp0s8
    ```

3. Wat is het IP-adres van `www.hogent.be`?

    ```
    $ ping www.hogent.be
    64 bytes from 178.62.144.90 ...
    ```

4. Om het IP-adres van websites en dergelijke op te kunnen vragen, moet je VM kunnen contact maken met een DNS-server. Hoe weet Linux welke DNS-server(s) beschikbaar is/zijn? Geef het configuratiebestand en de huidige inhoud ervan.

    ```
    $ cat /etc/resolv.conf
    # Generated by NetworkManager
    nameserver 10.0.2.3
    ```

5. In welk(e) configuratiebestanden worden de instellingen van je netwerkinterfaces bijgehouden? Geef hieronder telkens ook de inhoud van deze bestanden:

    ```
    $ cd /etc/sysconfig/network-scripts
    cat  ifcfg-enp0s3
    HWADDR=08:00:27:57:F9:75
    TYPE=Ethernet
    BOOTPROTO=dhcp
    DEFROUTE=yes
    PEERDNS=yes
    PEERROUTES=yes
    IPV4_FAILURE_FATAL=no
    IPV6INIT=yes
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes
    IPV6_FAILURE_FATAL=no
    IPV6_ADDR_GEN_MODE=stable-privacy
    NAME=enp0s3
    UUID=2dd84b1c-901e-3dbc-bdb6-6c99ceb6effd
    ONBOOT=yes
    AUTOCONNECT_PRIORITY=-999
    ```

6. Met welk commando test je of een host op het netwerk op dit moment online is? Probeer dit uit  vanop je VM met het IP-adres van je host-systeem en voeg de uitvoer hieronder in. Welk protocol uit de TCP/IP familie wordt door deze tool gebruikt?

    ```
    $ ping ...
    ...
    ```
   deze tool gebruikt het ICMP-protocol 

7. Met het commando `ss -tln` kan je opvragen welke services er draaien op je systeem, ahv. de open netwerkpoorten. Leg uit wat de opties (`-tln`) betekenen. Probeer het commando uit op je VM wanneer Apache en MariaDB draaien en voeg de uitvoer hieronder in. Geef voor elke open poort beneden de 10.000 welke netwerkservice er mee geassocieerd is.

    ```
    $ ss -tln
    State      Recv-Q Send-Q   Local Address:Port                    Peer Address:Port 
    LISTEN     0      5            127.0.0.1:ipp                                *:*           
    LISTEN     0      80                  :::mysql                             :::*             
    LISTEN     0      128                 :::http                              :::*             
    LISTEN     0      5                  ::1:ipp                               :::*  
    ```
    
    t: enkel TCP sockets tonen
    l: toon luisterende sockets
    n: numeriek, toon geen servicenamen

## Systeemlogbestanden

Om oorzaken te vinden van problemen op een Linux-systeem, maken systeembeheerders intensief gebruik van zgn. logbestanden. De meeste netwerkservices houden hun eigen logbestand(en), of maken gebruike van het algemene logsysteem dat op recente versies van Fedora `journald` heet.

1. In welke directory horen alle logbestanden volgens de Linux Filesystem Hierarcy Standard thuis?

  **Antwoord:**: /var/log

2. Met welk commando kan je de algemene systeemlogs bekijken op een recente versie van Fedora?

    ```
    $ journalctl
    UITVOER
    ```

3. Met welk commando (incl. opties) kan je de logs blijven bekijken terwijl er nieuwe boodschappen toegevoegd worden? (Ter info: dit afsluiten kan met `<Ctrl-C>`

    ```
    $ journalctl -f
    UITVOER
    ```

4. Met welk commando kan je de logs voor een bepaalde netwerkservice bekijken?

    ```
    $ journalctl -u httpd
    ...
    ```

5. Open in één console het algemene logbestand en hou het open voor nieuwe boodschappen (zoals hierboven gevraagd). Open nu een andere console waar je de host-only netwerkinterface opnieuw opstart. Wat gebeurt er in de logs? Bekijk in het bijzonder de lijnen met `dhclient`.

    ```
    $ journalctl -f
    $ systemctl restart network
    $ journalctl | grep 'dhclient'
    nov 16 13:39:20 localhost.localdomain NetworkManager[837]: <info>  [1479299960.6446] Using DHCP client 'dhclient'
    nov 16 13:39:29 localhost.localdomain NetworkManager[837]: <info>  [1479299969.9025] dhcp4 (enp0s8): dhclient started with pid 1097
    nov 16 13:39:29 localhost.localdomain NetworkManager[837]: <info>  [1479299969.9443] dhcp4 (enp0s3): dhclient started with pid 1099
    nov 16 13:39:30 localhost.localdomain dhclient[1099]: DHCPREQUEST on enp0s3 to 255.255.255.255 port 67 (xid=0xbbc1045c)
    nov 16 13:39:30 localhost.localdomain dhclient[1099]: DHCPACK from 10.0.2.2 (xid=0xbbc1045c)
    nov 16 13:39:30 localhost.localdomain dhclient[1097]: DHCPDISCOVER on enp0s8 to 255.255.255.255 port 67 interval 4 (xid=0x4da9644a)
    nov 16 13:39:30 localhost.localdomain dhclient[1097]: DHCPREQUEST on enp0s8 to 255.255.255.255 port 67 (xid=0x4da9644a)
    nov 16 13:39:30 localhost.localdomain dhclient[1097]: DHCPOFFER from 192.168.56.100
    nov 16 13:39:30 localhost.localdomain dhclient[1097]: DHCPACK from 192.168.56.100 (xid=0x4da9644a)
    nov 16 13:39:30 localhost.localdomain dhclient[1099]: bound to 10.0.2.15 -- renewal in 39610 seconds.
    nov 16 13:39:30 localhost.localdomain dhclient[1097]: bound to 192.168.56.101 -- renewal in 489 seconds.
    nov 16 13:43:32 localhost.localdomain NetworkManager[837]: <info>  [1479300212.8767] dhcp4 (enp0s3): dhclient started with pid 3053
    nov 16 13:43:32 localhost.localdomain dhclient[3053]: DHCPREQUEST on enp0s3 to 255.255.255.255 port 67 (xid=0x28d34017)
    nov 16 13:43:32 localhost.localdomain dhclient[3053]: DHCPACK from 10.0.2.2 (xid=0x28d34017)
    nov 16 13:43:32 localhost.localdomain dhclient[3053]: bound to 10.0.2.15 -- renewal in 41431 seconds.
    ```
    merk op: kregen opnieuw IP van DHCP-server (DHCPREQUEST, DHCPOFFER, ...)
    
    note: met de commando's 'ifup' & 'ifdown' kan je een specifieke netwerkinterface uitleggen en opnieuw opstarten. systemctl restart network herstart alles.

6. Wat is de naam van het logbestand waar je kan opvolgen welke webpagina's er opgevraagd worden aan je webserver? **Antwoord:** `/var/log/httpd/access_log`
7. Open dit bestand met `tail -f` en laad een webpagina via een webbrowser. Wat gebeurt er in het logbestand?

    ```
    $ sudo tail -f /var/log/httpd/access_log
    127.0.0.1 - - [16/Nov/2016:13:58:26 +0100] "GET / HTTP/1.1" 200 92537 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    127.0.0.1 - - [16/Nov/2016:13:58:27 +0100] "GET /favicon.ico HTTP/1.1" 404 209 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    127.0.0.1 - - [16/Nov/2016:13:58:27 +0100] "GET /favicon.ico HTTP/1.1" 404 209 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    ^C
    [student@localhost log]$ sudo tail -f /var/log/httpd/access_log
    127.0.0.1 - - [16/Nov/2016:13:58:26 +0100] "GET / HTTP/1.1" 200 92537 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    127.0.0.1 - - [16/Nov/2016:13:58:27 +0100] "GET /favicon.ico HTTP/1.1" 404 209 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    127.0.0.1 - - [16/Nov/2016:13:58:27 +0100] "GET /favicon.ico HTTP/1.1" 404 209 "-" "Mozilla/5.0 (X11; Fedora; Linux x86_64; rv:49.0) Gecko/20100101 Firefox/49.0"
    ```
    
    note:
    - sudo cd /var/log/httpd kan niet, de gebruiker 'student' kan niet in deze map gaan,
    - sudo ls /var/log/httpd kan wel om te zien welke log bestanden er in de map staan
    - 'sudo vim /var/log/httpd/access_log' is niet de bedoeling, een log bestand mogen we niet aanpassen, gebruik dus het 'tail' commando om log bestanden te lezen
    - tail -f : de 'f' optie staat voor 'follow', als er data bijkomt, dan zie je dat ook in de console
    - dit bestand kan leeg zijn
## Gebruikte bronnen

- 
