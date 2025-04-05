# Backend_Go-Rust

Projet 1 : API CRUD complète pour une gestion de tâches

Contexte

Tu travailles pour une startup qui développe une application de gestion de tâches personnelles. Tu es chargé de créer une API qui permettra à l'application frontend d'ajouter, lister, modifier et supprimer des tâches.

Objectifs

Créer une API RESTful.

Implémenter les opérations CRUD.

Gérer les statuts des tâches (à faire, en cours, terminé).

Ajouter des timestamps (création et mise à jour).


Stack recommandée

Langage : Go (Gin) ou Node.js (Express)

Base de données : PostgreSQL ou MongoDB

Outils : Postman, Docker (optionnel)


Fonctionnalités

GET /tasks : récupérer toutes les tâches.

GET /tasks/:id : récupérer une tâche par ID.

POST /tasks : ajouter une nouvelle tâche.

PUT /tasks/:id : modifier une tâche.

DELETE /tasks/:id : supprimer une tâche.


Étapes de développement

1. Initialiser le projet

Création du dépôt, configuration de Gin/Express.



2. Modèle de données

Structure de la tâche : id, title, description, status, createdAt, updatedAt.



3. Connexion à la base de données

Création de la table/collection.



4. Création des routes

Implémentation de chaque endpoint.



5. Tests avec Postman

Tester tous les cas (valide, erreur, non trouvé).



6. (Bonus) Ajout de validation

Empêcher les titres vides, etc.





---

Projet 2 : Système d’authentification avec JWT et OAuth2

Contexte

Tu dois sécuriser une application permettant aux utilisateurs de créer des comptes, de se connecter et de gérer leur profil. En plus du système classique, tu veux offrir la possibilité de se connecter via Google (OAuth2).

Objectifs

Authentification avec email/mot de passe (via JWT).

Connexion via Google (OAuth2).

Sécurisation des routes privées.


Stack recommandée

Langage : Node.js (Express) ou Go (Gin)

Auth : JWT, OAuth2 (Google)

Base de données : PostgreSQL / MongoDB


Fonctionnalités

POST /register : inscription d’un utilisateur.

POST /login : connexion avec retour d’un token JWT.

GET /me : récupérer le profil de l’utilisateur connecté.

GET /auth/google : démarrer l’authentification Google.

GET /auth/google/callback : callback de Google.


Étapes de développement

1. Initialiser le projet

Créer un backend de base.



2. Création du modèle utilisateur

id, email, password, provider, createdAt.



3. Mise en place du registre/login

Hachage du mot de passe (bcrypt).

Génération de JWT.



4. Middleware d’authentification

Vérification du token dans les headers.



5. Connexion Google

Utiliser l’API Google (client ID / secret).

Gérer le callback et récupérer les infos utilisateur.



6. Tests des routes sécurisées

Protéger certaines routes.


Projet 3 : Application de chat en temps réel avec WebSocket

Contexte

Tu développes une messagerie pour une application communautaire. Les utilisateurs doivent pouvoir envoyer et recevoir des messages instantanément, sans avoir besoin de rafraîchir la page.

Objectifs

Mettre en place un serveur WebSocket.

Permettre à plusieurs utilisateurs de discuter en temps réel.

Stocker les messages dans une base de données.

Gérer les connexions et déconnexions.


Stack recommandée

Backend : Go (Gorilla WebSocket) ou Node.js (Socket.io)

Frontend : HTML/JS simple ou Angular/React

Base de données : MongoDB ou PostgreSQL


Fonctionnalités

Connexion WebSocket par utilisateur.

Envoi / réception de messages en temps réel.

Affichage des messages historiques à l'ouverture du chat.

Notification d’utilisateur en ligne / hors ligne (optionnel).


Étapes de développement

1. Initialiser le projet avec serveur WebSocket

Créer un endpoint ws://.../chat.



2. Gérer les connexions

Sauvegarder les connexions actives par utilisateur.



3. Gérer l’envoi de message

Quand un message est reçu, l’envoyer au destinataire connecté.

Sauvegarder le message en base (champ : sender, receiver, content, timestamp).



4. Récupérer l’historique

Quand un utilisateur se connecte, charger ses messages récents.



5. Déconnexion et gestion des erreurs

