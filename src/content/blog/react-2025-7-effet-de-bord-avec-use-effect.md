---
author: Julien POIRIER
pubDatetime: 2025-02-06T07:00:00.00Z
modDatetime: 2025-02-06T07:00:00.00Z
title: Ajouter un effet de bord avec useEffect
slug: ajouter-un-effet-de-bord-avec-use-effect
featured: true
draft: false
tags:
  - react
  - debutant
description: L'ajout d'effets de bord dans React permet d'interagir avec l'extérieur de la page (a utiliser avec précaution)
---

# **Comprendre `useEffect` en React : Gérer les Effets Secondaires et les Requêtes API**

Après avoir exploré la gestion du **state** et compris comment React met à jour l’interface via l’**arbre de composants**, il est temps d’aborder un concept fondamental : **les effets secondaires**.

En React, la fonction `useEffect` est l’outil principal pour gérer ces effets secondaires, comme :

- 📡 **Faire des requêtes API**
- ⏱️ **Démarrer des timers**
- 🎨 **Manipuler directement le DOM**
- 🧹 **Nettoyer des ressources quand un composant est supprimé**

Dans cet article, nous allons :  
✅ **Comprendre ce qu’est un effet secondaire en React**  
✅ **Découvrir comment utiliser `useEffect` pour gérer des actions après le rendu**  
✅ **Faire des requêtes API avec `useEffect`**  
✅ **Apprendre à nettoyer correctement les effets pour éviter les fuites de mémoire**

---

## **1. Qu’est-ce qu’un Effet Secondaire en React ?**

Un **effet secondaire** (ou **side effect**) correspond à **toute action qui interagit avec l’extérieur de ton composant** ou qui n’est pas purement liée au rendu de l’interface.

### **📌 Exemples d’effets secondaires :**

- Faire un appel réseau pour récupérer des données (ex : requête API).
- Manipuler le DOM directement (ajouter des classes, des animations).
- Démarrer ou arrêter des timers (`setTimeout`, `setInterval`).
- Écouter des événements globaux (ex : écoute de la taille de la fenêtre).

### **Pourquoi a-t-on besoin de `useEffect` ?**

React est conçu pour **gérer uniquement l’interface** de manière déclarative. Mais pour intégrer des actions extérieures (comme une requête API), il faut un moyen de dire à React **"Fais ça après le rendu"**. C’est là que `useEffect` entre en jeu.

---

## **2. La Syntaxe de `useEffect`**

`useEffect` est un **hook** qui permet de déclencher une fonction après que le composant a été monté (rendu) ou après que certaines valeurs ont changé.

### **📌 Syntaxe de base :**

```jsx
import React, { useEffect } from "react";

useEffect(() => {
  // Code de l’effet secondaire
});
```

Par défaut, le code dans `useEffect` s’exécute :

1. **Après le premier rendu** du composant.
2. **Après chaque mise à jour** du composant.

---

## **3. Exemple Simple : Mettre à Jour le Titre de la Page**

Commençons par un exemple simple où l’on change le titre de la page en fonction d’un compteur.

```jsx
import React, { useState, useEffect } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `Compteur : ${count}`;
  });

  return (
    <div>
      <h1>Compteur : {count}</h1>
      <button onClick={() => setCount(count + 1)}>Incrémenter</button>
    </div>
  );
}

export default Counter;
```

### **🔍 Ce qui se passe :**

1. Lors du **premier rendu**, `useEffect` change le titre de la page en "Compteur : 0".
2. Chaque fois que tu cliques sur le bouton, `count` augmente et **le composant est réexécuté**.
3. Après chaque mise à jour, **`useEffect` est déclenché à nouveau** pour mettre à jour le titre de la page.

---

## **4. Limiter les Exécutions avec le Tableau de Dépendances**

Dans l’exemple précédent, `useEffect` s’exécute **à chaque rendu**, même si le changement ne concerne pas le titre de la page. Ce n’est pas toujours nécessaire !

### **📌 Comment limiter les exécutions ?**

On peut ajouter un **tableau de dépendances** pour dire à React :

- **Quand** exécuter l’effet.
- **Quels changements** doivent déclencher l’effet.

### **Exemple avec dépendances :**

