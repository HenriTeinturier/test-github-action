A REVOIR

Exercice : Refactorisation d'un workflow GitHub Actions avec des workflows réutilisables

Objectif

Apprendre à refactoriser un workflow GitHub Actions en utilisant des workflows réutilisables afin de réduire le code dupliqué et faciliter la maintenance.

Workflow initial (base)

Voici un workflow initial simple qui réalise les étapes suivantes :

Installation des dépendances

Exécution des tests

Déploiement sur un serveur fictif (simulé par un simple echo)

```yaml
name: CI-CD Basic

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Deploy application
        run: echo "Deploying application to server..."
```

Consigne

Vous devez refactoriser le workflow ci-dessus pour extraire les étapes suivantes dans un workflow réutilisable :

Installation des dépendances

Exécution des tests

Ce workflow réutilisable pourra ainsi être appelé depuis d'autres workflows à l'avenir.

Votre solution finale devra inclure :

Un nouveau workflow réutilisable

Le workflow initial refactorisé utilisant ce nouveau workflow réutilisable avec la syntaxe appropriée (uses).

Solution attendue

Workflow réutilisable (.github/workflows/test-and-install.yml)

```yaml
name: Test and Install Dependencies

on:
  workflow_call:

jobs:
  test-and-install:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

Workflow initial refactorisé

```yaml
name: CI-CD Basic (refactorisé)

on:
  push:
    branches:
      - main

jobs:
  test:
    uses: ./.github/workflows/test-and-install.yml

  deploy:
    needs: test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Deploy application
        run: echo "Deploying application to server..."
```

Instructions supplémentaires

Vérifiez que votre workflow réutilisable fonctionne correctement en faisant une modification dans votre dépôt GitHub.

Assurez-vous que le job deploy dépend du succès du job test (du workflow réutilisable) avant de s'exécuter.

Bonne refactorisation !