# Exercice CI/CD avec GitHub Actions

## 🎯 Objectif

Créer un workflow qui :

- Lance des tests sur une application React.
- Déploie l'application dans un environnement de production ou de développement selon la branche où l'on se situe :
  - Si sur la branche `dev` ⇒ développement.
  - Si sur la branche `main` ⇒ production.

## 🛠️ Mise en place

Nous avons besoin d'une application React avec un script de tests.

### 📝 Étapes d'installation

1. Installer React dans notre répertoire courant :

   ```sh
   npm create vite@latest . -- --template react-ts
   ```

   Vous pouvez ignorer les fichiers déjà présents dans le dépôt lorsque l'installateur vous posera la question.

2. Installer les dépendances :

   ```sh
   npm install
   ```

3. Installer Vitest, React Testing Library et jsdom :

   ```sh
   npm install -D vitest @testing-library/react jsdom
   ```

4. Ajouter le script de test `test:ci` dans `package.json` :

   ```json
   "scripts": {
     "test:ci": "vitest run"
   }
   ```

5. Ajouter un fichier de configuration pour Vitest à la racine du projet : `vitest.config.ts`

   ```tsx
   /// <reference types="vitest" />
   import { defineConfig } from "vite";
   import react from "@vitejs/plugin-react";

   export default defineConfig({
     plugins: [react()],
     test: {
       environment: "jsdom",
       globals: true,
     },
   });
   ```

6. Ajouter un test en créant le fichier `./src/App.test.tsx` avec le contenu suivant :

   ```tsx
   import { expect, it } from "vitest";
   import { render, screen, fireEvent } from "@testing-library/react";
   import App from "./App";

   it("should display the Vite heading", () => {
     render(<App />);
     const heading = screen.getByRole("heading", { level: 1 });
     expect(heading.textContent).toMatch(/Vite/i);
   });

   it("should increment counter on button click", () => {
     render(<App />);
     const button = screen.getByRole("button", { name: /count/i });

     expect(button.textContent).toBe("count is 0");

     fireEvent.click(button);
     expect(button.textContent).toBe("count is 1");

     fireEvent.click(button);
     expect(button.textContent).toBe("count is 2");
   });
   ```

7. Lancer le script de test pour vérifier si tout fonctionne correctement :

   ```sh
   npm run test:ci
   ```

8. Vous pouvez également lancer le serveur de développement si vous le souhaitez :

   ```sh
   npm run dev
   ```

## ✨ Consignes

- Supprimer les workflows déjà présents.
- Faire un commit avec le message : "Suppression des anciens workflows et installation de Vite".
- Créer un nouveau workflow `ci-cd.yml` qui réagit aux `push`.
  - **Bonus optionnel** : ne se lance que sur les branches `main` et `dev`.
- Créer **deux jobs** :

  - **Premier job : Tests**

    - Il nécessite :
      - De récupérer le dépôt.
      - D'installer Node.js en utilisant une action officielle prévue à cet effet.
      - D'installer les dépendances avec `npm install`.
      - De lancer les tests avec `npm run test:ci`.

  - **Second job : `deploy`**

    - Si l'on est sur la branche `dev`, on affiche :

      ```sh
      🚀 Deploying to DEVELOPMENT environment
      ```

    - Si l'on est sur la branche `main`, on affiche :

      ```sh
      🚀 Deploying to PRODUCTION environment
      ```

## 🏁 Code final attendu

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
      - dev

jobs:
  build-and-test:
    name: Build & Test
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Node.js
        uses: actions/setup-node@v4

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm run test:ci

  deploy:
    name: Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to Development
        if: github.ref == 'refs/heads/dev'
        run: echo "🚀 Deploying to DEVELOPMENT environment"

      - name: Deploy to Production
        if: github.ref == 'refs/heads/main'
        run: echo "🚀 Deploying to PRODUCTION environment"
```
