# Labo 5: Bestandspermissies

Als je gebruik maakt van andere bronnen (bv. blog-artikel of HOWTO die je op het Internet vond), voeg die dan toe aan het einde van dit document. Zo kan je het later terug vinden.

Maak ter voorbereiding zeker de oefeningen in Linux Fundamentals (Paul Cobbaut) over dit onderwerp (pp. 222 en 228).


## Algemene permissies

Geef in de volgende oefeningen telkens het commando dat nodig is om de taak uit te voeren en ook het resultaat.

1. Log in als gewone gebruiker. Wat is de waarde van umask?

  note: sudo !! -> voert vorige commando met sudo uit

    note: umask is builtin commando, zit ingebakken in bash, 'man umask' toont alle ingebakken commando's, scroll naar beneden naar 'umask' om opties te zien

    ```
    $ umask
    0002
    $ umask -S
    u=rwx,g=rwx,o=rx
    ```
    
    note umask moet je interpreteren als 'hoeveel permissies je afpakt'
    0002 -> octale weergave
    octaal: stelsel van 8, kan erover nadenken als 3-bits: 1 bit voor read, 1 bit voor write en 1 bit voor execute of waarde 0 tot 7
    
    umask: 4 octale getallen (0-7): 'speciale permissies' 'eigenaar' 'groep' 'anderen'
    0002:
    0 -> speciale permissies = 0,
    0 -> binair: 000 -> geen permissies afpakken van eigenaar -> rwx
    0 -> geen permissies afpakken van groep -> rwx
    2 -> binair: 010 (rwx) -> w permissie afpakken -> rx
    
    u=rwx, g=rwx, o=rx
    u = owner(eigenaar), g = group(groep), o = others(anderen)
    r = read, w = write, x = execute
    eg: rx -> lees- en uitvoerrechten
    rw -> lees- en schrijfrechten
    
    waarden rwx (octaal): r = 4, w = 2, x = 1
    eg rwx = 4 + 2 + 1 = 7
    r-x = 4 + 1 = 5
    -wx = 2 + 1 = 3
    
    van -rwxrwxr-x naar octale vorm: elke letter omzetten naar 1 bit: letter = '0', '-' = '1'
    -rwxrwxr-x -> 0000000010
    -rwxrw-r-- -> 0000001011
    
    bij bestand:
    r: kan je bestand lezen
    w: kan je naar bestand schrijven
    x: kan je bestand uitvoeren
    
    bij directory: licht verschil bij betekenis rechten:
    x: kan je naar map gaan (cd)
    
    dus opletten met chmod 777 -> aan iedereen alle rechten geven -> systeem gevaar
    
    umaskl: enkel 0,2,7
    chmod: enkel 0,4,5,6,7

2. Maak een directory `oefenenMetPermissies` aan. Welke octale permissies verwacht je voor deze nieuwe directory? 775 (want 777 - 002 = 775 in het octaal stelsel)

    ```
    $ stat -c "%a" .
    775
    ```

3. Controleer dit door de *symbolische* permissies op te vragen.

    ```
    $ ls -l
    drwxrwxr-x. 2 student student 4,1k 2016-11-16 14:15 oefenenMetPermissies
    ```

4. Controleer of de symbolische waarden en octale waarden overeen komen!
5. Stel nu de umask waarde zo in dat niemand permissies heeft op de bestanden en directories die je aanmaakt, behalve jijzelf. 

    ```
    $ umask 077
    ```
    
    opnieuw: 077 -> alle rechten van group & others afpakken
    

6. Maak nu een bestand aan met de naam `vanmij`, in de directory `oefenenMetPermissies` met als inhoud de tekst: `echo "Waarschuwing: eigendom van ${USER}!"` Schrijf neer hoe je dit bestand gemaakt hebt. Controleer de permissies; schrijf ze neer en was dit wat je verwachtte? Verklaar!

    ```
    $ echo "Waarschuwing: eigendom van ${USER}!" > vanmij
    $ cat vanmij
    Waarschuwing: eigendom van student!
    $ ls -l
    -rw-------. 1 student student 36 2016-11-16 14:47 vanmij
    ```

    merk op: eigenaar bestand heeft enkel read en write (rw) rechten, en geen execute (x)
    rechten, terwijl we toch de rechten op rwx voor de eigenaar hebben ingesteld?
    -> Bij aanmaken bestand krijgt een bestand NOOIT execute rechten

