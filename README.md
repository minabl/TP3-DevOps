# MERN APP

## Vue d'Ensemble
Ce projet est une application full-stack qui consiste en un client React et un serveur Node.js utilisant MongoDB comme base de données. Docker est utilisé pour la conteneurisation, et Docker Compose est utilisé pour orchestrer les services.


 ## Image
  
  ![image](https://github.com/user-attachments/assets/e52d6f57-dd91-43fe-b166-8bd66985594d)
  ![image](https://github.com/user-attachments/assets/6d45de35-cfb1-4d02-8cee-cd845b4d3190)


  ![image](https://github.com/user-attachments/assets/c8a98323-1315-45cf-a4c7-7178bf6990a2)

![image](https://github.com/user-attachments/assets/8dc42e23-057f-4fb6-8ff5-ad815bb95dbe)

## Explications des modifications :
1. Nettoyage après exécution :
 Le bloc post avec always est utilisé pour garantir que le nettoyage se fait à la fin du pipeline, qu'il réussisse ou échoue. La commande docker system prune -f supprime toutes les images Docker inutilisées et les conteneurs intermédiaires.

2. Exécution conditionnelle par dossier :
La directive when avec changeset permet de vérifier si des fichiers ont été modifiés dans les répertoires server ou client. Si un changement est détecté dans un des dossiers, les étapes correspondantes (build, scan, push) s'exécutent uniquement pour ce dossier. Par exemple, changeset "server/**" vérifie les modifications dans tout le dossier server.

## Technologies Utilisées
- **Frontend** : React
- **Backend** : Node.js, Express
- **Base de Données** : MongoDB
- **Conteneurisation** : Docker, Docker Compose

## Variables d'Environnement
Les variables d'environnement suivantes sont utilisées dans l'application :

- **REACT_APP_API_URL** : Cette variable contient l'URL de base pour le serveur API. Elle est utilisée dans le client React pour faire des requêtes au serveur.
- **MONGO_URI** : L'URI de connexion à MongoDB utilisée par le serveur pour se connecter à l'instance MongoDB.

## Configuration de Docker
Ce projet comprend des Dockerfiles pour le client et le serveur, qui facilitent la construction et l'exécution des services dans des conteneurs isolés. Les configurations incluent :

- **Client** : Un environnement Node.js pour construire l'application React. Les dépendances sont installées et l'application est construite pour une utilisation en production. Un serveur HTTP simple peut être utilisé pour servir l'application construite.
  
- **Serveur** : Un environnement Node.js qui installe les dépendances nécessaires et configure l'application pour écouter sur un port spécifique.

## Images Docker
Les images Docker créées pour ce projet sont les suivantes :

- **Image du Client** : `node:lts-alpine`
- **Image du Serveur** : `node:lts-alpine`
- **Image de la Base de Données** : `mongo:latest`

Ces images sont spécifiées dans les Dockerfiles respectifs et sont utilisées lors de la construction et du déploiement des services.

## Docker Compose
Docker Compose est utilisé pour gérer les différents services de l'application, y compris le client, le serveur et MongoDB. Les services sont interconnectés, ce qui permet une communication fluide entre le client et le serveur. Le fichier de configuration spécifie les images, les ports exposés, ainsi que les variables d'environnement nécessaires pour chaque service.

## Comment Exécuter le Projet
1. Assurez-vous d'avoir Docker et Docker Compose installés sur votre machine.
2. Clonez ce dépôt sur votre machine locale.
3. Accédez au répertoire du projet dans votre terminal.
4. Construisez et démarrez l'application en utilisant Docker Compose :

   ```bash
   docker-compose up --build
   ```

5. Accédez au client à [http://localhost:3000](http://localhost:3000).