```jsx
useEffect(() => {
  document.title = `Compteur : ${count}`;
}, [count]);
```

### **🔍 Explication :**

- Le tableau `[count]` indique que l’effet doit s’exécuter **uniquement quand `count` change**.
- Si d’autres états changent dans le composant, l’effet **ne sera pas déclenché**.

### **Exécuter l’effet une seule fois (au montage) :**

Pour exécuter un effet **une seule fois au montage** du composant (comme `componentDidMount` dans les classes), on passe un **tableau vide** :

```jsx
useEffect(() => {
  console.log("Le composant est monté !");
}, []);
```

---

## **5. Faire des Requêtes API avec `useEffect`**

L’un des usages les plus courants de `useEffect` est de **faire des appels réseau** pour récupérer des données.

### **📌 Exemple : Récupérer des utilisateurs depuis une API**

Nous allons utiliser l’API gratuite [JSONPlaceholder](https://jsonplaceholder.typicode.com/) pour récupérer une liste d’utilisateurs.

```jsx
import React, { useState, useEffect } from "react";

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then(response => response.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      })
      .catch(error => console.error("Erreur :", error));
  }, []);

  if (loading) {
    return <p>Chargement en cours...</p>;
  }

  return (
    <div>
      <h2>Liste des utilisateurs</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            {user.name} ({user.email})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

### **🔍 Explication :**

1. Lors du premier rendu, `useEffect` exécute la requête `fetch`.
2. Quand les données sont récupérées, `setUsers(data)` met à jour le state et déclenche un nouveau rendu pour afficher la liste des utilisateurs.
3. Le tableau de dépendances `[]` garantit que la requête est **exécutée une seule fois**.

---

## **6. Nettoyer les Effets : Éviter les Fuites de Mémoire**

Parfois, un effet secondaire **doit être nettoyé** pour éviter des comportements indésirables. Par exemple :

- Arrêter un **timer**.
- Supprimer un **écouteur d’événement**.
- Annuler une **requête en cours** si le composant est démonté.

### **📌 Exemple : Utiliser un Timer et le Nettoyer**

```jsx
import React, { useState, useEffect } from "react";

function Timer() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prevSeconds => prevSeconds + 1);
    }, 1000);

    // Nettoyage de l'effet (clearInterval)
    return () => clearInterval(interval);
  }, []);

  return <h1>Temps écoulé : {seconds} secondes</h1>;
}

export default Timer;
```

### **🔍 Explication :**

1. `setInterval` démarre un timer qui incrémente `seconds` toutes les secondes.
2. Lorsque le composant est supprimé (par exemple, en naviguant vers une autre page), le `return () => clearInterval(interval)` **arrête le timer** pour éviter des fuites de mémoire.

---

## **7. Le Mode Strict de React et `useEffect` : Pourquoi Mon Effet s’Exécute Deux Fois ?**

Si tu développes en React avec la configuration par défaut (comme avec **Create React App** ou **Vite**), tu as peut-être remarqué que ton effet `useEffect` s’exécute **deux fois** lors du premier rendu. Ce comportement peut sembler déroutant au début, mais il est en fait lié au **Mode Strict de React**.

### **📌 Qu’est-ce que le Mode Strict (`StrictMode`) en React ?**

Le **Mode Strict** de React est un outil de **développement** (et uniquement de développement) qui aide à :

- Détecter des **problèmes potentiels** dans ton application.
- Mettre en avant des **pratiques recommandées**.
- **Simuler des comportements** qui pourraient apparaître dans des situations complexes (comme le rendu concurrentiel).

Il s’active automatiquement dans les projets créés avec Create React App ou Vite et **n’affecte pas l’application en production**.

### **Exemple : Comment le Mode Strict est activé ?**

Dans le fichier `main.jsx` de ton projet React, tu verras probablement ceci :

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

### **Que fait le Mode Strict ?**

1. Il **détecte des problèmes** comme des effets secondaires inattendus ou des mutations d’état incorrectes.
2. Il **force la réexécution** de certaines fonctions pour t’aider à repérer des comportements non sécurisés.

---

## **8. Pourquoi `useEffect` S’Exécute Deux Fois en Mode Strict ?**

En Mode Strict, React **monte et démonte** immédiatement les composants **une première fois**, puis les **remonte** pour détecter des effets secondaires indésirables.

### **📌 Exemple :**

```jsx
import React, { useEffect } from "react";