Fermer proprement les connexions et retirer de la liste active.





---

Projet 4 : Tableau de bord dynamique avec données en temps réel

Contexte

Tu es en charge d’un outil interne pour surveiller l’état des serveurs d’une entreprise. L’équipe technique a besoin d’un tableau de bord en temps réel affichant les stats de CPU, RAM, disques, etc.

Objectifs

Collecter des métriques système régulièrement.

Les envoyer en temps réel via WebSocket à un frontend.

Afficher ces données de manière visuelle.


Stack recommandée

Backend : Go ou Node.js + WebSocket

Monitoring : lib système (ex : os / sysinfo)

Frontend : React / Angular + Chart.js / D3.js


Fonctionnalités

Affichage en direct de la charge CPU, RAM, etc.

Graphiques mis à jour automatiquement.

Historique conservé temporairement (en mémoire ou base).

Détection de surcharge ou anomalies (optionnel).


Étapes de développement

1. Écriture du module de récupération des stats système

Exemple Go : github.com/shirou/gopsutil



2. Mise à jour régulière via goroutines ou cron interne

Envoi des données chaque 5 secondes par WebSocket.



3. Création d’un frontend dynamique

Connexion à la WebSocket.

Utilisation de graphiques pour les données.



4. Ajout d’alertes visuelles

Couleurs ou icônes en cas de dépassement de seuil.



5. (Bonus) Historique et export des données



Projet 5 : Service de gestion de fichiers (upload, compression, cloud, PDF)

Contexte

Tu développes un service qui permet aux utilisateurs d'uploader des fichiers (images, PDF, docs...), qui seront ensuite stockés dans un répertoire local ou dans le cloud. Les images doivent être compressées, et les documents PDF peuvent être générés à partir de données.

Objectifs

Uploader et stocker des fichiers.

Compresser les images automatiquement.

Générer des fichiers PDF à partir de données utilisateur.

Option : Intégrer le stockage cloud (S3 ou autre).


Stack recommandée

Backend : Go (Gin) ou Node.js (Express)

Libs :

Compression images : Go → github.com/nfnt/resize ou imaging, Node → sharp

PDF : Go → gofpdf, Node → pdfkit

Cloud : AWS SDK, Google Cloud Storage, etc.


Base de données : pour stocker les chemins des fichiers


Fonctionnalités

POST /upload : uploader un fichier.

Compression automatique des images.

Génération de PDF avec un POST /generate-pdf en fournissant des données JSON.

Téléchargement avec GET /files/:id.


Étapes de développement

1. Configuration du serveur pour gérer les uploads

Middleware multipart/form-data.



2. Stockage des fichiers

En local ou dans un bucket cloud.



3. Compression des images

Dès qu’un fichier image est uploadé.



4. Génération de PDF

À partir d’un contenu dynamique (profil utilisateur, facture...).



5. Téléchargement sécurisé

Vérification des droits d'accès avant l’envoi.





---

Projet 6 : Optimisation de la performance d’un serveur backend

Contexte

Ton serveur backend commence à avoir du trafic. Il est temps d’optimiser les performances pour tenir la charge : mise en cache, compression, équilibrage de charge, etc.

Objectifs

Ajouter un système de cache.

Activer la compression des réponses HTTP.

Simuler un environnement à forte charge.

(Bonus) Utiliser un reverse proxy comme Nginx.


Stack recommandée

Backend : Go ou Node.js

Cache : Redis ou en mémoire

Compression : gzip middleware

Test de charge : Apache Benchmark, K6, Locust


Fonctionnalités

Mise en cache des réponses d’API lentes.

Compression gzip automatique.

Mesure des temps de réponse avec et sans optimisations.


Étapes de développement

1. Mettre en place un cache simple (en mémoire ou Redis)

Par exemple, cache des résultats de /products.



2. Activer la compression gzip

Middleware dans Express ou Gin.



3. Test de charge avant optimisation

Pour obtenir un point de comparaison.



4. Implémenter les optimisations

Ajouter cache, compression, limiter les logs inutiles.



5. Test de charge après optimisation

Comparer les gains obtenus.



6. (Bonus) Ajouter Nginx en reverse proxy avec gzip, keep-alive, etc.



