---
author: Julien POIRIER
pubDatetime: 2025-02-06T07:00:00.00Z
modDatetime: 2025-02-06T07:00:00.00Z
title: Comprendre et utiliser .map avec des composants modulaires
slug: composant-map-et-les-listes-en-react
featured: true
draft: false
tags:
  - react
  - debutant
description: Nous verrons comment rapidement mettre une place un projet React avec le tooling de Vite
---

# **Comprendre et Utiliser `.map()` en React : Le Guide Ultime pour les Débutants**

En React, l’une des difficultés courantes pour les débutants est de comprendre **comment afficher une liste d’éléments de manière efficace**. C’est là qu’intervient la méthode `.map()`, un outil puissant pour générer dynamiquement des composants à partir de données.

Dans cet article, nous allons :

- **Définir ce que fait `.map()` en JavaScript et en React**
- **Voir comment l’utiliser pour afficher des listes dynamiquement**
- **Créer un composant réutilisable et modulaire**
- **Explorer un exemple concret avec des cartes de produits**
- **Ajouter une recherche dynamique pour un affichage interactif**

---

## **1. Qu’est-ce que `.map()` en JavaScript ?**

Avant de l’utiliser en React, il est important de comprendre le `.map()` en JavaScript natif.

### **Définition**

`.map()` est une méthode des tableaux en JavaScript qui permet de **transformer chaque élément d’un tableau en appliquant une fonction** et de retourner un **nouveau tableau** contenant les éléments modifiés.

### **Exemple simple en JavaScript**

```js
const nombres = [1, 2, 3, 4, 5];
const doubles = nombres.map(nombre => nombre * 2);
console.log(doubles); // [2, 4, 6, 8, 10]
```

Ici, on applique une fonction qui multiplie chaque nombre par 2. Le tableau `doubles` contient les nouveaux résultats.

---

## **2. `.map()` en React : Générer du JSX Dynamique**

React introduit une **particularité** : contrairement à JavaScript où `.map()` retourne un tableau de valeurs, **React rend directement un tableau d’éléments JSX dans le flux de la page**.

### **Cas sans `.map()` : Code répétitif**

Imaginons que nous devons afficher une liste d’utilisateurs.

```jsx
function UserList() {
  return (
    <div>
      <h2>Liste des utilisateurs</h2>
      <p>Utilisateur : Alice</p>
      <p>Utilisateur : Bob</p>
      <p>Utilisateur : Charlie</p>
    </div>
  );
}
```

**Problème** : Ce code est figé. Si on a 100 utilisateurs, il devient ingérable.

---

## **3. Utiliser `.map()` pour Gérer des Listes Dynamiques**

Plutôt que d’écrire chaque élément à la main, nous pouvons **utiliser `.map()`** pour générer automatiquement les balises JSX.

### **Exemple : Affichage d’une liste d’utilisateurs**

```jsx
function UserList() {
  const users = ["Alice", "Bob", "Charlie"];

  return (
    <div>
      <h2>Liste des utilisateurs</h2>
      {users.map((user, index) => (
        <p key={index}>Utilisateur : {user}</p>
      ))}
    </div>
  );
}
```

### **Explication**

1. **Le tableau `users` contient la liste des noms.**
2. **`.map()` parcourt chaque élément et retourne un `<p>` avec le nom de l’utilisateur.**
3. **React affiche directement ce tableau d’éléments JSX dans le DOM.**

> **💡 Astuce** : L’attribut `key={index}` est important pour aider React à optimiser le rendu (nous y reviendrons plus tard).

---

## **4. Extraire un Composant Réutilisable**

Afficher une simple liste est une bonne première étape, mais nous pouvons aller plus loin en **créant un composant réutilisable**.

### **Créer un Composant `<User />`**

```jsx
function User({ name }) {
  return <p>Utilisateur : {name}</p>;
}

function UserList() {
  const users = ["Alice", "Bob", "Charlie"];

  return (
    <div>
      <h2>Liste des utilisateurs</h2>
      {users.map((user, index) => (
        <User key={index} name={user} />
      ))}
    </div>
  );
}
```

### **Pourquoi faire ça ?**

✅ **Modularité** : Si demain on veut ajouter une icône, un style spécifique, ou d’autres infos (âge, email...), on modifie uniquement `<User />`.  
✅ **Code plus lisible et maintenable**.

---

## **5. Exemple Concret : Afficher une Liste de Cartes**

Un cas classique en développement web est **l’affichage d’une liste de produits sous forme de cartes**.

### **Exercice : Affichage de Produits**

```jsx
function Product({ name, price }) {
  return (
    <div className="product-card">
      <h3>{name}</h3>
      <p>Prix : {price} €</p>
    </div>
  );
}

function ProductList() {
  const products = [
    { id: 1, name: "Ordinateur", price: 999 },
    { id: 2, name: "Smartphone", price: 499 },
    { id: 3, name: "Casque Audio", price: 199 },
  ];

  return (
    <div>
      <h2>Produits en vente</h2>
      <div className="product-list">
        {products.map(product => (
          <Product key={product.id} name={product.name} price={product.price} />
        ))}
      </div>
    </div>
  );
}
```

