Exercice : Création d'une action réutilisable complexe dans GitHub Actions

Objectif

Créer une action réutilisable GitHub Actions, plus complexe que les exercices précédents, permettant de gérer plusieurs étapes avancées dans un processus de CI/CD.

Scénario proposé

Vous devez créer une action réutilisable qui réalise les opérations suivantes sur un projet Node.js :

Vérifier la syntaxe du code avec ESLint

Vérifier le formatage du code avec Prettier

Lancer les tests unitaires

Générer un rapport de couverture de tests

Publier le rapport de couverture en tant qu'artifact sur GitHub Actions

Cette action doit être facilement utilisable par différents projets Node.js.

Consigne

Vous devrez :

Créer un nouveau dépôt GitHub pour l'action réutilisable.

Configurer correctement les fichiers action.yml, Dockerfile (si nécessaire), et scripts associés.

Rendre votre action accessible en utilisant un tag ou une branche spécifique.

Créer un workflow de démonstration dans un projet Node.js pour tester l'action.

Votre solution finale devra inclure :

Le fichier action.yml correctement configuré

Le workflow d'exemple utilisant votre nouvelle action

Solution attendue

Action réutilisable (action.yml)

```yaml
name: Node.js Advanced Checks

description: "Performs linting, formatting check, tests and coverage reports for Node.js projects"

inputs:
  node-version:
    description: "Node.js version"
    required: false
    default: "20.x"

runs:
  using: composite
  steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}

    - name: Install Dependencies
      run: npm install
      shell: bash

    - name: Run ESLint
      run: npm run lint
      shell: bash

    - name: Check Prettier Formatting
      run: npm run format:check
      shell: bash

    - name: Run Tests and Coverage
      run: npm run test:coverage
      shell: bash

    - name: Upload Coverage Report
      uses: actions/upload-artifact@v4
      with:
        name: coverage-report
        path: ./coverage
```

Workflow exemple utilisant l'action réutilisable

```yaml
name: CI-CD with Custom Action

on:
  push:
    branches:
      - main

jobs:
  quality-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Execute custom checks
        uses: username/nodejs-advanced-checks-action@v1
        with:
          node-version: '20.x'

  deploy:
    needs: quality-check
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Deploy application
        run: echo "Deploying application..."
```

Instructions supplémentaires

Assurez-vous que chaque commande (lint, format:check, test:coverage) est définie dans le package.json de votre projet Node.js.

Vérifiez la disponibilité et le bon fonctionnement des rapports d'artifacts sur GitHub après l'exécution du workflow.

Bon développement !

