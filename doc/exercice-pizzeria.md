# Exercice : Simulation d'une pizzeria avec GitHub Actions

## 🔍 Contexte

Vous êtes le nouveau responsable de la pizzeria "Pizza Express" et vous avez décidé de moderniser votre établissement avec GitHub Actions. Votre objectif : éviter les catastrophes comme la fois où Luigi a livré une pizza végétarienne à un client qui avait commandé une "Carnivore Supreme" ! 🥩

Le workflow doit simuler tout le processus, de la prise de commande jusqu'à la gestion des réclamations (qu'on espère éviter grâce à notre système automatisé !).

## 🎯 Objectifs pédagogiques

- Comprendre l'utilisation des inputs dans workflow_dispatch
- Maîtriser la manipulation des données JSON avec fromJSON
- Utiliser les matrices pour paralléliser les tâches
- Comprendre le mécanisme de concurrence
- Gérer les permissions GitHub

## 📝 Étapes de réalisation

### 1️⃣ Configuration initiale

- Créer un fichier `.github/workflows/pizzeria.yml`
- Configurer le déclenchement manuel avec `workflow_dispatch`
- Définir deux inputs :
  - `nom_client` : pour le nom du client
  - `pizzas` : pour la liste des pizzas au format JSON (avec description détaillée du format attendu)

### 2️⃣ Réception de la commande

1. Créer un job `reception-commande`
2. Configurer un output pour transmettre la liste des pizzas
3. Créer des steps pour :
   - Afficher un message d'accueil avec le nom du client
   - Confirmer la réception de la commande

### 3️⃣ Préparation en cuisine

1. Créer un job `cuisine` qui dépend de `reception-commande`
2. Configurer une matrice avec :
   - `pizza` : utiliser fromJSON pour récupérer la liste des pizzas
   - `taille` : ["L", "XL"]
3. Ajouter une configuration include pour une pizza dessert gratuite
4. Créer un step pour simuler la préparation de chaque pizza

### 4️⃣ Livraison

1. Créer un job `livraison` qui dépend de `cuisine`
2. Configurer la concurrence :
   - Définir un groupe `zone-livraison`
   - Activer `cancel-in-progress`
3. Créer deux steps :
   - Préparation de la livraison
   - Simulation du temps de livraison (1 minute)

### 5️⃣ Gestion des réclamations

1. Créer un job `reclamation` qui dépend de `livraison`
2. Configurer la permission d'écriture pour les issues
3. Créer une issue GitHub pour la réclamation

## 🛠️ Conseils techniques

- Utilisez `runs-on: ubuntu-latest` pour tous les jobs
- Pour l'input JSON : `["pizza1","pizza2"]` (attention aux guillemets doubles)
- Pour la concurrence, utilisez `concurrency` au niveau du job
- Pour les permissions, utilisez `permissions` au niveau du job

## ⚠️ Points d'attention

- Le format JSON doit être strictement respecté
- Les needs doivent être correctement configurés
- La concurrence doit permettre l'annulation des livraisons
- Les permissions doivent être limitées au strict nécessaire

## ✅ Critères de réussite

- Le workflow s'exécute sans erreur
- Les pizzas sont correctement transmises entre les jobs
- La matrice génère le bon nombre de combinaisons
- La livraison peut être annulée si une nouvelle commande arrive
- Les issues sont créées avec les bonnes permissions

## 📊 Structure du workflow final

```yaml
jobs:
  reception-commande:
    # Configuration de la réception

  cuisine:
    needs: reception-commande
    # Configuration de la cuisine avec matrix

  livraison:
    needs: cuisine
    # Configuration de la livraison avec concurrency

  reclamation:
    needs: livraison
    # Configuration des réclamations avec permissions
```

## 💭 Indices supplémentaires

- Pensez à tester le workflow avec différentes combinaisons de pizzas
- La matrice permet de préparer plusieurs pizzas en parallèle
- La concurrence simule un livreur qui ne peut gérer qu'une livraison à la fois
- Les permissions sont nécessaires uniquement pour le job de réclamation
