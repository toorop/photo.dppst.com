# Roadmap d'optimisation pour la galerie photo

## Optimisations actuelles
- Lazy loading des images avec IntersectionObserver
- Utilisation de placeholders flous de petite taille (10px)
- Adaptation de la taille des métadonnées EXIF sur mobile
- Préchargement des images avant qu'elles n'entrent dans la vue (rootMargin: 200px)

## Optimisations à implémenter

### 1. Nettoyage du code
- Supprimer le fichier `gallery-item.html` vide qui n'est pas utilisé
- Nettoyer les commentaires de test et le code inutilisé
- Consolider les styles CSS (certains sont dupliqués entre le CSS et le JavaScript)

### 2. Optimisation des performances
- **Formats d'image modernes** : Générer et servir des images WebP avec fallback JPEG
  ```html
  <picture>
    <source srcset="{{ $image.Resize "800x webp" }}" type="image/webp">
    <img src="{{ $placeholder.RelPermalink }}" data-src="{{ $thumbnail.RelPermalink }}" ...>
  </picture>
  ```

- **Optimisation des placeholders** : Utiliser des placeholders LQIP (Low Quality Image Placeholders) encore plus petits
  ```go
  {{ $placeholder := $image.Resize "20x jpg q20" }}
  ```

- **Chargement prioritaire** : Prioriser le chargement des images visibles dans la première vue
  ```javascript
  const isPriority = index < 6; // Les 6 premières images
  if (isPriority) {
    img.fetchPriority = "high";
  }
  ```

- **Préchargement adaptatif** : Ajuster dynamiquement le rootMargin en fonction de la vitesse de connexion
  ```javascript
  if ('connection' in navigator) {
    const connection = navigator.connection;
    const rootMargin = connection.effectiveType.includes('4g') ? '400px' : '100px';
    // Utiliser rootMargin dans l'IntersectionObserver
  }
  ```

### 3. Améliorations UX
- **Indicateur de chargement** : Ajouter un spinner ou une barre de progression pendant le chargement des images
- **Animation de transition** : Améliorer la transition entre placeholder et image complète
  ```css
  .placeholder {
    filter: blur(10px);
    transform: scale(1.05);
    transition: filter 0.5s ease-out, transform 0.5s ease-out;
  }
  
  .placeholder.loaded {
    filter: blur(0);
    transform: scale(1);
  }
  ```

- **Gestion des erreurs** : Ajouter un fallback en cas d'échec de chargement d'une image
  ```javascript
  tempImg.onerror = function() {
    img.src = '/images/error-placeholder.jpg';
    observer.unobserve(img);
  };
  ```

### 4. Optimisation SEO et accessibilité
- **Attributs alt descriptifs** : Améliorer les attributs alt des images (actuellement seulement le nom du fichier)
- **Lazy loading natif** : Ajouter l'attribut `loading="lazy"` en complément de notre solution personnalisée
- **Attributs width/height explicites** : S'assurer que toutes les images ont des dimensions explicites pour éviter le CLS (Cumulative Layout Shift)

### 5. Optimisation de Masonry
- **Réduction des recalculs** : Limiter les appels à `masonry.layout()` en les regroupant
  ```javascript
  // Utiliser un debounce pour limiter les recalculs
  let layoutTimeout;
  function debouncedLayout() {
    clearTimeout(layoutTimeout);
    layoutTimeout = setTimeout(() => {
      masonry.layout();
    }, 100);
  }
  ```

- **Initialisation progressive** : Initialiser Masonry avec un petit nombre d'images, puis ajouter les autres progressivement

### 6. Optimisation de lightgallery
- **Chargement différé** : Charger lightgallery uniquement lorsque l'utilisateur interagit avec la galerie
  ```javascript
  let lg = null;
  grid.addEventListener('click', function(e) {
    if (!lg && e.target.closest('.gallery-item')) {
      // Charger lightgallery à la demande
      import('lightgallery').then(module => {
        const lightGallery = module.default;
        lg = lightGallery(grid, { /* options */ });
        // Déclencher l'ouverture
      });
    }
  }, { once: true });
  ```

- **Préchargement sélectif** : Précharger uniquement les images adjacentes dans lightgallery

### 7. Optimisation pour les appareils mobiles
- **Tailles d'images adaptatives** : Servir des images de taille différente selon l'appareil
  ```html
  <img srcset="{{ $image.Resize "400x" }} 400w, {{ $image.Resize "800x" }} 800w, {{ $image.Resize "1200x" }} 1200w"
       sizes="(max-width: 600px) 400px, (max-width: 1200px) 800px, 1200px"
       src="{{ $placeholder.RelPermalink }}"
       data-src="{{ $thumbnail.RelPermalink }}"
       ...>
  ```

- **Économie de données** : Détecter la préférence d'économie de données et adapter la qualité des images
  ```javascript
  if ('connection' in navigator && navigator.connection.saveData) {
    // Charger des images de qualité inférieure
  }
  ```

## Calendrier d'implémentation suggéré

### Phase 1 (Court terme)
- Nettoyage du code
- Optimisation des placeholders
- Amélioration des transitions

### Phase 2 (Moyen terme)
- Formats d'image modernes (WebP)
- Optimisation de Masonry
- Gestion des erreurs

### Phase 3 (Long terme)
- Chargement différé de lightgallery
- Optimisation pour les appareils mobiles
- Économie de données 