### **Pourquoi c’est utile ?**

✔️ **On manipule des données dynamiques**  
✔️ **On évite la répétition manuelle**  
✔️ **On rend le code modulaire et facile à maintenir**

---

## **6. L’Importance de la `key` dans `.map()`**

React utilise les `key` pour **optimiser le rendu** et éviter les erreurs. Une `key` unique aide React à identifier **quel élément a changé, ajouté ou supprimé**.

### **Exemple d’erreur courante**

```jsx
{
  products.map(product => (
    <Product name={product.name} price={product.price} />
  ));
}
```

🚨 **React va afficher un warning car il manque `key={product.id}`**.

### **Pourquoi l'index n'est pas une bonne `key` dans `.map()` ?**

Il est tentant d’utiliser l’**index du tableau** comme `key` lorsque les objets n’ont pas d’identifiant unique. C’est une solution rapide et facile, mais elle **pose des problèmes** dans certaines situations.

#### **1. React et le Re-rendering**

React utilise les `key` pour identifier **de manière unique** chaque élément d’une liste et optimiser son rendu. Si les `key` changent de manière inattendue, React peut :

- **Mal réutiliser les composants**, ce qui entraîne un affichage incorrect.
- **Supprimer et recréer des éléments inutiles**, ce qui ralentit l’application.

#### **2. L'index change lorsque la liste est modifiée**

L’un des plus gros problèmes avec `key={index}` se produit **lorsqu’un élément est ajouté, supprimé ou réorganisé**.

Exemple :

```jsx
function UserList() {
  const [users, setUsers] = React.useState(["Alice", "Bob", "Charlie"]);

  return (
    <div>
      {users.map((user, index) => (
        <p key={index}>{user}</p>
      ))}
      <button onClick={() => setUsers(["David", ...users])}>
        Ajouter David
      </button>
    </div>
  );
}
```

🚨 **Problème** : Si un nouvel élément est inséré au début (`David`), tous les indices suivants sont décalés. **React va croire que tous les éléments ont changé**, alors qu’ils n’ont fait que se décaler.

#### **3. Mauvaise gestion de l’état des composants enfants**

Si chaque élément de la liste contient un **champ éditable ou un état local**, React peut mal le gérer si `key={index}` est utilisé.

Exemple :

```jsx
function EditableList() {
  const [items, setItems] = React.useState(["Item 1", "Item 2", "Item 3"]);

  return (
    <div>
      {items.map((item, index) => (
        <input key={index} defaultValue={item} />
      ))}
      <button onClick={() => setItems(["Nouvel Item", ...items])}>
        Ajouter un item
      </button>
    </div>
  );
}
```

**Bug possible** : En ajoutant un élément au début, l’entrée qui était liée à "Item 1" va rester à la première place **mais son état interne va suivre l’ancien index**, provoquant un comportement inattendu.

---

## **Bonne Pratique : Utiliser une ID Unique**

La meilleure approche est d’utiliser un **identifiant unique** pour chaque élément.

```jsx
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" },
];

{
  users.map(user => <p key={user.id}>{user.name}</p>);
}
```

✅ **Avantages** :

- Pas d’erreur lors des ajouts/suppressions.
- Meilleure optimisation du rendu par React.
- Meilleure gestion de l’état interne des composants.

  **Évite d’utiliser l’index comme `key`**, sauf si la liste est **totalement statique et ne changera jamais**. Pour toutes les autres situations, **préférez une clé unique issue des données**.

---

## **7. BONUS : Ajouter une Recherche Dynamique**

Pour rendre notre liste **interactive**, ajoutons un champ de recherche qui filtre les produits en **temps réel**.

### **Exercice : Ajouter un Filtre Dynamique**

```jsx
function ProductList() {
  const [search, setSearch] = React.useState("");
  const products = [
    { id: 1, name: "Ordinateur", price: 999 },
    { id: 2, name: "Smartphone", price: 499 },
    { id: 3, name: "Casque Audio", price: 199 },
  ];

  const filteredProducts = products.filter(product =>
    product.name.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div>
      <h2>Produits en vente</h2>
      <input
        type="text"
        placeholder="Rechercher un produit..."
        value={search}
        onChange={e => setSearch(e.target.value)}
      />
      <div className="product-list">
        {filteredProducts.map(product => (
          <Product key={product.id} name={product.name} price={product.price} />
        ))}
      </div>
    </div>
  );
}
```

### **Pourquoi c’est intéressant ?**

- On **gère les entrées utilisateur**
- On **filtre dynamiquement la liste**
- C’est une **base pour plein de projets interactifs**

---

## **Conclusion**

1. **`.map()` permet de générer dynamiquement du JSX à partir d’un tableau.**
2. **React affiche directement un tableau d’éléments JSX dans le flux du DOM.**
3. **Extraire un composant rend le code plus propre et réutilisable.**
4. **Avec `.map()`, on peut facilement créer des interfaces interactives et modulaires.**
5. **Utiliser une `key` unique est essentiel pour éviter les bugs et améliorer les performances.**

> **👉 Maintenant, à toi de jouer !** Essaie d’afficher une liste d’articles de blog avec `.map()`.
