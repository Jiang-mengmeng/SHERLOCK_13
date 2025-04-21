# Sherlock 13

## Description

Ce mini-projet simule le jeu Sherlock 13 avec quatre joueurs. Les règles complètes sont disponibles dans le PDF : [SHERLOCK_13.pdf](./SHERLOCK_13.pdf).

## Fonctionnalités

### Côté joueur
- **C** : Inscription et connexion au serveur (`C <IP> <port> <nom>`)
- **O** : Enquête « objet » – demande à tous les autres joueurs s'ils possèdent un symbole spécifique (`O <id_joueur> <id_objet>`)
- **S** : Enquête « standard » – demande à un joueur ciblé le nombre exact d'un symbole (`S <id_joueur> <id_ciblé> <id_objet>`)
- **G** : Accusation – accusation contre un suspect (`G <id_joueur> <id_suspect>`)

### Côté serveur
- **I** : Attribution d’un identifiant unique au joueur (`I <id>`)
- **L** : Diffusion de la liste des joueurs connectés (`L <nom1> <nom2> <nom3> <nom4>`)
- **D** : Distribution des cartes aux joueurs (`D <cartes_joueur>`)
- **M** : Notification du joueur courant (`M <id_joueur>`)
- **V** : Réponse à une enquête (valeur d'un symbole) (`V <id_joueur> <valeur>`)
- **F** : Échec d'une accusation (`F <id_joueur>`)
- **W** : Victoire d'un joueur (`W <id_gagnant>`)

### Fin de la partie
un joueur gagne immédiatement sur une accusation correcte (W) ou lorsqu’un troisième joueur a échoué, le dernier joueur restant remporte la partie.

### Aspects techniques
- **Réseau TCP/IP** : communication client-serveur basée sur des sockets POSIX (socket, bind, listen, accept, connect, read/write)
- **Machine à états** : le serveur utilise une FSM (`fsmServer`) pour suivre les phases de connexion, de jeu et de fin
- **Threads** : le serveur utilise des threads pour gérer les connexions multiples, chaque joueur est traité dans un thread indépendant permettant un fonctionnement concurrent du jeu
- **Mutex (verrous)** : des `pthread_mutex_t` sont utilisés pour protéger les ressources critiques et éviter les race conditions
- **Parsing et protocole textuel** : commandes textuelles analysées avec `sscanf()` et `switch-case`
- **SDL2** : le client graphique utilise SDL2 pour la fenêtre, le rendu et la gestion des événements utilisateur

*Les détails techniques sont expliqués en profondeur dans le [rapport du projet](./Rapport_Projet_OS_User_-_SHERLOCK_13.pdf).*

## Prérequis
- GCC (ou autre compilateur C)
- SDL2 (pour le client graphique)
- SDL2_image et SDL2_ttf
- Environnement Unix/Linux ou WSL

## Structure du projet
```
SHERLOCK_13/
├── SHERLOCK_13.pdf
├── src/
│   ├── server.c
│   └── sh13.c
├── assets/
│   ├── fonts/
│   │   └── sans.ttf                # Police TTF
│   └── images/
│       ├── SH13_0.png              # Cartes
│       ├── SH13_pipe_120x120.png   # Objets
│       └── ...                     # Autres images
├── bin/          # Dossier pour les exécutables
├── cmd.sh        # Script de compilation
└── README.md
```

## Implémentation

### Compilation
Exécuter le script de compilation :
```bash
chmod +x cmd.sh
./cmd.sh
```
### Exécution
1. Démarrer le serveur (exemple sur le port 32000) :
```bash
./bin/server 32000
```
2. Démarrer les clients pour chaque joueur (terminaux séparés) :
```bash
./bin/sh13 localhost 32000 localhost 32001 Anna
./bin/sh13 localhost 32000 localhost 32002 Bob
./bin/sh13 localhost 32000 localhost 32003 Cathy
./bin/sh13 localhost 32000 localhost 32004 David
```
3. Les joueurs interagissent via l’interface SDL pour enquêter et accuser.

## Améliorations Futures
- Mode expert (règles avancées pour positionnement et valeurs)
- Interface graphique plus riche (icônes, animations)
- Gestion réseau plus robuste (reconnexion, timeouts)

## Contact
Pour toute question ou suggestion concernant ce projet, vous pouvez me contacter :  
- **Email** : [jiangmengmeng1211@gmail.com](mailto:jiangmengmeng1211@gmail.com)  
- **GitHub** : [GitHub Repository](https://github.com/Jiang-mengmeng/SHERLOCK-13)  

> Ce projet a été réalisé dans le cadre du module **Systèmes d'exploitation - OS User**. Une partie du code a été fournie comme base, tandis que d'autres parties ont été laissées à compléter par les étudiants. Le travail présenté ici est donc un effort collaboratif s'appuyant sur une structure initiale commune à tous les groupes.