7. Verander nu de permissies van het bestand `vanmij` zodat je zelf het bestand kan uitvoeren. Geef het gebruikte commando (op de octale manier) en test het resultaat.

    ```
    $ chmod u+x vanmij #symbolische manier
    $ ls -l
    -rwx------. 1 student student 36 2016-11-16 14:47 vanmij
    
    OF
    
    $ chmod 700 vanmij #octale manier 
    ```
    
    chmod u+x vanmij: u(eigenaar) +(geven we) x(execute rechten)
    chmod g-x+w vanmij: g(groep) -(nemen we af:) x(execute rechten) +(geven we bij) w(write rechten)
    chmod a+x vanmij: a(all/iedereen) +(geven) x(execute rechten)

8. Log in als de gebruiker `alice` (als je deze niet meer hebt, maak je die aan en voorzie de gebruiker van een eenvoudig paswoord), maar zorg dat je niet verandert van directory. Kan `alice` het bestand `vanmij` uitvoeren? Verklaar!

    ```
    $ chmod 744 ~ #leesrechten geven van home map student aan iedereen
    $ su - alice
    <wachtwoord invoeren>
    $ ls /home/student
    ... #kunnen lezen maar niet naar map gaan (cd)
    ```

9. Verander nu de permissies van het bestand vanmij zodat iedereen het bestand kan uitvoeren. Geef het gebruikte commando (op de symbolische manier) en test het resultaat.

    ```
    $ chmod a+x vanmij
    
    ```

## Geavanceerde permissies

1. Zoek alle bestanden in het systeem waar de SUID-permissie op ingesteld staat. Schrijf het resultaat in een bestand met de naam `suidBestanden` in het homedirectory van de `root`. Schrijf de fouten weg naar een `foutenBestand`. Doe dit in één commandolijn. (Tip: gebruik het commando `find` en zoek in de manpages naar de geschikte opties).

    ```
    $ sudo find / -perm /4000 > suidBestanden 2> foutenBestand 
    UITVOER
    ```

2. Controleer het resultaat door een bestand te nemen uit `suidBestanden` en de permissies op te vragen van dat bestand.

    ```
    $ stat -c "%a" /usr/bin/crontab
    4755
    ```

3. Check of het programmabestand `/usr/bin/passwd` in het bestand `suidBestanden` aanwezig is. Schrijf het gebruikte commando op. Is het aanwezig?

    ```
    $ grep "/usr/bin/passwd" suidBestanden
    /usr/bin/passwd
    ```

## Geavanceerde permissies: setGID en de *sticky bit*

1. Ga naar de directory `oefenenMetPermissies`, als gewone gebruiker (dus niet als `root`) en kopieer het bestand `/etc/hosts` daar naartoe.

    ```
    $ cp /etc/hosts .
    ```

2. Verander nu de permissies van het bestand `hosts` in directory `oefenenMetPermissies` als volgt: SGID aan, `xr` voor *others*, `wr` voor *group* en geen permissies voor de eigenaar. Geef het gebruikte commando en ook het commando om te controleren of de permissies juist ingesteld zijn.

    ```
    $ chmod 4065 hosts
    $ ls -l
    ---Srw-r-x. 1 student student 158 2016-11-23 14:09 hosts
    ```

3. Kan de eigenaar nu het bestand bekijken? Waarom wel of niet? Noteer hoe je dit controleert:

    ```
    $ cd hosts
    bash: cd: hosts: Not a directory
    $ cat hosts
    cat: hosts: Permission denied
    ```

4. Kan de eigenaar de permissies wijzigen? Controleer.

    ```
    $ chmod 744 hosts
    $ ls -l
    -rwxr--r--. 1 student student 158 2016-11-23 14:09 hosts
    ```

5. Kan de eigenaar het bestand verwijderen? Controleer.

    ```
    $ rm hosts
    $ ls
    vanmij
    ```

Kopieer als je eigen gebruiker (niet als root!) nu opnieuw het bestand `/etc/hosts` in het directory `oefenenMetPermissies` en pas de permissies opnieuw aan zoals eerder voorgeschreven. Zorg dat gebruiker `alice` lid is van de groep die groepseigenaar is van het bestand `hosts` in directory `oefenenMetPermissies`. Switch nu naar gebruiker alice.

1. Kan alice nu het bestand bekijken? Controleer.

    ```
    $ sudo chown student:sporten hosts
    $ sudo usermod -a -G sporten alice
    $ cat /home/student/oefenenMetPermissies/hosts
    ```

