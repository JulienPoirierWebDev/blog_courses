---
author: Julien POIRIER
pubDatetime: 2025-02-06T07:00:00.00Z
modDatetime: 2025-02-06T07:00:00.00Z
title: Utiliser React avec Vite
slug: utiliser-react-avec-vite
featured: true
draft: false
tags:
  - react
  - debutant
  - vite
description: Nous verrons comment rapidement mettre une place un projet React avec le tooling de Vite
---

# **Installation et Configuration d’un Projet React avec Vite**

Dans l’article précédent, nous avons découvert pourquoi React est si populaire et ses concepts fondamentaux. Maintenant, passons à la pratique !

Dans ce guide, nous allons voir comment **installer et configurer un projet React moderne** en utilisant **Vite**, une alternative ultra-rapide à Create React App (CRA).

📌 **Pourquoi Vite ?**

- ⚡ **Rapidité** : Vite utilise ES Modules et un serveur de développement performant.
- 🚀 **Léger et efficace** : Moins d’overhead que CRA, un build plus rapide.
- 🛠️ **Prêt pour TypeScript et d’autres outils modernes**.

---

## **1. Prérequis : Installer Node.js**

Avant d’installer React, assure-toi d’avoir **Node.js** installé sur ton ordinateur.

🔹 **Vérifier si Node.js est installé**  
Ouvre un terminal et tape :

```sh
node -v
```

Si une version est affichée (`v18.x.x` ou plus récent), tu es prêt ! Sinon, télécharge et installe [Node.js](https://nodejs.org).

---

## **2. Créer un projet React avec Vite**

Dans ton terminal, exécute la commande suivante :

```sh
npm create vite@latest mon-projet-react --template react
```

📌 **Explication** :

- `create vite@latest` → Crée un projet avec la dernière version de Vite.
- `mon-projet-react` → Nom du dossier où sera créé le projet.
- `--template react` → On spécifie que l’on veut un projet React.

---

## **3. Installer les dépendances**

Une fois le projet créé, entre dans le dossier et installe les modules nécessaires :

```sh
cd mon-projet-react
npm install
```

Cette commande va récupérer toutes les dépendances définies dans `package.json`.

---

## **4. Lancer le serveur de développement**

Tout est prêt ! Pour voir ton projet en action, lance la commande :

```sh
npm run dev
```

📌 **Vite démarre un serveur ultra-rapide** et affiche une URL du type :

```
Local: http://localhost:5173/
```

Ouvre cette adresse dans ton navigateur. 🎉 Tu devrais voir la page d’accueil React par défaut !

---

## **5. Structure d’un Projet React avec Vite**

Voyons rapidement les fichiers générés par Vite :

```
mon-projet-react/
│── node_modules/         # Dépendances installées
│── public/               # Fichiers statiques (images, favicons…)
│── src/                  # Code source
│   ├── App.jsx           # Composant principal
│   ├── main.jsx          # Point d'entrée
│   ├── index.css         # Styles globaux
│── .gitignore            # Fichiers à ignorer par Git
│── package.json          # Dépendances et scripts
│── vite.config.js        # Configuration de Vite
```

🔹 **`src/App.jsx`** → Contient le composant principal de l’application.  
🔹 **`src/main.jsx`** → Monte le composant `<App />` dans le DOM.

---

## **6. Modifier le premier composant React**

Ouvre `src/App.jsx` et modifie le contenu comme ceci :

```jsx
function App() {
  return (
    <div>
      <h1>Bienvenue dans mon premier projet React 🚀</h1>
      <p>Ceci est une application créée avec Vite.</p>
    </div>
  );
}

export default App;
```

Dès que tu enregistres, ton navigateur **se met à jour instantanément** grâce au **Hot Module Replacement (HMR)**. Plus besoin de recharger la page manuellement !

---

## **7. Ajouter du CSS**

Ajoutons quelques styles à `src/index.css` :

```css
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  color: #333;
  text-align: center;
  margin: 0;
  padding: 20px;
}
```

**Résultat** : L’application est maintenant stylée et prête pour du développement.

---

## **8. Conclusion : Pourquoi choisir Vite pour React ?**

✅ **Démarrage ultra-rapide**  
✅ **Système de hot reload instantané**  
✅ **Optimisé pour la production**  
✅ **Prise en charge native de TypeScript, Sass, et d’autres outils modernes**
