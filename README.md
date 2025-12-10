# Rapport de TP : Conteneurisation d'une Application Smart Home

## 1. Introduction

Ce document présente le compte-rendu du travail pratique (TP) portant sur la conteneurisation d'une application "Smart Home". Cette application est constituée d'une architecture micro-services incluant un backend développé avec le framework Spring Boot et un frontend réalisé avec Angular. L'objectif principal de ce travail est de mettre en œuvre les concepts de conteneurisation à l'aide de Docker et d'orchestration de conteneurs via Docker Compose.

L'architecture technique du projet comprend :
*   **Backend** : Service RESTful Java Spring Boot.
*   **Frontend** : Interface utilisateur Angular.
*   **Base de Données** : Système de gestion de base de données relationnelle MySQL.
*   **Administration** : Interface PhpMyAdmin pour la gestion de la base de données.

## 2. Prérequis Techniques

La mise en œuvre de ce projet nécessite l'installation préalable des outils suivants sur l'environnement hôte :
*   **Docker** : Plateforme de conteneurisation.
*   **Docker Compose** : Outil de définition et d'exécution d'applications Docker multi-conteneurs.

## 3. Structure du Projet

L'arborescence du projet est organisée de la manière suivante :

*   `Smart_Home_back/` : Ce répertoire contient le code source de l'application backend ainsi que le fichier `Dockerfile` nécessaire à la création de son image.
*   `smartHome-front/` : Ce répertoire héberge le code source de l'application frontend et son fichier `Dockerfile` associé.
*   `docker-compose.yml` : Fichier de configuration YAML situé à la racine, définissant les services, les réseaux et les volumes pour l'orchestration de l'ensemble de l'application.

## 4. Procédure de Déploiement

### 4.1. Récupération des sources
Si les sources ne sont pas présentes localement, elles peuvent être obtenues via le gestionnaire de versions Git :
```bash
git clone https://github.com/lachgar/smarthouse.git
cd smarthouse
```

### 4.2. Construction des Images
La commande suivante permet de construire les images Docker pour les services définis dans le fichier de configuration :
```bash
docker-compose build
```

### 4.3. Démarrage des Services
Pour instancier et démarrer les conteneurs, exécutez la commande :
```bash
docker-compose up -d
```
L'option `-d` (detached) permet l'exécution des conteneurs en arrière-plan.

### 4.4. Vérification
L'état des conteneurs peut être vérifié à l'aide de :
```bash
docker-compose ps
```

## 5. Analyse de la Configuration de Conteneurisation

### 5.1. Backend (Spring Boot)
Le fichier `Smart_Home_back/Dockerfile` utilise une stratégie de construction multi-étapes (Multi-Stage Build) pour optimiser la taille de l'image finale :
*   **Étape de Compilation** : Utilisation d'une image Maven pour compiler le code source Java et produire l'arfact exécutable (JAR).
*   **Étape d'Exécution** : Utilisation d'une image JRE (Java Runtime Environment) légère basée sur Alpine Linux pour exécuter l'application.

### 5.2. Frontend (Angular)
Le fichier `smartHome-front/Dockerfile` adopte également une approche multi-étapes :
*   **Étape de Construction** : Utilisation d'une image Node.js pour installer les dépendances (npm install) et compiler l'application Angular (npm run build).
*   **Étape de Diffusion** : Utilisation d'un serveur web Nginx (image Alpine) pour héberger les fichiers statiques générés.

### 5.3. Orchestration (Docker Compose)
Le fichier `docker-compose.yml` orchestre quatre services interconnectés :
*   **mysql** : Service de base de données, exposé sur le port 3306.
*   **backend** : Service applicatif Spring Boot, exposé sur le port 8085. Il déclare une dépendance explicite (`depends_on`) envers le service `mysql`.
*   **frontend** : Service web Angular, exposé sur le port 80 (HTTP standard). Il dépend du service `backend` pour son fonctionnement.
*   **phpmyadmin** : Interface d'administration pour MySQL, accessible via le port 8081.

## 6. Accès à l'Application

Une fois l'infrastructure déployée, les différents composants sont accessibles via les adresses locales suivantes :

*   **Interface Frontend** : http://localhost:80
*   **API Backend** : http://localhost:8085
*   **Interface d'Administration (PhpMyAdmin)** : http://localhost:8081

## 7. Démonstration

Cette section est réservée à la présentation de captures d'écran validant le bon fonctionnement de l'application.

### 7.1. Interface Utilisateur
*(Insérer ici une capture d'écran montrant l'interface principale de l'application Angular chargée dans le navigateur)*

### 7.2. Base de Données
*(Insérer ici une capture d'écran de l'interface PhpMyAdmin confirmant la création et la structure de la base de données)*
