---
layout: [post, post-xml]
title: "Linux für Entwickler 01: Navigation in der Kommandozeile"
date:   2023-12-30 19:02
modified_date: 2023-01-04 19:23
author_ids: [kenneth-may-adesso]
categories: [Architektur]
tags: [DevOps, Basics, Essentials, Linux]
---

In der Welt der Softwareentwicklung ist Linux ein unverzichtbares Werkzeug.
Linux bietet Flexibilität, Leistungsfähigkeit und die Freiheit, das System nach eigenen Vorstellungen anzupassen - Schlüsselkompetenzen für jeden Entwickler.
In jedem meiner Projekte war es bislang notwendig, ein System zu konfigurieren.
Diese Artikelreihe richtet sich an Softwareentwickler und andere Interessierte, die sich mit den Grundlagen von Linux vertraut machen möchten. 
Sei es um ein eigenes Entwicklungssystem zu konfigurieren oder eine breitere Wissensverteilung im eigenen Projekt zu erreichen.

# Teil 1: Das Linux-Dateisystem

## Grundverständnis und Unterschiede zu Windows

Das Linux-Dateisystem unterscheidet sich grundlegend von dem unter Windows. 
Windows organisiert physische Festplatten in der Regel in Laufwerken wie C:, D: usw.
Linux dagegen organisiert alles in einer Verzeichnishierarchie, die bei einem einzigen Wurzelverzeichnis, dem sogenannten Root-Verzeichnis (`/`), beginnt.
Alle anderen Laufwerke und Partitionen werden in diesen Verzeichnisbaum eingehängt.
Tatsächlich geht Linux hier noch einen Schritt weiter: Selbst Hardware und Kernel-Konfiguration existieren als Datei.

Neben Root gibt es noch das Homeverzeichnis. 
Dieses liegen in der Regel unter `/home/<Benutzername>`.
Dort werden die benutzerspezifischen Dateien und Konfigurationen gespeichert.
Dieses Prinzip dürfte aus Windows bekannt sein, es hat eine vergleichbare Struktur.
Und wie auch bei Windows, sind die Berechtigungen eingeschränkt.
Hier gibt es aber noch eine kleine Ausnahme, die ich nicht unerwähnt lassen möchte.
Der Root-Benutzer hat ebenfalls ein Home-Verzeichnis.
Um die Sonderstellung von Root zu unterstreichen, liegt das nicht unter `/home/<Benutzername>`, sondern direkt unter `/root` 

Neben Root und Home gibt es auch weitere, spezielle Verzeichnisse.
Um die Einführung für euch übersichtlich zu halten, werden diese in einem späteren Artikel beschrieben.

> ### Wichtig für den weiteren Inhalt
> Alle Pfade sind relativ zum aktuellen Pfad, außer der Verweis beginnt mit einem `/` für das Root Verzeichnis, dann ist der Pfad absolut.
> Wenn ihr aber bereits mit der Powershell oder der Eingabeaufforderung gearbeitet habt, dann kennt ihr das bereits.
 
## Hands-On

Gerade am Anfang fällt es leichter, wenn man die Befehle selbst ausprobieren kann.
Da kommt uns Docker besonders gelegen.
Wenn ihr bereits eine laufende Docker-Instanz habt, beispielsweise über DockerDesktop könnt ihr einfach einen neuen Container starten und als Image `ubuntu:latest` auswählen.
Sobald ihr euch mit dem Container verbindet, könnt ihr die Befehle live miterleben.

Für die Windowsnutzer unter euch - Windows bietet mit WSL (Windows Subsystem for Linux) bereits mit Bordmitteln eine Shell (Bash).
Für die Applenutzer ist es noch einfacher: OS X bringt direkt zsh als Shell mit.

## Wichtige Befehle zur Navigation und Dateiverwaltung

