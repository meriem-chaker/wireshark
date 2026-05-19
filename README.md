\# Wireshark Project



\## Introduction



Dans ce projet, nous avons découvert et utilisé Wireshark, un analyseur réseau permettant d’écouter les trames et paquets circulant sur un réseau.



L’objectif était de comprendre le fonctionnement des protocoles réseau, le modèle OSI, l’analyse des paquets, les filtres Wireshark et l’utilisation de Tshark en ligne de commande.



\---



\# Présentation de Wireshark



Wireshark est un analyseur de trafic réseau open source.



Il permet :



\* d’écouter le trafic réseau en temps réel ;

\* d’analyser les protocoles ;

\* de filtrer les paquets ;

\* de diagnostiquer des problèmes réseau ;

\* d’observer les échanges entre machines.



Wireshark fonctionne sous Linux, Windows et macOS.



\---



\# Différence entre une trame et un paquet



Une trame correspond aux données transmises au niveau de la couche liaison du modèle OSI (Ethernet).

Elle contient notamment les adresses MAC source et destination.



Un paquet correspond aux données de la couche réseau (IP).

Il contient les adresses IP source et destination.



La trame encapsule généralement un paquet IP.



\---



\# Format pcap / pcapng



Les formats `.pcap` et `.pcapng` sont des formats de fichiers utilisés pour sauvegarder des captures réseau.



Ils permettent d’enregistrer les paquets capturés par Wireshark ou Tshark afin de les analyser plus tard.



Le format `.pcapng` est plus récent et permet de stocker davantage d’informations comme les interfaces réseau ou les commentaires.



\---



\# Installation de Wireshark



Installation sous Debian :



```bash

apt install wireshark -y

```



Ajout de l’utilisateur au groupe Wireshark :



```bash

/usr/sbin/usermod -aG wireshark maria

```



Lancement de Wireshark :



```bash

wireshark

```



\---



\# Interface réseau utilisée



Interface utilisée pour les captures :



```text

ens33

```



Adresse IP observée :



```text

192.168.200.133

```



\---



\# Modèle OSI et désencapsulation



Lors de l’analyse des paquets, Wireshark permet de visualiser les différentes couches du modèle OSI.



Exemple observé :



```text

Ethernet

└── IP

&#x20;   └── TCP

&#x20;       └── HTTPS

```



Cela correspond à l’encapsulation des données réseau.



\---



\# Capture ARP



Filtre utilisé :



```text

arp

```



Le protocole ARP permet d’associer une adresse IP à une adresse MAC.



Exemple observé :



\* MAC source

\* MAC destination

\* Adresse IP demandée



\---



\# Capture UDP



Filtre utilisé :



```text

udp

```



UDP est un protocole rapide mais non fiable.



Il est utilisé notamment pour :



\* DNS

\* streaming

\* jeux vidéo



\---



\# Capture TCP



Filtre utilisé :



```text

tcp

```



TCP est un protocole fiable permettant :



\* le contrôle des erreurs ;

\* la retransmission ;

\* le maintien de l’ordre des données.



\---



\# SYN / ACK / FIN



Filtres utilisés :



```text

tcp.flags.syn == 1

```



```text

tcp.flags.fin == 1

```



Le protocole TCP utilise un mécanisme appelé « Three-way Handshake ».



\## Diagramme TCP



```text

CLIENT                        SERVEUR



SYN ------------------------->



&#x20;         <------------------- SYN ACK



ACK ------------------------->



Connexion établie



FIN ------------------------->



&#x20;         <------------------- ACK



Connexion fermée

```



\---



\# Adresses observées



\## Adresses MAC



Exemple :



```text

00:0c:29:d3:1a:78

00:50:56:ea:6e:48

```



\## Adresses IP



Exemple :



```text

192.168.200.133

142.250.151.113

```



\---



\# Analyse hexadécimale



Wireshark permet de visualiser les paquets en hexadécimal.



Exemple :



```text

45 00 00 3c

```



L’hexadécimal représente les données binaires d’un paquet réseau.



\---



\# DNS



Filtre utilisé :



```text

dns

```



Le protocole DNS permet de traduire un nom de domaine en adresse IP.



Exemple :



```text

google.com -> 142.250.x.x

```



\---



\# DHCP



Filtre utilisé :



```text

dhcp

```



DHCP (Dynamic Host Configuration Protocol) permet d’attribuer automatiquement une adresse IP aux machines d’un réseau.



Les principaux messages DHCP sont :



\* Discover

\* Offer

