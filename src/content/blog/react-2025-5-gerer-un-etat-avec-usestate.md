---
author: Julien POIRIER
pubDatetime: 2025-02-06T07:00:00.00Z
modDatetime: 2025-02-06T07:00:00.00Z
title: Gérer l'état d'un composant avec useState
slug: gerer-un-etat-dans-un-composant-avec-usestate
featured: true
draft: false
tags:
  - react
  - debutant
description: Nous verrons comment gérer un état avec useState, pour stocker et modifier des données dans un composant ou un arrbre de composants.
---

# **Le State en React : Gérer des Données Dynamiques avec `useState`**

Après avoir compris comment créer et organiser des composants en React, il est temps d’aborder une notion essentielle : **le state**. C’est grâce à lui que ton application peut devenir **interactive et réactive**.

Dans cet article, nous allons découvrir :  
✅ **Ce qu’est le state en React**  
✅ **Comment utiliser le hook `useState`**  
✅ **Des exemples concrets pour gérer des interactions**  
✅ **Les bonnes pratiques pour manipuler le state efficacement**

---

## **1. Qu’est-ce que le State en React ?**

Le **state** est un **objet** qui permet de stocker des informations **propres à un composant** et qui peuvent **changer au cours du temps**. Lorsqu’une valeur dans le state change, **React met automatiquement à jour l’interface** pour refléter cette modification.

📌 **Différence entre props et state :**

- Les **props** sont **passées** aux composants depuis un parent et **ne peuvent pas être modifiées** par le composant qui les reçoit.
- Le **state** est **interne** au composant et peut être **modifié** pour refléter des changements dans l’interface.

---

## **2. Introduction à `useState` : Le Hook pour Gérer le State**

En React, on utilise le hook **`useState`** pour gérer le state dans les **composants fonctionnels**.

### **Syntaxe de base :**

```jsx
import React, { useState } from 'react';

function MonComposant() {
  const [valeur, setValeur] = useState(valeurInitiale);

  return (
    // Ton JSX ici
  );
}
```

📌 **Explication :**

- `valeur` → La **valeur actuelle** du state.
- `setValeur` → La **fonction** qui permet de modifier cette valeur.
- `useState(valeurInitiale)` → Initialise la valeur du state.

---

## **3. Exemple Simple : Un Compteur**

Commençons par un exemple classique : un **compteur** qui s’incrémente à chaque clic.

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // Initialisation à 0

  return (
    <div>
      <h1>Compteur : {count}</h1>
      <button onClick={() => setCount(count + 1)}>Incrémenter</button>
    </div>
  );
}

export default Counter;
```

### **Ce qui se passe ici :**

1. Le compteur est initialisé à **0** grâce à `useState(0)`.
2. À chaque clic sur le bouton, la fonction `setCount(count + 1)` est appelée, ce qui **met à jour le state**.
3. React **re-render** le composant pour afficher la nouvelle valeur.

---

## **4. Gérer des Champs de Saisie avec `useState`**

Voyons un autre exemple : **un champ de texte** où l’utilisateur peut saisir son nom.

```jsx
import React, { useState } from "react";

function NameForm() {
  const [name, setName] = useState(""); // Le champ est vide au départ

  return (
    <div>
      <h2>Entrez votre nom :</h2>
      <input type="text" value={name} onChange={e => setName(e.target.value)} />
      <p>Bonjour, {name} !</p>
    </div>
  );
}

export default NameForm;
```

### **Explication :**

1. Le state `name` est initialisé avec une **chaîne vide**.
2. L’attribut `value={name}` fait du champ un **champ contrôlé** : sa valeur dépend du state.
3. À chaque frappe, `onChange` déclenche `setName(e.target.value)` pour **mettre à jour le state**.
4. Le texte “Bonjour, {name} !” est automatiquement mis à jour.

---

## **5. Mettre à Jour un State Complexe (Objet ou Tableau)**

Le state ne se limite pas aux nombres ou aux chaînes. Tu peux aussi gérer des **objets** ou des **tableaux**.

### **Exemple : Ajouter des éléments dans une liste**

Créons une liste de tâches où l’on peut **ajouter dynamiquement** des éléments.

```jsx
import React, { useState } from "react";

