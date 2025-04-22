# Exercice : La Cuisine des Workflows

## Contexte
Vous êtes responsable d'un restaurant virtuel qui prépare différents types de plats. Chaque plat nécessite des étapes de préparation spécifiques et des ingrédients différents.

## Objectifs
- Comprendre la structure des workflows GitHub Actions
- Apprendre à gérer des étapes répétitives
- Découvrir comment utiliser les matrices pour optimiser les workflows
- Voir la différence entre une approche répétitive et une approche optimisée

## Structure du projet
```
cuisine/
├── plats/
│   ├── ratatouille.txt
│   ├── pizza.txt
│   └── sushi.txt
└── .github/workflows/
```

## Tâches à réaliser

### 1. Workflow "Préparation de la Ratatouille"
- Créer un workflow qui se déclenche sur les push vers main
- Le workflow doit vérifier si le changement concerne la ratatouille (`plats/ratatouille.txt`)
- Afficher les étapes de préparation avec les ingrédients

#### Vérification
1. Se placer sur la branche main
2. Créer un fichier `plats/ratatouille.txt`
3. Commiter et pousser les changements
4. Vérifier dans l'onglet "Actions" que :
   - Le workflow s'est déclenché
   - Les ingrédients sont affichés
   - Les étapes de préparation sont affichées

### 2. Workflow "Préparation de la Pizza"
- Créer un workflow qui se déclenche sur les push vers main
- Le workflow doit vérifier si le changement concerne la pizza (`plats/pizza.txt`)
- Afficher les étapes de préparation avec les ingrédients

#### Vérification
1. Se placer sur la branche main
2. Créer un fichier `plats/pizza.txt`
3. Commiter et pousser les changements
4. Vérifier dans l'onglet "Actions" que :
   - Le workflow s'est déclenché
   - Les ingrédients sont affichés
   - Les étapes de préparation sont affichées

### 3. Workflow "Préparation des Sushis"
- Créer un workflow qui se déclenche sur les push vers main
- Le workflow doit vérifier si le changement concerne les sushis (`plats/sushi.txt`)
- Afficher les étapes de préparation avec les ingrédients

#### Vérification
1. Se placer sur la branche main
2. Créer un fichier `plats/sushi.txt`
3. Commiter et pousser les changements
4. Vérifier dans l'onglet "Actions" que :
   - Le workflow s'est déclenché
   - Les ingrédients sont affichés
   - Les étapes de préparation sont affichées

## Aide pour les workflows

### Workflow Ratatouille
```yaml
name: Préparation de la Ratatouille

on:
  push:
    branches: [ main ]
    paths:
      - 'plats/ratatouille.txt'

jobs:
  preparation:
    runs-on: ubuntu-latest
    steps:
      - name: Préparation de la Ratatouille
        run: |
          echo "=== Préparation de la Ratatouille ==="
          echo "Ingrédients :"
          echo "- Aubergines"
          echo "- Courgettes"
          echo "- Poivrons"
          echo "- Tomates"
          echo "- Oignons"
          echo "- Ail"
          echo "- Herbes de Provence"
          echo "Étapes :"
          echo "1. Couper les légumes en rondelles"
          echo "2. Disposer en couches dans un plat"
          echo "3. Assaisonner avec herbes et huile d'olive"
          echo "4. Cuire au four 45 minutes"
          echo "=== Bon appétit ! ==="
```

### Workflow Pizza
```yaml
name: Préparation de la Pizza

on:
  push:
    branches: [ main ]
    paths:
      - 'plats/pizza.txt'

jobs:
  preparation:
    runs-on: ubuntu-latest
    steps:
      - name: Préparation de la Pizza
        run: |
          echo "=== Préparation de la Pizza ==="
          echo "Ingrédients :"
          echo "- Pâte à pizza"
          echo "- Sauce tomate"
          echo "- Mozzarella"
          echo "- Jambon"
          echo "- Champignons"
          echo "- Olives"
          echo "- Origan"
          echo "Étapes :"
          echo "1. Étaler la pâte"
          echo "2. Étaler la sauce tomate"
          echo "3. Ajouter les garnitures"
          echo "4. Cuire au four 15 minutes"
          echo "=== Bon appétit ! ==="
```

### Workflow Sushi
```yaml
name: Préparation des Sushis

on:
  push:
    branches: [ main ]
    paths:
      - 'plats/sushi.txt'

jobs:
  preparation:
    runs-on: ubuntu-latest
    steps:
      - name: Préparation des Sushis
        run: |
          echo "=== Préparation des Sushis ==="
          echo "Ingrédients :"
          echo "- Riz à sushi"
          echo "- Vinaigre de riz"
          echo "- Saumon"
          echo "- Avocat"
          echo "- Concombre"
          echo "- Nori"
          echo "- Wasabi"
          echo "Étapes :"
          echo "1. Préparer le riz vinaigré"
          echo "2. Couper les ingrédients en lamelles"
          echo "3. Rouler les sushis"
          echo "4. Couper en morceaux"
          echo "=== Bon appétit ! ==="
```