\* Request

\* ACK



\---



\# mDNS



Filtre utilisé :



```text

mdns

```



mDNS (Multicast DNS) permet la découverte automatique des appareils sur un réseau local.



Il est utilisé notamment pour :



\* AirDrop

\* imprimantes réseau

\* appareils connectés



\---



\# FTP



Filtre utilisé :



```text

ftp

```



FTP est un protocole de transfert de fichiers non sécurisé.



Un serveur FTP a été installé avec :



```bash

apt install vsftpd -y

```



Connexion FTP réalisée :



```bash

ftp 192.168.200.133

```



Conclusion :



Les échanges FTP ne sont pas chiffrés.

Il est donc possible d’intercepter certaines informations réseau.



\---



\# HTTPS / TLS



Filtres utilisés :



```text

https

```



```text

tls

```



TLS permet de chiffrer les communications réseau.



Contrairement à FTP, les données HTTPS/TLS ne sont pas lisibles directement dans Wireshark.



\---



\# SMB



Filtre utilisé :



```text

smb

```



SMB (Server Message Block) est un protocole principalement utilisé sous Windows pour le partage de fichiers et d’imprimantes.



\---



\# Filtres Wireshark utilisés



| Filtre             | Fonction                   |

| ------------------ | -------------------------- |

| arp                | Afficher les paquets ARP   |

| tcp                | Afficher les paquets TCP   |

| udp                | Afficher les paquets UDP   |

| dns                | Afficher les requêtes DNS  |

| dhcp               | Afficher les échanges DHCP |

| mdns               | Afficher les échanges mDNS |

| ftp                | Afficher les paquets FTP   |

| tls                | Afficher les paquets TLS   |

| tcp.flags.syn == 1 | Afficher les SYN           |

| tcp.flags.fin == 1 | Afficher les FIN           |



\---



\# Tshark



Tshark est la version en ligne de commande de Wireshark.



\## Affichage des interfaces



```bash

tshark -D

```



\## Capture DNS



```bash

tshark -i ens33 -f "port 53"

```



\## Capture TCP



```bash

tshark -i ens33 -f "tcp"

```



\## Sauvegarde d’une capture



```bash

tshark -i ens33 -w /tmp/capture.pcapng

```



\## Explication des options



| Option | Fonction                |

| ------ | ----------------------- |

| -D     | Afficher les interfaces |

| -i     | Choisir l’interface     |

| -f     | Ajouter un filtre       |

| -w     | Sauvegarder une capture |



\---



\# Sécurité réseau



L’analyse réseau permet de détecter des problèmes de sécurité.



Exemple observé :



\* FTP transmet les données sans chiffrement ;

\* HTTPS/TLS chiffre les données ;

\* les filtres permettent d’isoler les paquets importants.



\---



\# Conclusion



Ce projet a permis de découvrir :



\* le fonctionnement des réseaux ;

\* le modèle OSI ;

\* les protocoles TCP/IP ;

\* l’analyse réseau avec Wireshark ;

\* les filtres réseau ;

\* Tshark ;

\* les notions de sécurité réseau.



Wireshark est un outil essentiel pour l’analyse, le diagnostic et la sécurité des réseaux.



\---



\# Présentation orale / Diapo



\## Slide 1 — Titre



Wireshark Project

Analyse réseau et protocoles



\---



\## Slide 2 — Objectif du projet



\* Découvrir Wireshark

\* Capturer des paquets réseau

\* Comprendre TCP/IP

\* Utiliser des filtres

\* Utiliser Tshark



\---



\## Slide 3 — Modèle OSI



\* Ethernet

\* IP

\* TCP / UDP

\* DNS / HTTPS



\---



\## Slide 4 — Protocoles étudiés



\* ARP

\* TCP

\* UDP

\* DNS

\* FTP

\* TLS

\* DHCP



\---



\## Slide 5 — TCP Handshake



SYN

SYN ACK

ACK

FIN



\---



\## Slide 6 — Wireshark



\* Capture réseau

\* Filtres

\* Analyse hexadécimale

\* Désencapsulation



\---



\## Slide 7 — Tshark



\* Ligne de commande

\* Filtres réseau

\* Sauvegarde pcapng



\---



\## Slide 8 — Sécurité réseau



\* FTP non sécurisé

\* TLS chiffré

\* Analyse réseau importante



\---



\## Slide 9 — Conclusion



\* Compréhension des protocoles réseau

\* Utilisation de Wireshark et Tshark

\* Découverte de la cybersécurité réseau