function TodoList() {
  const [tasks, setTasks] = useState([]); // Liste vide au départ
  const [newTask, setNewTask] = useState(""); // Champ de saisie

  const addTask = () => {
    if (newTask.trim() !== "") {
      setTasks([...tasks, newTask]); // Ajoute la nouvelle tâche
      setNewTask(""); // Réinitialise le champ
    }
  };

  return (
    <div>
      <h2>Liste de Tâches</h2>
      <input
        type="text"
        value={newTask}
        onChange={e => setNewTask(e.target.value)}
      />
      <button onClick={addTask}>Ajouter</button>

      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li> // Affiche chaque tâche
        ))}
      </ul>
    </div>
  );
}

export default TodoList;
```

### **Points clés :**

- `setTasks([...tasks, newTask])` → On **copie** l’ancien tableau avec l’opérateur spread `...` et on ajoute la nouvelle tâche à la fin.
- Chaque mise à jour du state **rafraîchit automatiquement** la liste affichée.

---

## **6. Bonnes Pratiques pour Gérer le State**

### **✅ Ne jamais modifier le state directement**

Toujours utiliser la fonction de mise à jour (`setValeur`).  
❌ Mauvais :

```jsx
count = count + 1; // Cela ne mettra pas à jour l’interface !
```

✅ Correct :

```jsx
setCount(count + 1);
```

---

### **✅ Utiliser la version fonctionnelle pour les mises à jour dépendantes**

Lorsque la mise à jour dépend de la valeur précédente, il est préférable d’utiliser la version fonctionnelle de `setState`.

Exemple avec un compteur :

```jsx
setCount(prevCount => prevCount + 1);
```

Cela évite des erreurs lorsque plusieurs mises à jour sont enchaînées rapidement.

---

### **✅ Gérer les objets et tableaux avec précaution**

Lorsque tu modifies des objets ou des tableaux dans le state, assure-toi de **ne pas muter l’état directement**. Utilise des copies pour conserver l’immuabilité.

Exemple pour un objet :

```jsx
setUser(prevUser => ({ ...prevUser, age: prevUser.age + 1 }));
```

---

# **Le State en React : Pourquoi `useState` Déclenche le Rendu du Composant**

Dans l’article précédent, nous avons appris à utiliser `useState` pour gérer des données dynamiques en React. Mais **comment React sait-il qu’il doit mettre à jour l’interface quand une valeur change ?** 🤔

La réponse réside dans **la façon dont React gère le "rendu" des composants**. `useState` ne se contente pas de stocker une valeur ; il **informe React qu’une donnée a changé** et qu’il doit **réexécuter le composant** pour refléter cette modification.

Dans cet article, nous allons explorer :  
✅ **Comment React détecte les changements avec `useState`**  
✅ **Ce qu’est un "rendu" (render) en React**  
✅ **Pourquoi React réexécute un composant même si la valeur ne change pas**  
✅ **Des exemples pour comprendre quand et comment le rendu se déclenche**

---

## **1. Qu’est-ce qu’un Rendu en React ?**

En React, un **rendu** (ou **render**) correspond à **l’exécution d’un composant** pour générer l’interface utilisateur.

### **📌 Exemple simple :**

```jsx
function App() {
  console.log("Le composant est rendu !");
  return <h1>Bienvenue !</h1>;
}
```

Chaque fois que ce composant est exécuté, le message **"Le composant est rendu !"** apparaît dans la console. Cela signifie que React **exécute à nouveau** la fonction `App` pour générer l’élément `<h1>`.

---

## **2. `useState` : Le Déclencheur de Rendu**

Quand tu utilises `useState`, tu crées un **état local** qui, lorsqu'il est modifié via la fonction de mise à jour (`setState`), **déclenche automatiquement un nouveau rendu** du composant.

### **📌 Exemple avec `useState` :**

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  console.log("Le composant Counter est rendu !");

  return (
    <div>
      <h1>Compteur : {count}</h1>
      <button onClick={() => setCount(count + 1)}>Incrémenter</button>
    </div>
  );
}

export default Counter;
```

