# M300 - Plattformübergreifende Dienste in ein Netzwerk integrieren Alex Treichler

### Inhaltsverzeichnis

* [1. Ziele](#1.Ziele)
* [2. Voraussetzungen](#2.Voraussetzungen)
* [3. Applikationen](#3.Applikationen)
* [4. Lernumgebung](#4.Lernumgebung)
* [5. Cloud_Vagrant](#5.CloudVagrant)
* [6. Konfiguration](#6.Konfiguration)
* [7. Sicherheit](#7.Sicherheit)


## 1.Ziele

In der bereits erstellten VM einen Container erstellen in welchem Sinatra läuft.


## 2.Voraussetzungen

* PC mit min. 8 GB freiem RAM und ca. 20 GB freiem Harddisk (Ressourcen der VMs können angepasst werden).
* Software Packer
* VirtualBox

<b> Modul </b>     | Plattformübergreifende Dienste in ein Netzwerk integrieren
-------------------|---------------------------------------------------------------------------------------------------------------------------------------
<b> Kompetenz </b> | Plattformübergreifende Dienste nach Vorgabe für eine heterogene Systemumgebung konfigurieren, in Betrieb nehmen, testen und freigeben.


## 3.Applikationen 
  - **-Installiert und Funktionsfähig-**
  
<tab>    | <tab>
-------------------|---------------------------------------------------------------------------------------------------------------------------------------              
**Vagrant:**    
![Programmbilder](\Bilder\Programme_Vagrant.PNG)
 
1. Visual Studio Code<br>   ![Programmbilder](\Bilder\Programme_Visual_Studio_Code.PNG)
2. VirtualBox<br>
   ![Programmbilder](\Bilder\Programme_VirtualBox.PNG)
3. GIT<br>   
   ![Programmbilder](\Bilder\Programme_GIT.PNG)
4. SSH Key<br>
   ![Programmbilder](\Bilder\SSH_Key.PNG)
         
## 4.Lernumgebung 
  - **-Installiert und Funktionsfähig-**
  
<tab>    | <tab>
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------
**GITHUB**   | ![Programmbilder](\Bilder\GITHUB.PNG)
**Dokumentation ist als Mark Down**        | JA
**Mark down ist strukturiert** | Meiner eigenen Meinung Nach ist es Strukturiert.

## 5.CloudVagrant 


<tab>    | <tab>
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Bestehende vm aus Vagrant-Cloud einrichten** | ![Programmbilde_r](\Bilder\Vagrant_VM.PNG)

-------------------
  
## 6.Konfiguration
### Packer
Mit Vagrant können virtuelle Maschinen vollständig automatisiert erstellt werden. Es werden jegendlich einige Konfigurationsdateien benötigt. All dies wird in diesem Kapitel beschrieben. :
#### Verzeichnisse

Folgende Verzeichnisse werden werden für Packer benötigt:

<tab>    | <tab>
--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------
**Ordner**   | Verwendung
scripts        | Alle Scripts, welche nach der erfolgreichen Installation des Betriebssystems durchlaufen müssen, müssen in diesem Ordner abgelegt sein.
iso       | Das ISO File wird in dieses Verzeichnis heruntergeladen. Alternativ kann das ISO File auch in dieser Verzeichnis kopiert werden. 
http| Die Datei "preseed.cfg" muss hier abgelegt sein. Diese beinhaltet weitere Konfigurationselemente.

#### Konfigurationsdateien
##### Install.json
Diese Datei beinhaltet alle wichtigen Konfigurationen. Unter dem Abschnitt "boot_command" werden alle Einstellungen definiert, welche dem Ubuntu mitgegeben werden(Keyboardlayout, Zeitzone, User etc.). 
Der Abschnitt "builders" beinhaltet alle weiteren Konfigurationen, wie z.B. die ISO Datei oder der SSH User. 
Unter "vboxmanage" werden alle Einstellungen definiert, welche Virtualbox benötigt (Anzahl CPUs, RAM).
In diesem File muss im Abschnitt "provisioners" alle Bash Files angegeben werden, welche nach der Installation ausgeführt werden müssen. 
##### Preseed.cfg
Diese Datei beinhaltet sämtliche Konfigurationen, welche für die Ubunut Installation benötit werden. Hier werden zum Beispiel die benötigten Partitionen erstellt. Ebenfalls werden hier noch die User erstellt und auch die zu installierenden Pakete definiert. 
##### Setup.sh
Diese Datei wird nach der Installation des Systems ausgeführt. 
Zuerst wird der User "vagrant" in die Gruppe sudoers hinzugefügt. Danach wird das System aktualisiert und die Firewall Software ufw installiert und konfiguriert. Anschliessend wird der Apache Reverse Proxy installiert und konfiguriert. 
### Virtualbox
Unter Virtualbox müssen keine weiteren Einstellungen getätigt werden. 
### configure-sinatra-app.sh
Diese Datei sollte inerhalb des Containers sinatra installieren, welches ein sehr einfacher Webservice ist. den habe ich gewählt weil dieser einfach zu installieren ist und auch keine hohen anforderungen hat.
### Docker-compose.yml
das installierte Ruby wird nun konfiguriert und mit dem app.rb verknüpft so das der Server den inhalt anzeigt.
### app.rb
Inhalt für Webservice

## 7.Sicherheit
Wähend der Installation werden mehrere sicherheitsrelevante Einstellungen getätigt. Diese werden hier aufgelistet:
### Firewall
Mit dem Skript "setup.sh wird die Firewall UFW installiert. Anschliessend werden die Ports 22 und 80 (TCP) geöffnet.
### Apache2 Proxy
Zuerst werden die benötigten Pakete installiert und diese auch aktiviert. 
Danach wird das File "001-reverseproxy.conf" erstellt und anschliessend auch mit den Einstellungen befüllt. 
Danach werden die Services noch neu gestartet. 
