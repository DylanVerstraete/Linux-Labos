# Labo 1: Linux installeren

## VirtualBox installeren en een nieuwe VM aanmaken

1. Download en installeer de laatste versie van VirtualBox. Als je werkt op een van de klaspc's (campus Gent), dan is VirtualBox al geïnstalleerd.
2. Maak een nieuwe virtuele machine aan om straks Linux in te installeren. Let er op dat je volgende instellingen voorziet:
    - Als je op een klaspc werkt is de naam van je VM je eigen naam, bv. PietPieters
    - Het type van de VM is “Linux”, de versie “Fedora (64 bit)”
    - RAM: minstens 1024MB
    - Virtuele harde schijf van ~20GB, dynamisch
    - Videogeheugen: zoveel mogelijk (128MB)
    - 3D acceleration aanzetten
    - Maak twee virtuele netwerkadapters aan, één van het type NAT (= standaardkeuze), en één van het type "Host only"

### Procedure

1. start VirtualBox op, klik op 'nieuw', selecteer als type 'linux' en als versie 'Fedora (64-bit), klik op volgende.
2. selecteer RAM, dan volgende, dan create (als optie 'create a new harddisk' aangeduid staat'), opnieuw volgende (VDI staat aangeduid), dan volgende (dynamisch gealloceerd), kies een schijfgrootte, dan create.
3. ga naar settings > display, zet 'video memory' op 128Mb, en vink '3D acceleration' aan
4. ga naar settings > Network, kijk of adapter1 op 'enable' staat, en van type NAT is.
5. nog steeds in Network, ga naar de tab 'adapter2', vink 'enable' aan, verander 'attached to' naar 'Host only', onder naam moet er nu een naam staan vb 'vbox1', als dat niet zo is, volg stap 5a
    5a. ga in VirtualBox naar File > preferences > network > Host-only network en klik op 'maak niew host-only netwerk' (icoon met groene plus)

## Linux installeren in een VirtualBox VM

1. Download de laatste [Fedora Live Desktop CD](https://getfedora.org/en_GB/workstation/download/). Start je VM op met deze LiveCD als opstartschijf (het is niet nodig een fysieke cd te branden, je kan een VM rechtstreeks van de ISO booten). Let op dat je met heel de klasgroep niet het draadloos netwerk satureert...
2. Wijzig na opstarten voor het gemak de toetsenbordinstelling naar Belgische Azerty
3. Installeer Fedora naar de harde schijf van je VM (opm. dit is een volkomen veilige operatie en heeft geen enkele invloed op je hostsysteem).
    - Kies de juiste tijdzone, toetsenbordindeling en taal (bij voorkeur Engels)
    - Kies de voorgestelde partitionering van de harde schijf
    - Stel het wachtwoord van de “root” gebruiker in. LET OP: ben je zeker dat je in Azerty-indeling aan het typen bent? TIP: noteer dit wachtwoord onder “Description” in de instellingen van je VirtualBox VM. Als je het vergeet kan je het makkelijk terugvinden.
4. Herstart je VM in het geïnstalleerde systeem. Maak een nieuwe gebruiker aan voor jezelf en voeg die toe aan de “Administrators” groep. In Linux is de conventie om *enkel kleine letters* te gebruiken voor gebruikersnamen.
5. Zoek je weg in het nieuwe systeem.
    - Kan je een webbrowser opstarten? Kan je op het Internet?
    - Kan je een teksteditor (Gnome Editor of Gedit) opstarten?
    - Kan je een tekstverwerker (LibreOffice Writer) opstarten?
    - Kan je de inhoud van je “thuismap” tonen in het Linux-equivalent van Windows Verkenner?
    - Kan je een “terminal-venster” openen?
    - Kan je de screensaver uitschakelen?

**Opmerking** Als je een nieuw Linux-systeem installeert, krijg je al gauw de melding dat er heel wat updates beschikbaar zijn. Het is af te raden om dit in het klaslokaal te doen, opnieuw om de netwerkverbinding naar buiten niet te satureren. Doe dit dus liefst thuis.

### Procedure

Beschrijf hier de exacte procedure hoe je dit uitgevoerd hebt. Zorg er voor dat je aan de hand van je beschrijving deze taken later heel vlot kan herhalen als dat nodig is.

1. selecteer je machine, ga naar settings > storage, onder 'controller : IDE' (maak aan als nodig door onderaan op 'make new controller' te klikken (icoon met plus, niet floppydisk icoon), klik naast controller:IDE op de cd met een plus, kies voor 'choose disk', navigeer naar de map met je fedora ISO.
2. start de machine op, doorloop de installatieprocedure
3. sluit de machine opnieuw af, ga dan naar settings > Storage en verwijder onder controller:IDE de ISO.
4. start de machine opnieuw op, log in met wachtwoord

## Nieuwe software installeren

Installeer onderstaande applicaties of “packages”. Zorg er voor dat je dit zowel via de grafische gebruikersinterface kan als vanop de command-line.

- git
- nano
- ShellCheck
- vim-enhanced
- vim-X11

Installeer optioneel de “VirtualBox Guest Additions” (zie procedure in de [studiewijzer](../studiewijzer/installatie.md)). Creëer een lokale kopie van je Github repository.

### Procedure

Beschrijf hier de exacte procedure hoe je dit uitgevoerd hebt. Zorg er voor dat je aan de hand van je beschrijving deze taken later heel vlot kan herhalen als dat nodig is.

1. open de terminal
2. enter volgende commando's:
  1. sudo dnf install nano
  2. sudo dnf install git
  3. sudo dnf install vim-enhanced
  4. sudo dnf install ShellCheck
  5. sudo dnf install vim-X11

## Gebruikers en groepen aanmaken

Het doel van deze opgave is om de opdrachten en de begrippen met betrekking tot  gebruikers en groepen te bestuderen, binnen de context van Linux als een multi-user-systeem.

Tussen de vragen is ruimte voorzien om je antwoorden in te vullen. Het gaat telkens om zgn. codeblokken in Markdown, die starten en eindigen met drie backquotes. Binnen elk codeblok geef je telkens het commando dat je hebt ingetikt en de uitvoer die je krijgt.

1. Je hebt al een gebruiker aangemaakt voor jezelf. Log in als deze gebruiker. Geef hieronder telkens het commando en de uitvoer
    - Wat is het commando om de huidige directory op te vragen? Welke uitvoer toont het commando?

        ```
        $ pwd
        /home/student
        ```

    - Wat is het UID (user id) van deze gebruiker?

        ```
        $ id -u
        1000
        ```

    - Wat is het GID (group id) van deze gebruiker?

        ```
        $ id -g
        1000
        ```

2. Log in als de `root`-gebruiker met het commando `su -` (let op de spatie!)
    - Wat is de home-directory van `root`?

        ```
        $ cd ~
        $ pwd
        /root
        ```

    - Wat is het UID van deze gebruiker?

        ```
        $ id -u
        0
        ```

    - Wat is het GID van deze gebruiker?

        ```
        $ id -g
        0
        ```

3. Maak een nieuwe gebruiker aan met de naam `alice`, zonder specifieke opties
    - Geef het gebruikte commando:

        ```
        $ useradd alice
        ```

    - Voorzie een geschikt wachtwoord (gekozen voor 'student alice' voor deze gebruiker (en vergeet het niet! Noteer het eventueel hier in je verslag of in de beschrijving van je VM)

        ```
        $ passwd alice
        > student alice
        ```

4. Configuratiebestanden voor gebruikersbeheer:
    - In welk bestand kan je de UID, gebruikersnaam, homedirectory, enz. van alle gebruikers terugvinden?

        ```
        /etc/passwd
        ```

    - In welk configuratiebestand kan je al de bestaande gebruikersgroepen nakijken, en ook de gebruikers die lid zijn van elke groep?

        ```
        /etc/group
        ```

    - In welk configuratiebestand vind je de *wachtwoorden* van alle gebruikers?

        ```
        /etc/shadow
        ```

5. Gebruikersgroepen aanmaken
    - Maak een groep aan met de naam `sporten`

        ```
        $ groupadd sporten
        ```

    - In welk configuratiebestand vind je het GID van deze groep terug?

        ```
        $ cat /etc/group | grep 'sporten'
        sporten:x:1002:
        ```

    - Wat zal het GID zijn van de groepen `zwemmen` en `judo` als je deze nu onmiddellijk zou aanmaken? Maak ze aan en controleer!

        ```
        $ cat /etc/group | grep zwemmen
        zwemmen:x:1003
        $ cat /etc/group | grep judo
        judo:x:1004
        ```

    - Voeg de gebruiker `alice` toe aan de groepen `sporten` en `zwemmen`

        ```
        $ gpasswd -a alice sporten
        Adding user alice to group sporten
        $ gpasswd -a alice zwemmen
        Adding user alice to group zwemmen
        ```

    - Log in als `alice` door in een terminal het commando `su - alice` (let op de spaties!) uit te voeren

        ```
        $ su - alice
        ```

    - Zorg er nu voor dat de groep `sporten` de primaire groep wordt van `alice`.

        ```
        $ su
        $ sudo usermod -g sporten alice
        ```

    - Zorg er voor dat `alice` uitgelogd is, ga terug naar `root`

        ```
        $ pkill -KILL -u alice
        $ su
        ```

6. Maak nu de gebruikers in onderstaande tabel aan. Zorg er voor dat ze al meteen bij aanmaken tot de aangegeven groepen behoren. Kies zelf geschikte wachtwoorden voor deze gebruikers en vergeet ze niet (vul eventueel een kolom toe aan de tabel).

    | Gebruikersnaam | Primaire groep | Secundaire groep | Wachtwoord |
    | :---           | :---           | :---             | :---       |
    | `bob`          | `sporten`      | `judo`           |            |
    | `carol`        | `sporten`      | `zwemmen`        |            |
    | `daniel`       | `sporten`      | `judo`           |            |
    | `eva`          | `sporten`      | `zwemmen`        |            |

    - Geef de gebruikte commando's om de gebruikers aan te maken en ook om te verifiëren of dit correct gebeurd is:

        ```
        $ useradd -G sporten,judo -p lazercar bob
        $ cat /etc/passwd | grep 'bob'
        $ useradd -G sporten,zwemmen -p circusgiraffe carol
        $ useradd -G sporten,judo -p zeverman daniel
        $ useradd -G sporten,zwemmen -p lolz eva
        ...
        ```

    - Verwijder nu de *groep* `alice` en controleer.

        ```
        $ groepdel alice
        ???
        ```

    - Gebruiker `daniel` gaat een tijdje niet meer sporten. Zorg er voor dat deze gebruiker tot nader order geen toegang meer kan hebben tot het systeem (zonder het wachtwoord of de gebruiker te verwijderen!).

        ```
        $ passwd daniel -l
        Locking password for user daniel
        passwd: Success
        ```

    - Hoe kan je controleren dat `daniel` inderdaad geen toegang meer heeft tot het systeem? In welk bestand kan dat en hoe zie je daar dan dat het account afgesloten is?

        ```
        $ 
        $ cat /etc/shadow | grep 'daniel'
        -> ! voor wachtwoord
        ```

    - Gebruiker `daniel` komt terug naar de sportclub. Geef hem opnieuw toegang tot het systeem.

        ```
        $ passwd daniel -u
        ```

    - Gebruiker `eva` stopt helemaal met sporten. Verwijder deze gebruiker, maar doe dit zorgvuldig: zorg er in het bijzonder voor dat ook haar homedirectory verwijderd wordt.

        ```
        $ userdel -r eva
        $ su - eva
        su: user eva does not exist
        ```

7. Log aan als de gebruiker `carol`

    ```
    $ su - carol
    
    ```

    - Controleer of je in de “thuismap” bent van deze gebruiker. Maak onder het directory `Documenten/` een bestand `test` aan door middel van het commando `touch`.

        ```
        $ touch Documenten/test
        ```

    - Probeer nu als gebruiker `carol` je te verplaatsen naar de “thuismap” van `alice`.

        ```
        $ cd /home/alice
        -bash: cd: /home/alice: Permission denied
        ```

    - Kan je de inhoud van de mappen binnen de thuismap van `alice` bekijken?

        ```
        $ ls /home/alice
        ls: cannot open directory '/home/alice': Permission denied
        ```

    - Probeer nu als `carol` onder het directory `Documenten/` in de “thuismap” van `alice` ook een bestand `test` te maken. Lukt dit? Kan je dit verklaren?

        ```
        $ touch /home/alice/test
        touch: cannot touch 'home/alice/test': Permission denied
        ```