### **Ce qui se passe ici :**

1. **Initialisation** : Lors du premier chargement, `useState(0)` initialise `count` à **0** et React **rend** le composant une première fois.
2. **Mise à jour** : Quand tu cliques sur le bouton, `setCount(count + 1)` modifie la valeur de `count`.
3. **Déclenchement du rendu** : Dès que `setCount` est appelé, **React réexécute le composant entier** pour refléter le changement.

---

## **3. Pourquoi React Réexécute-t-il le Composant Entier ?**

En React, **le composant est une fonction pure**. Cela signifie que chaque fois qu’un changement de state est détecté, React **réexécute toute la fonction** pour s'assurer que l'interface est à jour.

### **📌 Pourquoi ne pas mettre à jour uniquement l'élément concerné ?**

- React utilise le **Virtual DOM** pour comparer l’interface générée avant et après le changement.
- Même si **tout le composant est réexécuté**, **seuls les éléments modifiés sont mis à jour dans le DOM réel**.

### **Exemple :**

Dans l’exemple du compteur, **même si le composant entier est réexécuté**, React sait que seul le texte du `<h1>` a changé et **optimise** la mise à jour.

---

## **4. Et si la Valeur du State ne Change Pas ?**

Un point important à comprendre : **même si tu appelles `setState` avec la même valeur, React réexécute le composant**.

### **Exemple :**

```jsx
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  console.log("Le composant Counter est rendu !");

  return (
    <div>
      <h1>Compteur : {count}</h1>
      <button onClick={() => setCount(count)}>
        Rafraîchir sans changement
      </button>
    </div>
  );
}
```

### **Ce qui se passe :**

1. Tu cliques sur le bouton, mais la valeur de `count` reste la même (`0`).
2. Pourtant, **React réexécute quand même le composant**.
3. Toutefois, **grâce au Virtual DOM**, React détecte que **rien n’a changé dans l’interface** et **ne met pas à jour le DOM réel**.

### **Pourquoi ce comportement ?**

- **Simplicité** : React adopte une approche simple : **à chaque appel de `setState`, le composant est réexécuté**, sans vérifier la valeur en amont.
- **Optimisation déléguée** : C’est le **Virtual DOM** qui s’occupe de vérifier s’il faut réellement mettre à jour l’interface.

---

## **5. Optimiser les Rendus Inutiles**

Même si React est optimisé pour gérer ces rendus, il existe des moyens d’éviter des exécutions inutiles dans des cas plus complexes.

### **1. Comparer les valeurs avant d’appeler `setState` :**

Avant de mettre à jour l’état, tu peux vérifier si la valeur a vraiment changé.

```jsx
const increment = () => {
  if (count !== count + 1) {
    setCount(count + 1);
  }
};
```

### **2. Utiliser `React.memo` pour éviter les rerenders inutiles :**

`React.memo` est un **outil d’optimisation** qui empêche un composant de se réexécuter si ses **props** n’ont pas changé.

```jsx
const Greeting = React.memo(function Greeting({ name }) {
  console.log("Le composant Greeting est rendu !");
  return <h2>Bonjour, {name} !</h2>;
});
```

---

## **6. En Résumé : Pourquoi `useState` Déclenche un Rendu ?**

🎯 **`useState` est le mécanisme qui signale à React qu’une donnée a changé**. Lorsqu’un changement est détecté :  
✅ **Le composant est réexécuté dans son intégralité**.  
✅ **React utilise le Virtual DOM pour comparer les versions** avant et après le changement.  
✅ **Seuls les éléments réellement modifiés sont mis à jour dans le DOM réel**.

Même si la valeur du state ne change pas, l’appel à `setState` déclenche un rendu, mais grâce aux optimisations internes de React, cela **n’impacte pas les performances** dans la plupart des cas.

---

