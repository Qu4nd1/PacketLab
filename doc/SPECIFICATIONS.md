# Spécifications – PacketLab

**Version :** 0.1 (brouillon)
**Auteur :** *(à compléter)*
**Statut :** En cours de rédaction

---

## 1. Contexte et objectifs

### 1.1 Contexte
PacketLab est un projet d'apprentissage dont le but est de découvrir les bases de la capture et de l'analyse de paquets réseau (« packet sniffing »). Il part de zéro connaissance et progresse par étapes, de l'observation avec des outils existants jusqu'à un analyseur de paquets en ligne de commande développé from scratch.

### 1.2 Objectifs
- **Pédagogique :** comprendre le modèle TCP/IP, la structure des trames Ethernet, des paquets IP, TCP, UDP, ainsi que les protocoles DNS et HTTP.
- **Technique :** développer un outil capable de capturer, décoder, filtrer et résumer du trafic réseau.
- **Portfolio :** produire un dépôt GitHub documenté et présentable.

### 1.3 Public cible
Principalement l'auteur ; secondairement, tout débutant souhaitant suivre une progression similaire.

---

## 2. Cadre légal et éthique

- La capture de trafic est **uniquement autorisée sur des réseaux dont l'utilisateur est propriétaire ou pour lesquels il dispose d'une autorisation explicite**.
- Les tests sont réalisés dans un environnement contrôlé (machine virtuelle, réseau domestique personnel, serveur de test local).
- Le projet ne doit pas inclure de fonctionnalités d'attaque (usurpation ARP, injection, déchiffrement de trafic tiers, etc.).
- Les captures (`.pcap`) contenant des données personnelles ne sont **jamais** versionnées dans le dépôt.

---

## 3. Périmètre

### 3.1 Inclus
- Capture en direct sur une interface réseau.
- Décodage des couches : Ethernet, IPv4 (IPv6 en option), TCP, UDP, ICMP, DNS.
- Filtrage par protocole, adresse, port.
- Statistiques et rapport de synthèse.
- Lecture et écriture de fichiers `.pcap`.
- Interface en ligne de commande (CLI).

### 3.2 Exclu
- Interface graphique.
- Déchiffrement TLS/HTTPS.
- Capture en mode « monitor » Wi-Fi.
- Fonctionnalités offensives (voir section 2).
- Analyse en temps réel haute performance (> 1 Gbit/s).

---

## 4. Exigences fonctionnelles

| ID | Exigence | Priorité |
|----|----------|----------|
| F1 | Lister les interfaces réseau disponibles | Haute |
| F2 | Capturer des paquets en direct sur une interface choisie | Haute |
| F3 | Afficher un résumé d'une ligne par paquet (horodatage, source, destination, protocole, taille) | Haute |
| F4 | Décoder manuellement les en-têtes Ethernet et IPv4 à partir des octets bruts | Haute |
| F5 | Décoder les en-têtes TCP, UDP et ICMP | Moyenne |
| F6 | Filtrer par protocole, adresse IP et port | Haute |
| F7 | Limiter la capture (nombre de paquets ou durée) | Moyenne |
| F8 | Produire un rapport : paquets par protocole, hôtes les plus actifs, ports les plus utilisés | Moyenne |
| F9 | Enregistrer une capture au format `.pcap` | Moyenne |
| F10 | Lire et analyser un fichier `.pcap` existant | Moyenne |
| F11 | Journaliser les requêtes DNS observées | Basse (fonctionnalité bonus) |
| F12 | Signaler le trafic non chiffré (ex. HTTP) sur un serveur de test | Basse (fonctionnalité bonus) |
| F13 | Détecter un balayage de ports simple | Basse (fonctionnalité bonus) |

*Au moins une fonctionnalité bonus (F11 à F13) doit être réalisée pour considérer le projet comme terminé.*

---

## 5. Exigences non fonctionnelles

- **Lisibilité :** code commenté, structuré en modules, respect de PEP 8.
- **Portabilité :** fonctionne sous Linux et macOS ; Windows en option (nécessite Npcap).
- **Robustesse :** les paquets malformés ou tronqués ne doivent pas faire planter l'outil.
- **Sécurité :** exécution avec les privilèges minimaux nécessaires ; documentation claire de l'usage de `sudo`.
- **Documentation :** README en anglais, spécifications en français, notes d'apprentissage dans `docs/`.
- **Tests :** tests unitaires sur le décodage des en-têtes à l'aide de paquets d'exemple.

