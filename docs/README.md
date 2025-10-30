# IA Challenge - HTML & CSS : la suite

Cet atelier est la suite de l’atelier [IA Challenge - HTML & CSS - partie 1](https://wildcodeschool.github.io/workshop-dev-ia-challenge-html-css-part-1/).  
{: .text-center}

Dans l'atelier précédent, tu as analysé le code HTML et CSS produit par une IA à partir d'un prompt donné et d'une image de référence.
Tu as pu identifier les points à améliorer concernant la sémantique HTML, l'accessibilité et la maintenabilité du code CSS.  
Peut-être as-tu solicité l'aide d'une IA pour t'aider dans cette analyse. Et là c'est le drame : l'IA t'a de nouveau prodigué des conseils pertinents mais également des recommandations erronées ou incomplètes.

**Cet atelier se présente en deux parties** :
1. Retour sur l’analyse de l’IA
2. Correction du code initial en tenant compte des bonnes recommandations



## 🕵  Retour sur l'analyse de l'IA
 
<details markdown="1">
<summary>Code HTML fourni pour analyse</summary>
Nettoyage effectué en amont par rapport au [code initial](https://github.com/WildCodeSchool/workshop-dev-ia-challenge-html-css-part-1/blob/main/index.html) :
- L'image de l'ordinateur portable (`.product-image`) réalisée en CSS a été remplacée par une image réelle.
- Le CSS a été extrait dans un fichier séparé.

```html
<!DOCTYPE html>
<html lang="fr">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>UltraBook Pro X - High-Performance Laptop</title>
  <link rel="stylesheet" href="./style.css">
</head>

<body>
  <div class="product-card">
    <div class="hot-sale-badge">HOT SALE</div>

    <img class="product-image" src="https://picsum.photos/id/0/600/400">


    <div class="product-info">
      <div class="product-category">HIGH-PERFORMANCE LAPTOP</div>
      <h1 class="product-title">UltraBook Pro X</h1>
      <p class="product-description">
        Cutting-edge performance with Intel Core i9, 32GB RAM, and a 1TB SSD in a sleek, lightweight design.
      </p>

      <div class="product-features">
        <div class="feature">
          <span class="feature-highlight">4K Display</span>
        </div>
        <div class="feature">
          <span class="feature-highlight">16-Hour Battery</span>
        </div>
        <div class="feature">
          <span class="feature-highlight">Thunderbolt 4</span>
        </div>
      </div>

      <div class="pricing-section">
        <div class="price-container">
          <span class="original-price">$2,499</span>
          <span class="current-price">$1,999</span>
        </div>
        <button class="add-to-cart-btn">
          <span class="cart-icon"></span>
          Add to Cart
        </button>
      </div>

      <div class="product-rating">
        <div class="stars">
          <div class="star"></div>
          <div class="star"></div>
          <div class="star"></div>
          <div class="star"></div>
          <div class="star"></div>
          <span class="reviews-count">245 Reviews</span>
        </div>
        <div class="stock-status">In Stock</div>
      </div>
    </div>
  </div>
</body>

</html>
```
</details>
>_Extrait d'une analyse réalisée par [ChatGPT - GPT-5](https://chatgpt.com/) (octobre 2025). Seuls quelques points sont présentés ici_.  
>### 🧩 Analyse HTML  
⚠️ Points à améliorer
1. Balises sémantiques
- Tu utilises uniquement des `<div>`. Cela rend le code moins accessible et moins explicite pour les lecteurs d’écran ou les moteurs de recherche.
- 👉 Amélioration :
  - `<article>` pour `.product-card`
  - `<header>` pour l’image et le badge
  - `<section>` pour les blocs “features”, “pricing” et “rating”
  - `<button>` correct, mais ajouter un `<span aria-hidden="true">` pour l’icône
1. Accessibilité
   - L’image n’a pas d’attribut `alt`.
   - Les étoiles sont décoratives mais pas lisibles pour un lecteur d’écran (elles devraient avoir un `aria-label` ou être remplacées par un texte caché indiquant la note).
   - Le bouton “**Add to Cart**” ne contient pas de texte alternatif pour l’icône du panier.
   - 👉 Amélioration
   ```html
    <img src="..." alt="UltraBook Pro X laptop front view">
    <div class="stars" aria-label="Rated 5 out of 5 stars">...</div>
  ```

#### Les erreurs ou imprécisions dans cette analyse  
1. **Balises sémantiques**. L'icône `<span class="cart-icon"></span>` dans le bouton "**Add to Cart**" n'a pas besoin de l'attribut `aria-hidden="true"` car l'icône est positionnée uniquement en CSS. Elle est donc déjà ignorée des lecteurs d'écran et ne sera pas lue. Une optimisation CSS pourrait malgré tout être apportée pour supprimer cette balise inutile. 
2. **Accessibilité**. L'information de l'ancien prix n'est disponible que par la forme (texte barré). Il est donc nécessaire d'ajouter du texte caché pour les lecteurs d'écran, par exemple avec une `<span class="sr-only">` (screen reader only) autour de l'ancien prix.
```html
<span class="sr-only">Original price:</span> $2,499
```
Plus d'infos sur la [technique SR-only](https://webaim.org/techniques/css/invisiblecontent/).
3. **Accessibilité**. L'image de l'ordinateur ne contient pas d'information, elle est purement décorative. **L'attribut `alt` doit donc être présent MAIS vide (`alt=""`)** pour qu'elle soit ignorée par les lecteurs d'écran.
4. **Accessibilité**. Les étoiles apportent, quant à elles, une information importante : la note du produit. Elles ne sont donc pas décoratives. L'information doit être restituée. Cependant, l'attribut `aria-label` est habituellement réservé à des éléments interactifs (boutons, liens, champs de formulaires) ou à des images. Il doit être utilisé avec précaution sur des éléments non interactifs. Ici, il pourrait rentrer en conflit avec la mention textuelle "**245 Reviews**" qui suit les étoiles.
Une approche complète serait de regrouper les informations (note du produit + nombre d'avis) dans cette `<div>` à laquelle on pourrait ajouter `role="img"` afin que l'ensemble soit lu correctement par les lecteurs d'écran.
  ```html
   <div class="stars" role="img" aria-label="Average rating: 5 out of 5, based on 245 reviews">...</div>
  ```

<details markdown="1">
<summary>Code CSS fourni pour analyse</summary>
Nettoyage effectué en amont par rapport au [code initial](https://github.com/WildCodeSchool/workshop-dev-ia-challenge-html-css-part-1/blob/main/index.html) :
- L'image de l'ordinateur portable (`.product-image`) réalisée en CSS a été remplacée par une image réelle avec des propriétés CSS (`width`, `height`, `object-fit` définies).

```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
}

.product-card {
  max-width: 720px;
  background: white;
  border-radius: 24px;
  box-shadow: 0 30px 60px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  position: relative;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.product-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 40px 80px rgba(0, 0, 0, 0.15);
}

.hot-sale-badge {
  position: absolute;
  top: 20px;
  right: 20px;
  background: linear-gradient(135deg, #e53e3e, #c53030);
  color: white;
  padding: 8px 20px;
  border-radius: 20px;
  font-weight: 700;
  font-size: 12px;
  letter-spacing: 1px;
  z-index: 10;
  box-shadow: 0 4px 15px rgba(229, 62, 62, 0.4);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
  }

  50% {
    transform: scale(1.05);
  }
}

.product-image {
  width: 100%;
  height: 280px;
  object-fit: cover;
}

.product-info {
  padding: 40px;
}

.product-category {
  color: #718096;
  font-size: 14px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 10px;
}

.product-title {
  font-size: 36px;
  font-weight: 800;
  color: #1a202c;
  margin-bottom: 20px;
  line-height: 1.2;
}

.product-description {
  color: #4a5568;
  font-size: 18px;
  line-height: 1.6;
  margin-bottom: 30px;
}

.product-features {
  display: flex;
  gap: 30px;
  margin-bottom: 30px;
  flex-wrap: wrap;
}

.feature {
  text-align: center;
  color: #718096;
  font-weight: 500;
  font-size: 14px;
}

.feature-highlight {
  display: block;
  color: #2d3748;
  font-weight: 700;
  font-size: 16px;
  margin-bottom: 4px;
}

.pricing-section {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 25px;
}

.price-container {
  display: flex;
  align-items: baseline;
  gap: 15px;
}

.original-price {
  color: #a0aec0;
  text-decoration: line-through;
  font-size: 18px;
}

.current-price {
  color: #1a202c;
  font-size: 42px;
  font-weight: 900;
}

.add-to-cart-btn {
  background: linear-gradient(135deg, #2d3748, #1a202c);
  color: white;
  border: none;
  padding: 15px 30px;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 10px;
  box-shadow: 0 10px 25px rgba(45, 55, 72, 0.3);
}

.add-to-cart-btn:hover {
  background: linear-gradient(135deg, #1a202c, #0f1419);
  transform: translateY(-2px);
  box-shadow: 0 15px 35px rgba(45, 55, 72, 0.4);
}

.cart-icon {
  width: 20px;
  height: 20px;
  background: currentColor;
  mask: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-1.1 5.4M7 13v6a2 2 0 002 2h6a2 2 0 002-2v-6m-8 0h8"/></svg>') center/contain no-repeat;
  -webkit-mask: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 3h2l.4 2M7 13h10l4-8H5.4m0 0L7 13m0 0l-1.1 5.4M7 13v6a2 2 0 002 2h6a2 2 0 002-2v-6m-8 0h8"/></svg>') center/contain no-repeat;
}

.product-rating {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.stars {
  display: flex;
  gap: 2px;
}

.star {
  width: 20px;
  height: 20px;
  background: linear-gradient(135deg, #f6e05e, #ecc94b);
  clip-path: polygon(50% 0%, 61% 35%, 98% 35%, 68% 57%, 79% 91%, 50% 70%, 21% 91%, 32% 57%, 2% 35%, 39% 35%);
}

.reviews-count {
  color: #718096;
  font-size: 14px;
  margin-left: 10px;
}

.stock-status {
  color: #38a169;
  font-weight: 600;
  font-size: 14px;
}

@media (max-width: 768px) {
  .product-card {
    margin: 10px;
  }

  .product-info {
    padding: 30px 20px;
  }

  .product-title {
    font-size: 28px;
  }

  .product-features {
    gap: 15px;
  }

  .pricing-section {
    flex-direction: column;
    align-items: flex-start;
    gap: 20px;
  }

  .current-price {
    font-size: 32px;
  }
}
```
</details>
>### 🎨 Analyse CSS
1. Manque de variables CSS
- Couleurs réutilisées en dur (`#1a202c`, `#718096`, `#2d3748`…).
1. Hiérarchie et maintenabilité
- Les sélecteurs sont très plats (ex: `.product-title`, `.product-description`), mais pourraient être mieux contextualisés avec un BEM-like :
  - `.product-card__title`
  - `.product-card__feature`
2. Accessibilité des contrastes
   - Certains textes gris clair sur fond blanc (`#718096` sur `#fff`) ont un contraste un peu faible.

#### Précisions complémentaires sur cette analyse
1. **Hiérarchie et maintenabilité**. L'analyse est correcte. Il est aussi possible d'utiliser le [CSS nesting](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_nesting/Using_CSS_nesting) pour améliorer la lisibilité.
2. **Accessibilité des contrastes**. L'analyse est correcte. Le contraste entre le texte gris clair et le fond blanc est en effet un peu faible. Il y a aussi le texte vert clair (`#38a169`) sur fond blanc (`In Stock`) dont le contraste est insuffisant.