function DemoEffect() {
  useEffect(() => {
    console.log("useEffect exécuté");
  }, []);

  return <h1>Regarde la console !</h1>;
}

export default DemoEffect;
```

### **Résultat en Mode Strict :**

```
useEffect exécuté
useEffect exécuté
```

---

### **🔍 Pourquoi React fait ça ?**

1. **Détection des effets non nettoyés** : Si un effet secondaire n’est pas correctement nettoyé (par exemple, un timer ou un écouteur d’événements), ce comportement aide à identifier des **fuites de mémoire** potentielles.
2. **Simulation des comportements concurrents** : Avec l’introduction de fonctionnalités comme **React Concurrent Mode**, certains composants peuvent être montés/démontés rapidement. Le Mode Strict permet de simuler ce genre de situations.

---

## **9. Comment Corriger ou Gérer ce Comportement ?**

### **1. Nettoyer correctement les effets**

Quand tu utilises des effets qui créent des ressources (comme des timers, des abonnements à des événements, etc.), il est essentiel de **nettoyer** ces effets avec une fonction de retour.

**Exemple avec un timer :**

```jsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Timer en cours...");
  }, 1000);

  // Nettoyage pour éviter les fuites de mémoire
  return () => {
    clearInterval(timer);
    console.log("Timer nettoyé");
  };
}, []);
```

---

### **2. Désactiver le Mode Strict (déconseillé)**

Si le double rendu te dérange pendant le développement, tu peux retirer `<React.StrictMode>` dans `main.jsx`. **Cependant, ce n’est pas recommandé**, car tu perds les avantages des vérifications automatiques de React.

**Avant (Mode Strict activé) :**

```jsx
<React.StrictMode>
  <App />
</React.StrictMode>
```

**Après (Mode Strict désactivé) :**

```jsx
<App />
```

---

### **3. Ignorer en Production**

Pas d’inquiétude ! Le **Mode Strict n’affecte pas le comportement en production**.  
En production, `useEffect` ne s’exécutera qu’une seule fois comme attendu.

---

## **10. En Résumé : Le Mode Strict et `useEffect`**

🎯 **Le Mode Strict de React est un outil précieux pour identifier des problèmes potentiels dans ton code.**  
Quand il est activé :  
✅ `useEffect` s’exécute **deux fois lors du montage** pour détecter des effets non nettoyés.  
✅ Ce comportement **n’apparaît pas en production**.  
✅ Le Mode Strict t’aide à anticiper des situations complexes (comme le rendu concurrentiel).

**Nettoie toujours correctement tes effets** pour éviter des fuites de mémoire et assurer la stabilité de ton application.

---

## **11. Bonnes Pratiques avec `useEffect`**

### **✅ Utilise le tableau de dépendances pour contrôler quand l’effet s’exécute**

- `[]` : L’effet s’exécute **une seule fois** au montage.
- `[stateVar]` : L’effet s’exécute **quand `stateVar` change**.

### **✅ Nettoie toujours les effets qui créent des ressources externes**

- Pour les **timers**, les **écouteurs d’événements**, ou les **requêtes API**, utilise un retour de fonction dans `useEffect` pour faire le ménage.

### **✅ Évite les effets inutiles**

Ne mets pas dans le tableau de dépendances des valeurs qui **ne changent pas** ou qui n’ont **pas d’impact sur l’effet**.

---

## **12. Conclusion : Pourquoi `useEffect` est Essentiel en React**

🎯 **`useEffect` est l’outil indispensable pour gérer des effets secondaires dans une application React.**  
Avec lui, tu peux :  
✅ Faire des **requêtes API** et récupérer des données externes.  
✅ Gérer des **timers**, des **écouteurs d’événements**, et des **manipulations du DOM**.  
✅ Nettoyer correctement les ressources pour éviter des **fuites de mémoire**.

---

**🚀 Prochaine étape : Gérer des états plus complexes avec `useReducer` et comprendre quand l’utiliser à la place de `useState`.**  
👉 **Lis l’article suivant : "useState vs useReducer : Quelle différence et quand les utiliser en React ?"**
