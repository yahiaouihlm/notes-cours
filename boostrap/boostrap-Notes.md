# <h1 align="center"> Bootstrap </h1>
Bootstrap est un framework CSS open source qui permet de créer facilement des interfaces web modernes, responsives et cohérentes. Il a été développé initialement par Twitter.

# 📚 Composants de Bootstrap

Bootstrap est un framework CSS qui fournit des composants prêts à l'emploi pour créer des interfaces web modernes et responsives.

---
## ✅ 1. Grid (`.grid`)
Bootstrap divise l’écran horizontalement en 12 parties égales, appelées colonnes. Cela permet de créer des mises en page flexibles et responsives.

- `container` : Élément principal qui contient la grille (row et col).  Par défaut, container a une largeur maximale (max-width) fixe qui change selon la taille de l'écran (responsive).


- `container-fluid` : Similaire à container, mais prend 100% de la largeur de l’écran quelle que soit la taille, Idéal pour des mises en page pleine largeur.

    - `container-{breakpoint} (optionnel)` :Bootstrap propose aussi des containers intermédiaires avec des points de rupture (breakpoints) , Cela permet de contrôler à partir de quand le container doit cesser de prendre toute la largeur de l’écran.
        | Classe          | Comportement                             |
        | --------------- | ---------------------------------------- |
        | `container-sm`  | 100% jusqu’à `576px`, puis largeur fixe  |
        | `container-md`  | 100% jusqu’à `768px`, puis largeur fixe  |
        | `container-lg`  | 100% jusqu’à `992px`, puis largeur fixe  |
        | `container-xl`  | 100% jusqu’à `1200px`, puis largeur fixe |
        | `container-xxl` | 100% jusqu’à `1400px`, puis largeur fixe |

- `container-xl` : Le conteneur aura une largeur fluide (100%) jusqu’au breakpoint xl (≥1200px), puis une largeur fixe à partir de là.

  📦 **Maintenant, dans Bootstrap :**

    | Classe Bootstrap   | Largeur maximale               | Comportement                      |
    | ------------------ | ------------------------------ | --------------------------------- |
    | `.container`       | **fixe** selon le *breakpoint* | centrée, avec marges automatiques |
    | `.container-fluid` | **toujours 100%**              | remplit tout l’écran              |

---

- `row` : sert à organiser horizontalement les colonnes (.col-*) à l’intérieur d’un conteneur (.container, .container-fluid, etc.), Créer une ligne pour accueillir les colonnes (.col) et gérer correctement leur alignement et espacement, Elle utilise `display: flex` pour aligner les colonnes sur une ligne horizontale.**On définit plusieurs .row dans un .container quand on veut créer plusieurs lignes distinctes, chacune avec son propre agencement de colonnes**

- `col` :  est une colonne dans le système de grille Bootstrap. Elle représente une portion de la largeur disponible sur une ligne (.row).
    - **⚙️ Fonctionnement de base :** Bootstrap divise chaque ligne en 12 unités.
    Tu peux choisir combien de colonnes un élément prend, avec des classes comme :
        - `.col` → prend automatiquement la place restante (flexible) 
        - `.col-6` → prend 6/12 (soit 50% de la ligne)
        - `.col-4` → prend 4/12 (soit 33.33%)

    - **Fonctionnement col-{breakpoint}-{n}** : Tu peux adapter la taille des colonnes selon la taille de l'écran :
        - exemple : `col-sm-6 col-lg-4` : ségniie que 
            - Sur un écran petit `(sm ≥ 576px)`, l'élément occupe 6 colonnes sur 12 (soit 50% de la ligne).
            - Sur un écran large `(lg ≥ 992px)` ou plus grand, il occupe 4 colonnes sur 12 (soit environ 33%).







---
## ✅ 1. Alertes (`.alert`)

Messages colorés pour afficher des informations, succès, erreurs, etc.

```html
<div class="alert alert-success">Succès !</div>
<div class="alert alert-danger">Erreur !</div>
```

---

## 🔘 2. Boutons (`.btn`)

Boutons stylisés avec différentes couleurs et styles.

```html
<button class="btn btn-primary">Envoyer</button>
<button class="btn btn-outline-secondary">Annuler</button>
```

---

## 🧭 3. Navbar (barre de navigation)

Barre de navigation responsive avec logo ou liens.

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="#">MonSite</a>
</nav>
```

---

## 📝 4. Formulaires

Styles Bootstrap pour les champs, boutons, validations, etc.

```html
<form>
  <div class="mb-3">
    <label for="email" class="form-label">Email</label>
    <input type="email" class="form-control" id="email">
  </div>
</form>
```

---

## 📌 5. Modals (fenêtres pop-up)

Fenêtres flottantes pour dialogues, messages ou formulaires.

```html
<div class="modal fade" id="exampleModal">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Titre</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body">
        Contenu de la modal.
      </div>
    </div>
  </div>
</div>
```

---

## 🔄 6. Carousel (diaporama)

Diaporama pour images ou contenu.

```html
<div id="carouselExample" class="carousel slide">
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="img1.jpg" class="d-block w-100" alt="...">
    </div>
    <div class="carousel-item">
      <img src="img2.jpg" class="d-block w-100" alt="...">
    </div>
  </div>
</div>
```

---

## 🔽 7. Dropdowns (menus déroulants)

Menus déroulants pour actions ou liens.

```html
<div class="dropdown">
  <button class="btn btn-secondary dropdown-toggle" data-bs-toggle="dropdown">
    Menu
  </button>
  <ul class="dropdown-menu">
    <li><a class="dropdown-item" href="#">Action</a></li>
    <li><a class="dropdown-item" href="#">Autre action</a></li>
  </ul>
</div>
```

---

## ✅ 8. Badges

Petits indicateurs pour notifications ou statuts.

```html
<span class="badge bg-primary">Nouveau</span>
<span class="badge bg-danger">Alerte</span>
```

---

## 📊 9. Progress Bars (barres de progression)

Barres indiquant une progression.

```html
<div class="progress">
  <div class="progress-bar" style="width: 60%;">60%</div>
</div>
```

---

## 🧱 10. Cards

Blocs visuels pour afficher contenu structuré.

```html
<div class="card" style="width: 18rem;">
  <div class="card-body">
    <h5 class="card-title">Titre</h5>
    <p class="card-text">Texte de la carte.</p>
    <a href="#" class="btn btn-primary">Action</a>
  </div>
</div>
```


##  types  d'ecrans  

| Nom             | Préfixe (classes) | Largeur min. | Exemple d’appareil     |
| --------------- | ----------------- | ------------ | ---------------------- |
| **Extra small** | (aucun préfixe)   | `0px`        | Téléphones très petits |
| **Small**       | `sm`              | `576px`      | Téléphones (portrait)  |
| **Medium**      | `md`              | `768px`      | Tablettes              |
| **Large**       | `lg`              | `992px`      | Ordinateurs portables  |
| **Extra large** | `xl`              | `1200px`     | Ordinateurs de bureau  |
| **XXL**         | `xxl`             | `1400px`     | Très grands écrans     |