2. Kan alice de permissies wijzigen? Controleer.

    ```
    $ chmod 075 /home/student/oefenenMetPermissies/hosts
    chmod: changing permissions of '/home/student/oefenenMetPermissies/hosts': Operation not permitted
    chmod: changing permissions of "...": Operation not permitted
    ```

3. Kan alice het bestand verwijderen? Is dit de bedoeling? Controleer.

    ```
    $ rm /home/student/oefenenMetPermissies/hosts
    rm: cannot remove '/home/student/oefenenMetPermissies/hosts': Permission denied
    ```

4. Stel nu de sticky-bit in op het directory oefenenMetPermissies. Geef het geschikte commando en controleer het resultaat.

    ```
    $ chmod o+t hosts
    $ stat -c "%a" hosts
    1065
    ```
    
    T -> niet actief
    t -> actief

## Eigenaars en groepseigenaars veranderen

1. Maak als `root` onder `/srv/` twee directories aan met de naam `groep/verkoop` en `groep/inkoop`. Maak ook 2 groepen aan met de namen `verkoop` en `inkoop`. Maak twee gebruikers aan, `margriet` met primaire groep `verkoop` en `roza`, die als primaire groep `inkoop` heeft. Zorg dat de groepen eigenaar zijn van de overeenkomstige directories en dat `margriet` eigenaar is van directory `verkoop` en `roza` van het directory `inkoop`. Geef de gebruikte commando’s en controleer:

    ```
    $ mkdir -p groep/{verkoop,inkoop}
    $ groupadd verkoop
    $ groupadd inkoop
    $ useradd roza
    $ useradd margriet
    $ gpasswd -a margriet verkoop
    $ gpasswd -a roza inkoop
    $ chown roza:inkoop inkoop
    $ chown margriet:verkoop verkoop
    ...
    # ls -l groep/
    total 0
    drwxr-xr-x. 2 roza     inkoop  6 Sep 22 22:23 inkoop
    drwxr-xr-x. 2 margriet verkoop 6 Sep 22 22:23 verkoop
    ```

2. Zorg ervoor dat gebruikers en groepen uit de vorige stap alle permissies hebben. Geef het geschikte commando en controleer.

    ```
    $ chmod 770 inkoop
    $ chmod 770 verkoop
    ```

3. Voeg een gebruiker, vb `alice`, toe aan de groep `inkoop` en `verkoop` en controleer. Geen van beide groepen zijn primair.

    ```
    $ gpasswd -a alice inkoop
    ```

4. Log in als `alice` en ga naar de directory verkoop. Laat de gebruiker hier een leeg bestand, `bestand1`, aanmaken in de directory verkoop. (Indien je hier problemen ondervindt, log dan in via een andere terminalvenster).

    ```
    $ touch /srv/groep/inkoop/bestand1
    UITVOER
    ```

5. Wie is nu eigenaar van `bestand1` en wie de groepseigenaar?

    ```
    $ ls -l
    alice alice
    ```

6. Zorg er nu voor dat de groepseigenaar van de directory `verkoop` automatisch de groepseigenaar wordt van alle bestanden en directories die onder `verkoop` gemaakt worden. Geef de gebruikte commando’s.

    ```
    $ COMMANDO
    UITVOER
    ```

7. Doe hetzelfde voor de directory `inkoop`. Geef de gebruikte commando’s:

    ```
    $ COMMANDO
    UITVOER
    ```

8. Verander opnieuw naar gebruiker `alice` en laat deze gebruiker een leeg `bestand2` aanmaken in de directory `verkoop`. Geef de gebruikte commando’s:

    ```
    $ COMMANDO
    UITVOER
    ```

9. Wie is nu eigenaar van `bestand2` en wie groepseigenaar?

    ```
    $ COMMANDO
    UITVOER
    ```

10. Laat nu gebruiker `margriet` een leeg bestand `bestand3` aanmaken. Controleer de eigenaar van `bestand3` en de groepseigenaar.

    ```
    $ COMMANDO
    UITVOER
    ```

11. Laat nu gebruiker `alice` `bestand3` verwijderen. Lukt dit?

    ```
    $ COMMANDO
    UITVOER
    ```

12. Zorg er nu voor dat de gebruikers elkaars bestanden niet kunnen verwijderen. Als de gebruiker echter eigenaar is van het betreffende directory mag dit wel. Leg uit hoe je dit doet en controleer. Schrijf je gevolgde procedure op.

    ```
    $ COMMANDO
    UITVOER
    ```

## Gebruikte bronnen