## **7. Conclusion : Le State, Moteur de l’Interactivité en React**

🎯 **Le state est ce qui rend tes applications React interactives et dynamiques.**  
Avec `useState`, tu peux facilement gérer des :  
✅ **Valeurs simples** (compteurs, champs de texte)  
✅ **Listes et objets complexes**  
✅ **Interactions utilisateur en temps réel**

**🚀 Prochaine étape : Apprendre à gérer les effets secondaires avec `useEffect` pour aller encore plus loin.**  
👉 **Lis l’article suivant : "L'arbre des composants en React et bien comprendre son utilisation".**

# **L'Arbre de Composants en React : Comprendre la Structure de ton Application**

Après avoir exploré le fonctionnement de `useState` et la gestion des rendus en React, il est temps de plonger dans une notion clé pour bien comprendre comment React structure et met à jour une application : **l’arbre de composants**.

Mais qu’est-ce que c’est exactement ? 🤔  
L’arbre de composants, c’est la **structure hiérarchique** qui représente **comment tes composants sont organisés et imbriqués** les uns dans les autres. C’est grâce à cet arbre que React sait **quels composants doivent être mis à jour** lorsque l’état change.

Dans cet article, nous allons voir :  
✅ **Ce qu’est un arbre de composants et comment il se forme**  
✅ **Comment les mises à jour du state affectent l’arbre**  
✅ **Le rôle clé du Virtual DOM dans la gestion de l’arbre**  
✅ **Des exemples concrets pour visualiser l’arbre de ton application**

---

## **1. Qu’est-ce que l’Arbre de Composants en React ?**

Un **arbre de composants** en React est une représentation de l’interface utilisateur sous forme de **nœuds imbriqués**.  
Chaque composant que tu crées devient un **nœud** dans cet arbre, et si un composant en contient d’autres, ils deviennent ses **enfants**.

### **📌 Exemple Visuel :**

Prenons un exemple simple avec trois composants : `App`, `Header` et `Footer`.

```jsx
function Header() {
  return <h1>Bienvenue sur mon site !</h1>;
}

function Footer() {
  return <footer>© 2024 Mon Site</footer>;
}

function App() {
  return (
    <div>
      <Header />
      <p>Ceci est le contenu principal.</p>
      <Footer />
    </div>
  );
}
```

### **🔍 L’Arbre de Composants généré ressemblerait à ceci :**

```
App
 ├── Header
 ├── <p>Ceci est le contenu principal.</p>
 └── Footer
```

**Chaque composant** est un **nœud** de l’arbre.

- `App` est la **racine** de l’arbre.
- `Header`, le paragraphe `<p>`, et `Footer` sont ses **enfants**.
- Si `Header` avait lui-même des composants imbriqués, ils deviendraient ses **sous-enfants**.

---

## **2. Comment le State Influence l’Arbre de Composants**

Lorsqu’un composant utilise `useState` et que son état change, React **détecte la modification** et décide **quels nœuds de l’arbre doivent être mis à jour**.

### **📌 Exemple :**

Ajoutons un bouton pour changer un titre dans le composant `Header` :

```jsx
import React, { useState } from "react";

function Header() {
  const [title, setTitle] = useState("Bienvenue sur mon site !");

  return (
    <div>
      <h1>{title}</h1>
      <button onClick={() => setTitle("Titre modifié !")}>
        Changer le titre
      </button>
    </div>
  );
}

function Footer() {
  return <footer>© 2024 Mon Site</footer>;
}

function App() {
  return (
    <div>
      <Header />
      <p>Ceci est le contenu principal.</p>
      <Footer />
    </div>
  );
}
```

### **Ce qui se passe dans l’arbre de composants :**

```
App
 ├── Header  ← Le titre change ici
 │     ├── <h1>Bienvenue sur mon site !</h1> → devient → <h1>Titre modifié !</h1>
 │     └── <button>Changer le titre</button>
 ├── <p>Ceci est le contenu principal.</p>  ← Ne change pas
 └── Footer  ← Ne change pas
```

### **🔍 Analyse :**