1. **cd (Change Directory)**

   Der Befehl `cd` wird verwendet, um zwischen Verzeichnissen zu wechseln.

   #### Beispiel: 
   `cd /home/benutzername` wechselt in das Benutzerverzeichnis.
   Dazu kommen die speziellen Ordnernamen `.` und `..`.
   `.` steht für das aktuelle Verzeichnis, während `..` das übergeordnete Verzeichnis meint.
   Dadurch ergeben sich besondere Varianten von cd.

   `cd .` weißt das System an, in das aktuelle Verzeichnis zu wechseln.
   Den Sinn dahinter zu finden, überlasse ich euch.
   Genutzt wird `.` vor allem, wenn Programme einen Pfad benötigen, wir uns aber bereits dort befinden.
   Ein Beispiel hierfür ist `docker build .`, da der Buildprozess ein Kontextverzeichnis erwartet.

   `cd ..` weißt das System an, in das Elternverzeichnis zu wechseln.
   Wenn man das, wie im obigen Beispiel in unserem Home-Verzeichnis (`/home/benutzername/`) ausführt, landen wir in `/home`.
   
   `cd` (man beachte das Fehlen eines Zieles) ist eine Kurzform, um in das eigene Homeverzeichnis (`/home/<Benutzername>`) zu wechseln.

2. **mkdir (Make Directory)**  

   Mit `mkdir` erstellt man ein neues Verzeichnis.
   Verzeichnisse gehören während der generierung dem aktuellen Benutzer und dessen Hauptgruppe.
   Wenn der Name mit einem `.` beginnt, dann ist es ein verstecktes Verzeichnis.

   Beispiel: `mkdir neuesVerzeichnis` erstellt ein Verzeichnis namens `neuesVerzeichnis` im aktuellen Verzeichnis.

   Beispiel: `mkdir /.neuesVersteck` erstellt ein verstecktes Verzeichnis namens `.neuesVersteck` im Root-Verzeichnis.
   Je nachdem, wie ihr die Befehle gerade ausprobiert, werdet ihr hier eine Fehlermeldung bekommen.
   Das Root Verzeichnis gehört dem Benutzer Root und seiner Gruppe Root (kreativ nicht?) und ihr seid unter Umständen gerade nicht root.
   Linux wird euch also freundlich darauf hinweisen, dass euch die Berechtigung fehlt.

3. **rmdir (Remove Directory)**

   Dieser Befehl entfernt ein leeres Verzeichnis.
   Leer bedeutet tatsächlich leer denn auch versteckte Dateien oder Unterordner verhindern das Löschen.

   Beispiel: `rmdir altesVerzeichnis` löscht `altesVerzeichnis`, vorausgesetzt, es ist leer.

4. **touch**

   `touch` wird oft genutzt, um schnell leere Dateien zu erstellen oder das letzte Modifikationsdatum zu aktualisieren.
   Wichtig ist, dass Dateiendungen bei Linux nur einen bedingten Zweck haben.
   Wenn man über einen Dateimanager eine Datei öffnet, dann wird die Endung zwar unter Umständen herangezogen, sollte sie aber anders lauten, spielt das oft eher eine untergeordnete Rolle.
   Touch ist hier ein gutes Beispiel: wenn eine leere Datei erstellt wird, ist sie Binär-Leer.
   Erstellt man eine `datei.pdf`, hat man keine PDF mit einer leeren Seite.

   > **Exkurs**
   > 
   > Der Unterschied können wir unter Windows sehen, wenn man eine leere TXT Datei erstellt (bspw. datei.txt) und sie im Nachgang zu datei.nfo umbenennt.
   > Wenn man die Datei dann öffnet, wird Windows sich beschweren, dass die Datei fehlerhaft ist und eine sehr detailierte Hardware und Softwareliste anzeigen.
   
   Wenn der Dateiname mit einem `.` beginnt, gilt die Datei als versteckt.

   Beispiel: `touch datei.txt` erstellt eine leere Datei namens `datei.txt`.

   Beispiel: `touch .versteckt.txt` erstellt eine versteckte, leere Datei namens `.versteckt.txt`
5. **rm (remove)**

   Mit `rm` werden Dateien unwiederbringlich gelöscht.
   Aufgrund der Natur des unwiederbringlichen löschens warnt und Linux beim Löschen und hinterfragt, ob wir das wirklich wollen.
   Außer man ist Root.
   Ohne den expliziten Hinweis kann rm aber keine Verzeichnisse löschen: `rm: test/: is a directory`.
   
   Beispiel: `rm datei.txt` löscht die im vorherigen Schritt erstellte Datei.
   Beispiel: `rm .versteckt.txt` löscht die im vorherigen Schritte erstellte versteckte Datei.

