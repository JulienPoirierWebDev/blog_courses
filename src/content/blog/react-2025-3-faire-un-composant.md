---
author: Julien POIRIER
pubDatetime: 2025-02-06T07:00:00.00Z
modDatetime: 2025-02-06T07:00:00.00Z
title: Faire des composants réutilisables
slug: faire-des-composants-reutilisables
featured: true
draft: false
tags:
  - react
  - debutant
description: Nous verrons comment créer un composant, l'élement de base de toute application React !
---

# **Comprendre les Composants en React : La Brique de Base de vos Applications**

Après avoir installé et configuré ton projet React avec Vite, il est temps de plonger dans le cœur de React : **les composants**. Ce sont eux qui donnent vie à tes interfaces, les rendant dynamiques, modulaires et faciles à maintenir.

Dans cet article, nous allons :  
✅ **Définir ce qu’est un composant en React**  
✅ **Voir comment créer et organiser des composants**  
✅ **Apprendre à passer des données avec les props**  
✅ **Découvrir l’importance de la réutilisabilité des composants**

---

## **1. Qu’est-ce qu’un Composant en React ?**

Un **composant** en React est une **fonction JavaScript** (ou une classe) qui retourne du **JSX**, un mélange de HTML et JavaScript.

📌 **Pourquoi utiliser des composants ?**  
Les composants permettent de **diviser ton interface en petites parties réutilisables**. Chaque composant a un rôle précis, ce qui rend ton code plus **modulaire**, **lisible** et **facile à maintenir**.

### **Exemple simple de composant :**

```jsx
function Welcome() {
  return <h1>Bienvenue sur mon site !</h1>;
}
```

➡️ Ici, `Welcome` est un composant fonctionnel qui retourne un titre `<h1>`.

---

## **2. Types de Composants : Fonctionnels vs Classes**

### **📌 Composants Fonctionnels (les plus courants)**

Depuis l’introduction des **hooks** en React 16.8, les **composants fonctionnels** sont devenus la norme. Ils sont plus simples et plus lisibles.

```jsx
function Hello() {
  return <p>Bonjour, utilisateur !</p>;
}
```

### **📌 Composants Basés sur des Classes (ancien usage)**

Avant les hooks, on utilisait des **classes** pour gérer l’état et les cycles de vie des composants. Aujourd’hui, ils sont moins courants.

```jsx
import React, { Component } from "react";

class Hello extends Component {
  render() {
    return <p>Bonjour, utilisateur !</p>;
  }
}
```

---

## **3. Structurer et Organiser ses Composants**

### **Composant Principal (`App.jsx`)**

Dans React, le fichier `App.jsx` est souvent le **composant principal** qui regroupe tous les autres.

**Exemple :**

```jsx
function App() {
  return (
    <div>
      <Header />
      <MainContent />
      <Footer />
    </div>
  );
}
```

### **Créer des Composants Séparés**

Tu peux organiser tes composants dans des fichiers distincts pour garder ton projet propre.

**`src/components/Header.jsx`**

```jsx
function Header() {
  return <h1>Mon Super Site</h1>;
}

export default Header;
```

**`src/components/Footer.jsx`**

```jsx
function Footer() {
  return <footer>© 2024 Mon Super Site</footer>;
}

export default Footer;
```

Ensuite, importe-les dans `App.jsx` :

```jsx
import Header from "./components/Header";
import Footer from "./components/Footer";

function App() {
  return (
    <div>
      <Header />
      <p>Bienvenue dans le contenu principal de l'application.</p>
      <Footer />
    </div>
  );
}
```

---

## **4. Les Props : Passer des Données aux Composants**

Les **props** (propriétés) permettent de **transmettre des données d’un composant parent à un composant enfant**. Cela rend les composants **dynamiques et personnalisables**.

### **Exemple :**

Créons un composant `Greeting` qui affiche un message personnalisé.

```jsx
function Greeting({ name }) {
  return <p>Bonjour, {name} !</p>;
}
```

Dans `App.jsx`, on passe une prop `name` :

```jsx
function App() {
  return (
    <div>
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </div>
  );
}
```

➡️ **Résultat** :

- Bonjour, Alice !
- Bonjour, Bob !

📌 **Remarque** : Les props sont **immuables**. Si tu veux modifier des données dans un composant, tu dois utiliser le **state** (qu’on verra dans un prochain article).

---

## **5. La Réutilisabilité des Composants**

L’un des plus grands avantages de React est la possibilité de créer des composants **réutilisables**. Cela permet de réduire la duplication de code et d’assurer la cohérence de l’interface.

### **Exemple : Créer un bouton réutilisable**

```jsx
function Button({ text, onClick }) {
  return <button onClick={onClick}>{text}</button>;
}
```

Utilisation dans `App.jsx` :

```jsx
function App() {
  return (
    <div>
      <Button text="Clique ici" onClick={() => alert("Bouton cliqué !")} />
      <Button
        text="Supprimer"
        onClick={() => alert("Suppression effectuée !")}
      />
    </div>
  );
}
```

➡️ Le composant `<Button />` est utilisé avec des textes et des actions différentes, sans réécrire le code.

---

## **6. Bonnes Pratiques pour Créer des Composants**

- **1 Composant = 1 Fonction** : Garde chaque composant simple et focalisé sur une seule tâche.
- **Nom des Composants en Majuscule** : Par convention, les composants commencent par une majuscule (`Header`, `Footer`).
- **Utilise les Props pour Rendre tes Composants Dynamiques**.
- **Garde tes Fichiers Organisés** : Crée un dossier `components/` pour ranger tes composants.

---

## **7. Conclusion : Les Composants, Cœur de React**

🎯 **Les composants sont les briques de base de toute application React.**  
Ils permettent de :  
✅ **Diviser** l’interface en petites parties réutilisables.  
✅ **Passer des données dynamiquement** avec les props.  
✅ **Garder ton code lisible, modulaire et facile à maintenir**.

**🚀 Prochaine étape : Apprendre à gérer des données dynamiques avec le State en React.**  
👉 **Lis l’article suivant : "Le State en React : Gérer des Données Dynamiques avec useState".**