- Quand tu cliques sur le bouton, **seul le composant `Header` et ses enfants sont réexécutés**.
- Les composants `App` et `Footer` **ne sont pas affectés** parce qu’ils n’ont pas de dépendance directe avec le state de `Header`.

---

## **3. Le Virtual DOM et l’Arbre de Composants**

Le **Virtual DOM** joue un rôle crucial dans la gestion de l’arbre de composants. Lorsqu’un composant change d’état :

1. React **réexécute le composant** pour générer un **nouvel arbre de composants virtuel**.
2. Il compare cet arbre avec la **version précédente** (c’est ce qu’on appelle le **diffing**).
3. React détermine **quels nœuds ont changé** et met à jour **uniquement ces parties** dans le DOM réel.

### **📌 Exemple de Diffing :**

Avant le clic :

```
<h1>Bienvenue sur mon site !</h1>
```

Après le clic :

```
<h1>Titre modifié !</h1>
```

Le Virtual DOM détecte que **seul le contenu de `<h1>`** a changé et **n’affecte pas le reste de l’arbre**.

---

## **4. Pourquoi Comprendre l’Arbre de Composants est Important ?**

### **1. Optimiser les Performances**

Comprendre comment React gère l’arbre de composants t’aide à éviter des **rendus inutiles**. Par exemple, en structurant bien ton application, tu peux faire en sorte que **seuls les composants nécessaires** soient réexécutés.

### **2. Mieux Déboguer**

Lorsque tu comprends l’arbre, tu peux plus facilement savoir **pourquoi un composant se réexécute**. Parfois, un changement de state dans un parent peut entraîner le rendu de tous ses enfants, même s’ils n’ont pas changé.

### **3. Réutilisabilité et Maintenance**

Organiser ton arbre de manière claire permet de **réutiliser** des composants facilement et de **maintenir** ton code sans complexité.

---

## **5. Visualiser l’Arbre de Composants avec les DevTools React**

Pour mieux comprendre comment tes composants s’organisent, tu peux utiliser les **React Developer Tools**, une extension pour Chrome et Firefox.

### **Comment l’installer ?**

1. Va sur le [Chrome Web Store](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) ou le [site de Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/).
2. Installe l’extension et recharge ta page React.
3. Dans l’onglet **Components** des DevTools, tu verras **l’arbre de tes composants** en temps réel.

---

## **6. Optimiser l’Arbre de Composants : Bonnes Pratiques**

### **1. Découpe ton interface en petits composants**

Plus tes composants sont petits et spécialisés, plus React peut **isoler les mises à jour** et éviter des réexécutions globales.

### **2. Utilise `React.memo` pour éviter des rendus inutiles**

`React.memo` empêche un composant de se réexécuter si ses **props** n’ont pas changé.

```jsx
const StaticFooter = React.memo(function Footer() {
  return <footer>© 2024 Mon Site</footer>;
});
```

### **3. Privilégie les états locaux plutôt que globaux**

Si un état ne concerne qu’un composant, garde-le **localement** avec `useState` plutôt que de le propager à travers plusieurs niveaux de l’arbre.

---

## **7. Conclusion : L’Arbre de Composants, la Clé pour Comprendre React**

🎯 **L’arbre de composants est au cœur de la logique de React**. Chaque composant représente un nœud, et lorsqu’une modification d’état se produit, React :  
✅ **Réexécute les composants concernés** pour générer un nouvel arbre virtuel.  
✅ **Compare l’ancien et le nouvel arbre** pour détecter les changements.  
✅ **Met à jour uniquement les nœuds nécessaires** dans le DOM réel grâce au Virtual DOM.

En comprenant cette structure, tu peux non seulement **déboguer plus facilement** tes applications mais aussi **optimiser leurs performances**.

---

**🚀 Prochaine étape : Gérer les effets secondaires et les requêtes API avec `useEffect` pour aller encore plus loin dans le développement React.**  
👉 **Lis l’article suivant : "Comprendre `useEffect` en React : Gérer les Effets et les Requêtes API".**
