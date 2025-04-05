# Backend_Go-Rust

## Introduction
Ce dépôt contient plusieurs projets backend développés en Go et Rust. Chaque projet couvre des fonctionnalités spécifiques et utilise des technologies modernes pour offrir des solutions robustes et évolutives.

## Table des matières
1. [Projet 1 : API CRUD complète](#projet-1-api-crud-complète)
2. [Projet 2 : Système d’authentification](#projet-2-système-dauthentification)
3. [Projet 3 : Application de chat en temps réel](#projet-3-application-de-chat-en-temps-réel)
4. [Projet 4 : Tableau de bord dynamique](#projet-4-tableau-de-bord-dynamique)
5. [Projet 5 : Service de gestion de fichiers](#projet-5-service-de-gestion-de-fichiers)
6. [Projet 6 : Optimisation de la performance](#projet-6-optimisation-de-la-performance)
7. [Projet 7 : Automatisation de tâches](#projet-7-automatisation-de-tâches)
8. [Projet 8 : Intégration d’API tierces](#projet-8-intégration-dapi-tierces)
9. [Projet 9 : Gestion de Webhooks](#projet-9-gestion-de-webhooks)
10. [Projet 10 : Architecture microservices](#projet-10-architecture-microservices)

## Projet 1 : API CRUD complète

### Contexte
Tu travailles pour une startup qui développe une application de gestion de tâches personnelles. Tu es chargé de créer une API qui permettra à l'application frontend d'ajouter, lister, modifier et supprimer des tâches.

### Objectifs
- Créer une API RESTful.
- Implémenter les opérations CRUD.
- Gérer les statuts des tâches (à faire, en cours, terminé).
- Ajouter des timestamps (création et mise à jour).

### Stack recommandée
- **Langage** : Go (Gin) ou Node.js (Express)
- **Base de données** : PostgreSQL ou MongoDB
- **Outils** : Postman, Docker (optionnel)

### Fonctionnalités
- `GET /tasks` : récupérer toutes les tâches.
- `GET /tasks/:id` : récupérer une tâche par ID.
- `POST /tasks` : ajouter une nouvelle tâche.
- `PUT /tasks/:id` : modifier une tâche.
- `DELETE /tasks/:id` : supprimer une tâche.

### Étapes de développement
1. **Initialiser le projet**
   - Création du dépôt, configuration de Gin/Express.
2. **Modèle de données**
   - Structure de la tâche : id, title, description, status, createdAt, updatedAt.
3. **Connexion à la base de données**
   - Création de la table/collection.
4. **Création des routes**
   - Implémentation de chaque endpoint.
5. **Tests avec Postman**
   - Tester tous les cas (valide, erreur, non trouvé).
6. **(Bonus) Ajout de validation**
   - Empêcher les titres vides, etc.

---

## Projet 2 : Système d’authentification

### Contexte
Tu dois sécuriser une application permettant aux utilisateurs de créer des comptes, de se connecter et de gérer leur profil. En plus du système classique, tu veux offrir la possibilité de se connecter via Google (OAuth2).

### Objectifs
- Authentification avec email/mot de passe (via JWT).
- Connexion via Google (OAuth2).
- Sécurisation des routes privées.

### Stack recommandée
- **Langage** : Node.js (Express) ou Go (Gin)
- **Auth** : JWT, OAuth2 (Google)
- **Base de données** : PostgreSQL / MongoDB

### Fonctionnalités
- `POST /register` : inscription d’un utilisateur.
- `POST /login` : connexion avec retour d’un token JWT.
- `GET /me` : récupérer le profil de l’utilisateur connecté.
- `GET /auth/google` : démarrer l’authentification Google.
- `GET /auth/google/callback` : callback de Google.

### Étapes de développement
1. **Initialiser le projet**
   - Créer un backend de base.
2. **Création du modèle utilisateur**
   - id, email, password, provider, createdAt.
3. **Mise en place du registre/login**
   - Hachage du mot de passe (bcrypt).
   - Génération de JWT.
4. **Middleware d’authentification**
   - Vérification du token dans les headers.
5. **Connexion Google**
   - Utiliser l’API Google (client ID / secret).
   - Gérer le callback et récupérer les infos utilisateur.
6. **Tests des routes sécurisées**
   - Protéger certaines routes.

---

## Projet 3 : Application de chat en temps réel

### Contexte
Tu développes une messagerie pour une application communautaire. Les utilisateurs doivent pouvoir envoyer et recevoir des messages instantanément, sans avoir besoin de rafraîchir la page.

### Objectifs
- Mettre en place un serveur WebSocket.
- Permettre à plusieurs utilisateurs de discuter en temps réel.
- Stocker les messages dans une base de données.
- Gérer les connexions et déconnexions.

### Stack recommandée
- **Backend** : Go (Gorilla WebSocket) ou Node.js (Socket.io)
- **Frontend** : HTML/JS simple ou Angular/React
- **Base de données** : MongoDB ou PostgreSQL

### Fonctionnalités
- Connexion WebSocket par utilisateur.
- Envoi / réception de messages en temps réel.
- Affichage des messages historiques à l'ouverture du chat.
- Notification d’utilisateur en ligne / hors ligne (optionnel).

### Étapes de développement
1. **Initialiser le projet avec serveur WebSocket**
   - Créer un endpoint ws://.../chat.
2. **Gérer les connexions**
   - Sauvegarder les connexions actives par utilisateur.
3. **Gérer l’envoi de message**
   - Quand un message est reçu, l’envoyer au destinataire connecté.
   - Sauvegarder le message en base (champ : sender, receiver, content, timestamp).
4. **Récupérer l’historique**
   - Quand un utilisateur se connecte, charger ses messages récents.
5. **Déconnexion et gestion des erreurs**
   - Fermer proprement les connexions et retirer de la liste active.

---

## Projet 4 : Tableau de bord dynamique

### Contexte
Tu es en charge d’un outil interne pour surveiller l’état des serveurs d’une entreprise. L’équipe technique a besoin d’un tableau de bord en temps réel affichant les stats de CPU, RAM, etc.

### Objectifs
- Collecter des métriques système régulièrement.
- Les envoyer en temps réel via WebSocket à un frontend.
- Afficher ces données de manière visuelle.

### Stack recommandée
- **Backend** : Go ou Node.js + WebSocket
- **Monitoring** : lib système (ex : os / sysinfo)
- **Frontend** : React / Angular + Chart.js / D3.js

### Fonctionnalités
- Affichage en direct de la charge CPU, RAM, etc.
- Graphiques mis à jour automatiquement.
- Historique conservé temporairement (en mémoire ou base).
- Détection de surcharge ou anomalies (optionnel).

### Étapes de développement
1. **Écriture du module de récupération des stats système**
   - Exemple Go : github.com/shirou/gopsutil
2. **Mise à jour régulière via goroutines ou cron interne**
   - Envoi des données chaque 5 secondes par WebSocket.
3. **Création d’un frontend dynamique**
   - Connexion à la WebSocket.
   - Utilisation de graphiques pour les données.
4. **Ajout d’alertes visuelles**
   - Couleurs ou icônes en cas de dépassement de seuil.
5. **(Bonus) Historique et export des données**

---

## Projet 5 : Service de gestion de fichiers

### Contexte
Tu développes un service qui permet aux utilisateurs d'uploader des fichiers (images, PDF, docs...), qui seront ensuite stockés dans un répertoire local ou dans le cloud. Les images doivent être compressées automatiquement et des fichiers PDF doivent être générés à partir de données utilisateur.

### Objectifs
- Uploader et stocker des fichiers.
- Compresser les images automatiquement.
- Générer des fichiers PDF à partir de données utilisateur.
- Option : Intégrer le stockage cloud (S3 ou autre).

### Stack recommandée
- **Backend** : Go (Gin) ou Node.js (Express)
- **Libs** :
  - Compression images : Go → github.com/nfnt/resize ou imaging, Node → sharp
  - PDF : Go → gofpdf, Node → pdfkit
  - Cloud : AWS SDK, Google Cloud Storage, etc.
- **Base de données** : pour stocker les chemins des fichiers

### Fonctionnalités
- `POST /upload` : uploader un fichier.
- Compression automatique des images.
- Génération de PDF avec un `POST /generate-pdf` en fournissant des données JSON.
- Téléchargement avec `GET /files/:id`.

### Étapes de développement
1. **Configuration du serveur pour gérer les uploads**
   - Middleware multipart/form-data.
2. **Stockage des fichiers**
   - En local ou dans un bucket cloud.
3. **Compression des images**
   - Dès qu’un fichier image est uploadé.
4. **Génération de PDF**
   - À partir d’un contenu dynamique (profil utilisateur, facture...).
5. **Téléchargement sécurisé**
   - Vérification des droits d'accès avant l’envoi.

---

## Projet 6 : Optimisation de la performance

### Contexte
Ton serveur backend commence à avoir du trafic. Il est temps d’optimiser les performances pour tenir la charge : mise en cache, compression, équilibrage de charge, etc.

### Objectifs
- Ajouter un système de cache.
- Activer la compression des réponses HTTP.
- Simuler un environnement à forte charge.
- (Bonus) Utiliser un reverse proxy comme Nginx.

### Stack recommandée
- **Backend** : Go ou Node.js
- **Cache** : Redis ou en mémoire
- **Compression** : gzip middleware
- **Test de charge** : Apache Benchmark, K6, Locust

### Fonctionnalités
- Mise en cache des réponses d’API lentes.
- Compression gzip automatique.
- Mesure des temps de réponse avec et sans optimisations.

### Étapes de développement
1. **Mettre en place un cache simple (en mémoire ou Redis)**
   - Par exemple, cache des résultats de /products.
2. **Activer la compression gzip**
   - Middleware dans Express ou Gin.
3. **Test de charge avant optimisation**
   - Pour obtenir un point de comparaison.
4. **Implémenter les optimisations**
   - Ajouter cache, compression, limiter les logs inutiles.
5. **Test de charge après optimisation**
   - Comparer les gains obtenus.
6. **(Bonus) Ajouter Nginx en reverse proxy avec gzip, keep-alive, etc.

---

## Projet 7 : Automatisation de tâches

### Contexte
Ton application a besoin d’effectuer certaines actions régulièrement : envoyer des emails, supprimer des données obsolètes, générer des rapports… Tu dois mettre en place un système de Cron Jobs.

### Objectifs
- Planifier l’exécution automatique de fonctions.
- Gérer la périodicité (chaque jour, chaque heure…).
- Logger les actions exécutées.

### Stack recommandée
- **Backend** : Go ou Node.js
- **Lib Cron** :
  - Go : robfig/cron
  - Node : node-cron ou agenda
- **Logs** : fichiers log ou base de données

### Fonctionnalités
- Lancement d’un job de nettoyage des utilisateurs inactifs.
- Envoi d’un rapport quotidien par email.
- Création d’un log des exécutions.

### Étapes de développement
1. **Configurer la lib de Cron**
   - Définir une tâche chaque X minutes/secondes.
2. **Créer une fonction cible**
   - Exemple : suppression des utilisateurs inactifs.
3. **Logger l'exécution**
   - Stocker la date, l'action effectuée, le résultat.
4. **Créer d’autres tâches automatisées**
   - Ex : sauvegarde de la base, alertes d’anomalies.
5. **Tester la planification**
   - Démarrer l’appli et observer les tâches.

---

## Projet 8 : Intégration d’API tierces

### Contexte
Tu développes une app qui doit récupérer des infos depuis des services externes (ex. météo, prix crypto, ou traduction de texte). Tu dois apprendre à faire des appels API externes, parser les réponses, et utiliser ces données.

### Objectifs
- Utiliser http client pour faire des appels API.
- Gérer les tokens d’authentification si nécessaire.
- Stocker ou afficher les données récupérées.

### Stack recommandée
- **Backend** : Go (net/http) ou Node.js (axios, fetch)
- **API à intégrer** :
  - Météo : OpenWeatherMap
  - Crypto : CoinGecko ou Binance
  - Traduction : DeepL, Google Translate API

### Fonctionnalités
- Endpoint : `/weather?city=Dakar` → données météo.
- Endpoint : `/crypto?symbol=BTC` → prix en temps réel.
- Endpoint : `/translate?text=Bonjour&to=en` → traduction.

### Étapes de développement
1. **Choisir une API publique (gratuite de préférence)**
   - Créer une clé si nécessaire.
2. **Faire un appel HTTP GET vers cette API**
   - Gérer les headers, tokens, etc.
3. **Parser et formater la réponse**
   - Extraire uniquement ce qui t’intéresse.
4. **Exposer les données via ton propre backend**
   - Créer des endpoints /weather, /crypto, /translate.
5. **(Bonus) Cacher les résultats pendant 1min avec un cache**

---

## Projet 9 : Gestion de Webhooks

### Contexte
Tu travailles sur une application qui doit réagir à des événements venant d'autres services (comme Stripe, GitHub, ou ton propre microservice). Ces services envoient des webhooks : des requêtes HTTP POST contenant des informations sur l'événement.

### Objectifs
- Créer un endpoint pour recevoir les webhooks.
- Sécuriser les données (signature HMAC, token secret…).
- Traiter et logger les événements reçus.

### Stack recommandée
- **Backend** : Go ou Node.js
- **Signature HMAC** : Go crypto/hmac, Node crypto
- **Base de données** : MongoDB, PostgreSQL

### Fonctionnalités
- `POST /webhook/stripe` : réception d’un paiement.
- `POST /webhook/github` : réception d’un push.
- Enregistrement en base des événements reçus.
- Traitement des événements selon le type.

### Étapes de développement
1. **Créer une route webhook**
   - Ex : /webhook/service-name
2. **Valider la signature**
   - Vérifier que l’appel vient bien du bon service.
3. **Logger les données**
   - Sauvegarder event_type, payload, timestamp.
4. **Traiter l’événement**
   - Ex : paiement confirmé → changement d’état en DB.
5. **Répondre au service**
   - Retourner un 200 OK ou un 400 en cas d’échec.
6. **(Bonus) Retenter les événements échoués automatiquement.**

---

## Projet 10 : Architecture microservices

### Contexte
Ton application devient complexe. Tu veux la découper en microservices pour plus de maintenabilité, scalabilité et autonomie des composants. Chaque microservice a sa responsabilité (users, products, notifications, etc.).

### Objectifs
- Concevoir plusieurs services indépendants.
- Permettre leur communication (HTTP ou message broker).
- Gérer l’orchestration avec un API Gateway.
- Conteneuriser le tout avec Docker.

### Stack recommandée
- **Langages** : Go, Node.js
- **Communication** : REST, gRPC ou Message Broker (NATS, RabbitMQ)
- **Gateway** : API Gateway maison ou Kong / Traefik
- **Orchestration** : Docker Compose ou Kubernetes (si tu veux pousser)

### Exemple d’architecture
- **Service Auth** : inscription / connexion
- **Service Users** : gestion du profil
- **Service Products** : produits, stock
- **Service Notifications** : emails, SMS, webhooks
- **API Gateway** : route les requêtes vers les bons services

### Étapes de développement
1. **Définir les domaines fonctionnels**
   - Découper chaque service et son rôle.
2. **Créer les services indépendants**
   - Chacun avec son propre projet, base de données, port.
3. **Créer un moyen de communication**
   - Soit en REST entre services, soit avec un broker (ex : RabbitMQ).
4. **Déployer avec Docker**
   - Un docker-compose.yml pour tous les services.
5. **Ajouter un API Gateway**
   - Pour centraliser les requêtes entrantes.
6. **(Bonus) Ajouter une interface admin**
   - Pour tester tous les services depuis une UI unique.

---

Pour plus de détails, veuillez consulter chaque projet dans les répertoires correspondants.

---

## Badges
![Build Status](https://img.shields.io/github/workflow/status/Cheikh-Nakamoto/Backend_Go-Rust/CI)
![Coverage](https://img.shields.io/codecov/c/github/Cheikh-Nakamoto/Backend_Go-Rust)
![License](https://img.shields.io/github/license/Cheikh-Nakamoto/Backend_Go-Rust)

---

## Auteurs et Licence
Projet développé par Cheikh-Nakamoto. Sous licence MIT.
```