6. **ls (List)**

   `ls` listet den Inhalt eines Verzeichnisses auf.
   `ls` ohne Argumente listet keine versteckten Verzeichnisse.
   Dafür wird die Option `-a` oder `-A` genutzt.
   `-a` listet sämtliche Dateien und Verzeichnisse im aktuellen Verzeichnis, versteckt oder nicht.
   `-A` macht fast das gleiche wie `-a`, ignoriert aber die beiden speziellen Verzeichnisse `.` und `..`.
   Ein weiterer interessanter Parameter ist `-l`, welcher auch Metadaten zu den Dateien listet.

   Beispiel: `ls /home` zeigt die Inhalte des Home-Verzeichnisses.
   
   ```
   $~> ls -lA
   total 28
   -rw------- 1 may admin   31 Nov 29 12:20 .bash_aliases
   -rw------- 1 may admin 1872 Dec 20 17:58 .bash_history
   -rw------- 1 may admin  220 Apr 23  2023 .bash_logout
   -rw------- 1 may admin 3526 Apr 23  2023 .bashrc
   -rw------- 1 may admin    0 Nov 26 05:21 .cloud-locale-test.skip
   -rw------- 1 may admin   20 Dec 20 14:58 .lesshst
   -rw------- 1 may admin  807 Apr 23  2023 .profile
   drwx------ 2 may admin 4096 Nov 29 12:20 .ssh
   -rw------- 1 may admin    0 Nov 29 12:28 .sudo_as_admin_successful
   ```
   Der Befehl listet uns das aktuelle Verzeichnis und zeigt auch versteckte Dateien aber ignoriert `.` und `..`.
   Weiterhin werden folgende Metainformationen zu den Dateien angezeigt:
   1. Type, Berechtigungen und Stickybits (Wobei vorerst nur die Berechtigungen interessant sind)
   2. Hardlinks auf die Datei
   3. der Eigentümer
   4. die Gruppe
   5. die Dateigröße in Bytes
   6. der Zeitpunkt der letzten Änderung (wir erinnern uns an touch)
   7. der Dateiname
   
   Wie wir im Beispiel sehen war ich sogar gezwungen, `-a` oder `-A` zu verwenden.
   Mein Home-Verzeichnis besitzt nur versteckte Dateien und ist ansonsten leer, eher schlecht für die Demonstration von `ls`.
   Was aber macht versteckte Dateien aus?
   Dazu mehr im nächsten Abschnitt.

7. **man (Manual)**  
   Dies ist ein sehr nützlicher Befehl, um Hilfe zu anderen Befehlen zu erhalten.
   Solange man allgemeine Befehle verwendet, kann man sich einfach die "Handbuchseite" anschauen und Informationen zum Befehl finden.
   Das wird häufig dann genutzt, wenn man die Argumente für einen Befehl durchsuchen möchte.
   In den Hilfeseiten gibt es am Ende häufig Referenzen auf andere Seiten mit weiterführenden Informationen. 

   Beispiel: `man ls` zeigt das Handbuch für den Befehl `ls`.

# Besonderheiten im Linux-Dateisystem: Versteckte Dateien und Berechtigungen

## Berechtigungen

Das Berechtigungssystem in Linux ist ein wesentliches Merkmal für die Sicherheit.
Jede Datei und jedes Verzeichnis hat Berechtigungen, die bestimmen, wer die Datei lesen, schreiben oder ausführen darf.
Diese Berechtigungen sind aufgeteilt in drei Kategorien:

- **Besitzer*in** (bei einer neu erstellten Datei/einem neu erstellten Ordner der Ersteller*in)
- **Gruppe** (alle Benutzer*innen, die der gleichen Gruppe angehören)
- **Andere** (alle anderen Benutzer*innen)

Die Berechtigungen werden oft in der Form `rwx` dargestellt, wobei `r` für Lesen (read), `w` für Schreiben (write) und `x` für Ausführen (execute) steht.
Mit dem Befehl `ls -l` kann man diese Berechtigungen anzeigen lassen (siehe oben).

## Erweiterte Erklärung zu `chmod`: Oktalzahlen und symbolische Notation

### Oktalzahlen in `chmod`

Bei der Verwendung von `chmod` können Berechtigungen auch mit Oktalzahlen (Basis 8) festgelegt werden.
Jede Ziffer repräsentiert einen Satz von Berechtigungen: Lesen (4, das dritte Bit), Schreiben (2, das zweite Bit) und Ausführen (1, das erste Bit).
Die Summe dieser Werte definiert die Berechtigungen für eine Kategorie (Besitzer*in, Gruppe, Andere).

Beispiel:
- `chmod 754 datei.txt` setzt die Berechtigungen so, dass der oder die Besitzer*in alle Berechtigungen hat (7), die Gruppe lesen und ausführen kann (5) und andere Benutzer*innen nur lesen dürfen (4).

