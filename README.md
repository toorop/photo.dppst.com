# Galerie Photo Hugo

Un site de galerie photo statique construit avec Hugo et TailwindCSS.

## Fonctionnalités

- Galerie photo responsive avec layout masonry
- Mode sombre/clair
- Affichage des métadonnées EXIF
- Lightbox pour la visualisation des photos
- Optimisation des images (WebP, lazy loading)
- Partage sur les réseaux sociaux

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

## Structure des contenus

Les galeries photos sont organisées dans le dossier `content/`. Chaque dossier à la racine de `content/` représente un album distinct.

## Configuration

Le fichier `hugo.toml` contient la configuration principale du site, y compris :
- Les paramètres d'optimisation des images
- Les champs EXIF à extraire
- Les paramètres de cache et de sécurité

## Optimisation

Les images sont automatiquement :
- Redimensionnées pour différents usages (vignettes, affichage, plein écran)
- Converties en WebP avec fallback JPEG
- Chargées en lazy loading
- Mises en cache via les headers HTTP
