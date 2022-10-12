# VirtualBox Ubuntu Server aufsetzen 

## Vorbereitung 

  * Ubuntu Server 22.04 LTS - ISO herunterladen

## Schritt 1: Virtuelle Maschine erstellen 

```
In VirtualBox Manager -> Menu -> Maschine -> Neu (Oder Neu icon) 

Seite 1:
Bei Name Ubuntu Server eingeben (dadurch wird gleich das richtige ausgewählt, bei den Selects) 
Alles andere so lassen.
Weiter 

Seite 2: 
Hauptspeicher mindest 4 GB , d.h. 4096 auswählen
Weiter

Seite 3:
Festplatte erzeugen ausgewählt lassen
Weiter

Seite 4: 
Dateityp der Festplatte: VDI ausgewählt lassen
Weiter 

Seite 5:
Art der Speicherung -> dynamisch alloziert ausgewählt lassen
Weiter 

Seite 6:
Dateiname und Größe -> bei Größe mindestens 30 GB einstellen (bei Bedarf größer) 
Erzeugen 
```

## Schritt 2: ISO einhängen / Netzwerk und starten / installieren

```
Neuen Server anklicken und ändern klicken:

1. 
Massenspeicher -> Controller IDE -> CD (Leer) klicken
CD - Symbol rechts neben -> Optisches Laufwerk (sekundärer Master) -> klicken -> Abbild auswählen
Downgeloadetes ISO Ubuntu 22.04 auswählen -> Öffnen klicken 

2. 
Netzwerk -> Adapter 2 (Reiter) anklicken -> Netzwerkadapter aktivieren
Angeschlossen an -> Host-only - Adapter 

3. 
unten rechts -> o.k. klicken 

```

## Schritt 3: Starten klicken und damit Installationsprozess beginnen

```
Try or install Ubuntu Server -> ausgewählt lassen

Seite 1:
Use up -.... Select your language 
-> English lassen
Enter eingeben

Seite 2: Keyboard Configuration
Layout auswählen (durch Navigieren mit Tab-Taste) -> Return
German auswählen (Pfeiltaste nach unten bis German, dann return)
Identify Keyboard -> Return
Keyboard Detection starting -> Ok 
Jetzt die gewünschten tasten drücken und Fragen beantworten
Layout - Variante bestätigen mit OK 

-> Done 

Seite 3: Choose type of install 
Ubuntu - Server ausgewählt lassen

-> Done 

Seite 4: Erkennung der Netzwerkkarten 
(192.168.56.1x) sollte auftauchen

-> Done 

Seite 5: Proxy

leer lassen

-> Done 

Seite 6: Mirror Address

kann so bleiben

-> Done 

Seite 7: 

Guided Storage konfiguration
Entire Disk 

-> Done 

Seite 8: File System Summary

-> Done 

Seite 9: Popup: Confirm destructive action 
Bestätigen, dass gesamte Festplatte überschrieben wird
(kein Problem, da Festplatte ohnehin leer und virtuell)

-> Continue 

Seite 10: Profile Setup

User eingeben / einrichten 
Servernamen einrichten 

-> Done 

Seite 11: SSH Setup 

Haken bei: Install OpenSSH Server 
setzen

-> Done 

Seite 12: Featured Server Snaps 

Hier brauchen wir nichts auswählen, alles kann später installiert werden

-> Done 

Seit 13:  Installation

Warten bis Installation Complete und dies auch unten angezeigt wird:
(es dauert hier etwas bis alle Updates (unattended-upgrades) im Hintergrund durchgeführt worden sind) 

-> Reboot Now 

Wenn "Failed unmounting /cdrom" kommt 
dann einfach Server stoppen
-> Virtual Box Manager -> Virtuelle Maschine auswählen -> Rechte Maustaste -> Schliessen ->  Ausschalten 

```

## Schritt 4: Starten des Gast-Systems in virtualbox 

```
* Im VirtualBox Manager auf virtuelle Maschine klicken
* Neben dem Start - Pfeil -> Dreieck anklicken und Ohne Gui starten wählen 
* System startet dann im Hintergrund (kein 2. Fenster)
```

## Erklärung 

 * Console wird nicht benötigt, da wir mit putty (ssh) arbeiten zum Administrieren des Clusters
 * Putty-Verbindung muss nur auf sein, wenn wir administrieren
 * Verwendung des Clusters (nutzer/Entwickler) erfolgt ausschliesslich über kubectl in powershell !! 
