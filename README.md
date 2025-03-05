# Galerie Photo Hugo

Un site de galerie photo statique construit avec Hugo et TailwindCSS.

## Fonctionnalités

- Galerie photo responsive avec layout masonry adaptatif
- Mode sombre/clair avec bouton de bascule
- Affichage des métadonnées EXIF
- Lightbox pour la visualisation des photos avec diaporama automatique
- Optimisation des images (WebP, lazy loading)
- Partage sur les réseaux sociaux
- Interface utilisateur moderne et élégante
- Mise en page adaptative pour tous les appareils

## Technologies utilisées

- **Hugo** : Générateur de site statique
- **TailwindCSS** : Framework CSS utilitaire
- **PostCSS** : Processeur CSS
- **Masonry** : Bibliothèque JavaScript pour la mise en page en mosaïque
- **LightGallery** : Bibliothèque JavaScript pour la visualisation des photos
- **imagesLoaded** : Bibliothèque JavaScript pour la gestion du chargement des images

## Installation

1. Cloner le dépôt
2. Installer les dépendances Node.js :
   ```bash
   npm install
   ```
3. Compiler les styles CSS :
   ```bash
   npm run build:css
   ```
4. Lancer le serveur de développement :
   ```bash
   npm run dev
   ```

## Ajouter un nouvel album

1. Créer un nouvel album avec l'archetype dédié :
   ```bash
   hugo new --kind album voyages-2024
   ```
   Cette commande va créer :
   - Le dossier `content/voyages-2024/`
   - Un fichier `index.md` pré-rempli avec :
     * Titre (basé sur le nom du dossier)
     * Date
     * Tags (à remplir)
     * Description (à remplir)
     * Autres métadonnées nécessaires

2. Personnaliser le fichier index.md généré :
   ```markdown
   ---
   title: "Voyages 2024"
   date: 2025-03-02
   draft: false
   gallery: true
   tags: ["voyage", "nature"]  # Ajouter vos tags
   weight: 1  # Pour l'ordre d'affichage (plus petit = plus haut)
   description: "Mes voyages en 2024"  # Courte description
   ---

   Description détaillée de l'album...
   ```

3. Ajouter les photos :
   - Copier les photos directement dans le dossier de l'album
   - Formats supportés : JPG, JPEG
   - Les photos seront automatiquement :
     * Redimensionnées pour les vignettes
     * Optimisées pour le web
     * Affichées dans l'ordre alphabétique de leurs noms

4. Tester l'album :
   ```bash
   hugo server -D
   ```
   Visiter http://localhost:1313 pour voir le résultat

## Conseils pour les photos

- **Nommage des fichiers** : Utilisez des noms descriptifs et sans espaces, par exemple : `coucher-soleil-paris.jpg`
- **Taille des images** : Les images originales peuvent être de grande taille, Hugo les optimisera automatiquement
- **Métadonnées EXIF** : Les métadonnées EXIF sont automatiquement extraites et affichées (appareil photo, paramètres de prise de vue, etc.)
- **Ordre d'affichage** : Pour contrôler l'ordre des photos, préfixez les noms avec des numéros, par exemple : `01-premiere-photo.jpg`
- **Formats de photos** : La galerie gère correctement les photos en format portrait et paysage, les présentant de manière esthétique dans une mosaïque adaptative

## Structure des contenus

Les galeries photos sont organisées dans le dossier `content/`. Chaque dossier à la racine de `content/` représente un album distinct.

## Configuration

Le fichier `hugo.toml` contient la configuration principale du site, y compris :
- Les paramètres d'optimisation des images
- Les champs EXIF à extraire
- Les paramètres de cache et de sécurité

### Paramètres d'optimisation des images

Les paramètres suivants dans `hugo.toml` contrôlent l'optimisation des images :

- `quality` : Définit la qualité de la compression JPEG (par défaut : 85). Une valeur plus basse réduit la taille du fichier, mais peut affecter la qualité de l'image.
- `resampleFilter` : Spécifie le filtre de rééchantillonnage utilisé lors du redimensionnement des images (par défaut : "Lanczos").
- `anchor` : Définit le point d'ancrage pour le recadrage des images (par défaut : "Smart").

## Optimisation

Les images sont automatiquement :
- Redimensionnées pour différents usages (vignettes, affichage, plein écran)
- Converties en WebP avec fallback JPEG
- Chargées en lazy loading
- Mises en cache via les headers HTTP

## Mise en page et style

Le site utilise TailwindCSS avec le plugin Typography pour une mise en page élégante et cohérente. Les styles principaux sont définis dans `assets/css/main.css` et sont compilés avec PostCSS.

### Mosaïque adaptative

La galerie utilise Masonry pour créer une mosaïque adaptative qui :
- S'ajuste automatiquement en fonction de la taille des images
- Préserve les proportions originales des photos
- S'adapte à différentes tailles d'écran (responsive)
- Crée un rendu esthétique quelle que soit l'orientation des photos

### Mode sombre/clair

Le site propose un mode sombre et un mode clair, avec un bouton de bascule en haut à droite de l'écran. Le choix de l'utilisateur est enregistré dans le localStorage pour être conservé lors des visites ultérieures.