### Symbolische Notation in `chmod`

Alternativ zur Oktalnotation könnt ihr auch symbolische Notation verwenden, was vor allem für Neulinge intuitiver sein sollte:

- `u` steht für Besitzer*in (user),
- `g` für Gruppe (group),
- `o` für Andere (others),
- `a` für alle (all).

Wir können Berechtigungen hinzufügen (`+`), entfernen (`-`) oder genau festlegen (`=`).

Dazu gehören dann die folgenden Berechtigungen:

- `r` steht für lesen (read)
- `w` steht für schreiben (write)
- `x` steht für ausführen (executable)
- `X` behält die Ausführrechte für Dateien und, wofür es besonders nützlich ist, setzt sie neu nur für Verzeichnisse!

Beispiele:
- `chmod u+w datei.txt` fügt dem oder der Besitzer*in Schreibberechtigung hinzu.
- `chmod g=rX datei.txt` setzt die Gruppenberechtigungen auf Lesen und, falls es sich um ein Verzeichnis handelt oder die Ausführungsberechtigung bereits gesetzt ist, auch auf Ausführen.
- `chmod g=rX unterverzeichnis/` setzt die Gruppenberechtigung für das Unterverzeichnis auf "lesen" und gibt der Gruppe die Berechtigung, das Verzeichnis zu betreten.
- `chmod ug+rX,o= -R unterverzeichnis/` fügt Gruppen und Benutzer*innen die Leseberechtigung hinzu und, sofern es sich um ein Verzeichnis handelt, darf dieses auch betreten werden.
  Für alle anderen werden die Berechtigungen entfernt, Oktal 0.
  Und schlussendlich mit `-R` führen wir die Änderungen Rekursiv für alle Unterdateien und Verzeichnisse aus.

### id

Der Befehl `id` zeigt die Benutzer*innen-ID und die Gruppen-ID des oder der aktuellen Benutzer*in an.
Das ist hilfreich, um zu verstehen, unter welchen Benutzer- und Gruppenberechtigungen man arbeitet.
Dazu kommt, dass man sehen kann, was die Hauptgruppe (gid) ist.

Beispiel:
- `id` zeigt die Benutzer-ID, Gruppen-ID und die Zugehörigkeit zu anderen Gruppen des aktuellen Benutzers.

### chown

Der Befehl `chown` (change owner) ändert den oder die Besitzer*in und/oder die Gruppe einer Datei oder eines Verzeichnisses.
Dies ist nützlich, um Zugriffsrechte zu verwalten.

Beispiel:
- `chown benutzer datei.txt` ändert den Besitzer der `datei.txt` in `benutzer` und lässt die Gruppe unangetastet.
- `chown benutzer: datei.txt` ändert den Besitzer der `datei.txt` in `benutzer` und die Gruppe zu der Hauptgruppe des Benutzers.
- `chown benutzer:gruppe datei.txt` ändert sowohl den Besitzer als auch die Gruppe.

# Zusammenfassung

Wir haben bislang die wichtigsten Befehle für die Dateiverwaltung kennengelernt.
Wir können mit `cd`, `mkdir` und `rmdir` in Verzeichnisse wechseln, sie erstellen oder löschen.
Mit `touch`, `rm` und `ls` können wir Dateien öffnen und löschen und sehen, was die Verzeichnisse beinhalten.
`man` hilft uns, weiterführende Informationen zu Befehlen zu finden.
Schlussendlich haben wir noch die Befehle `chmod` und `chown` kennengelernt und können mithilfe von `id` Informationen zum oder zur eigenen Benutzer*in sehen.
Die Kenntnis und Anwendung dieser Befehle und Konzepte ermöglicht eine fein abgestimmte Kontrolle über Datei- und Verzeichnisberechtigungen in Linux.
Dies ist besonders wichtig in Entwicklungsumgebungen, wo der Zugriff auf bestimmte Dateien und Verzeichnisse sorgfältig verwaltet werden muss.
Mit der Praxis wird diese Komplexität handhabbar und ein mächtiges Werkzeug in den Händen eines erfahrenen Entwicklers.

Für Fragen oder Anredungen könnt ihr mich unter kenneth.may@adesso.de erreichen.

# Ausblick

Im nächsten Artikel wird die Installation von neuen Programmen unter Linux besprochen.