---

## 6. Choix techniques

| Élément | Choix | Justification |
|---------|-------|---------------|
| Langage | Python 3.10+ | Syntaxe accessible, bonne documentation |
| Capture | Scapy (puis `socket` brut pour le décodage manuel) | Prise en main rapide, puis compréhension bas niveau |
| CLI | `argparse` | Bibliothèque standard |
| Tests | `pytest` | Simple et répandu |
| Outil d'observation | Wireshark | Référence pour valider les résultats |
| Versionnement | Git / GitHub | Portfolio |

*(Ces choix peuvent être modifiés avant l'étape 3.)*

---

## 7. Architecture prévue

```
packetlab/
├── README.md
├── SPECIFICATIONS.md
├── requirements.txt
├── docs/
│   └── notes/              # notes d'apprentissage par étape
├── src/packetlab/
│   ├── cli.py              # point d'entrée, gestion des arguments
│   ├── capture.py          # capture en direct / lecture pcap
│   ├── parsers/            # décodage Ethernet, IP, TCP, UDP, ICMP, DNS
│   ├── filters.py          # logique de filtrage
│   ├── stats.py            # statistiques et rapport
│   └── output.py           # formats d'affichage
├── tests/
└── samples/                # captures d'exemple anonymisées
```

---

## 8. Plan de réalisation (jalons)

| Étape | Contenu | Livrable | Critère de validation |
|-------|---------|----------|-----------------------|
| 0 | Installation de l'environnement | Wireshark et Python opérationnels | L'interface réseau est visible dans Wireshark |
| 1 | Concepts théoriques | Notes personnelles | Capacité à expliquer le parcours d'une requête web |
| 2 | Observation avec Wireshark | Notes sur ping, DNS, HTTP | Le handshake TCP est repéré dans une capture |
| 3 | Première capture en code | Script de résumé par paquet | Affichage source / destination / protocole |
| 4 | Décodage manuel des en-têtes | Module `parsers` | Résultats identiques à Wireshark |
| 5 | Filtrage et statistiques | Rapport de session | Comptage par protocole et top hôtes |
| 6 | Fichiers `.pcap` | Lecture et écriture | Un fichier généré s'ouvre dans Wireshark |
| 7 | Fonctionnalité bonus | Au choix F11, F12 ou F13 | Démonstration sur environnement de test |
| 8 | Finalisation | CLI, tests, README, spécifications | Dépôt propre et documenté |

---

## 9. Critères d'acceptation

Le projet est considéré comme terminé lorsque :
1. Toutes les exigences de priorité **Haute** et **Moyenne** sont implémentées.
2. Au moins une fonctionnalité bonus est livrée.
3. La sortie de l'outil est cohérente avec Wireshark sur au moins 5 paquets de types différents.
4. Les tests unitaires passent.
5. Le README permet à un tiers d'installer et d'utiliser l'outil sans aide.

---

## 10. Risques et contraintes

| Risque | Impact | Mitigation |
|--------|--------|------------|
| Droits administrateur requis pour la capture | Bloquant | Utiliser une VM Linux ; documenter `sudo` |
| Difficulté à comprendre les en-têtes binaires | Retard | Valider chaque champ avec Wireshark |
| Trafic HTTPS illisible | Frustration | Utiliser un serveur HTTP local de test |
| Fichiers `.pcap` contenant des données privées | Fuite de données | `.gitignore` sur `*.pcap`, échantillons anonymisés |
| Dispersion du périmètre | Projet inachevé | Respecter l'ordre des étapes ; bonus en dernier |

---

## 11. Glossaire

- **Paquet :** unité de données transmise sur un réseau.
- **Trame :** paquet encapsulé au niveau de la couche liaison (Ethernet).
- **Sniffing :** capture passive du trafic réseau.
- **PCAP :** format de fichier de capture réseau.
- **Handshake TCP :** échange SYN, SYN-ACK, ACK qui établit une connexion TCP.
- **Interface réseau :** point d'accès de la machine au réseau (ex. `eth0`, `en0`).
