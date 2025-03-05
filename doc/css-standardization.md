# Standardisation CSS du projet

## Problèmes résolus

1. **Mélange d'approches pour l'utilisation de Tailwind CSS**
   - Utilisation incohérente des classes Tailwind directement dans le HTML
   - Utilisation partielle des composants personnalisés via `@layer components`

2. **Incohérences dans les styles**
   - Certains éléments utilisaient des classes personnalisées
   - D'autres éléments utilisaient directement des classes Tailwind

3. **Configuration incomplète**
   - Absence du plugin `@tailwindcss/typography` alors que des classes `prose` étaient utilisées

## Modifications apportées

### 1. Configuration Tailwind

- Ajout du plugin `@tailwindcss/typography` dans `tailwind.config.js`
- Amélioration de la configuration pour la purge CSS

### 2. Optimisation PostCSS

- Ajout de `cssnano` pour la minification CSS en production
- Configuration conditionnelle pour n'appliquer la minification qu'en production

### 3. Standardisation des composants CSS

- Création de composants pour tous les éléments répétitifs
- Organisation des composants par catégories (galerie, navigation, contenu, etc.)
- Utilisation de noms de classes sémantiques et cohérents

### 4. Mise à jour des templates

- Remplacement des classes Tailwind directes par des classes de composants
- Mise à jour des partiels (exif.html, share.html) pour utiliser les nouvelles classes
- Mise à jour du template single.html pour utiliser les nouvelles classes

### 5. Amélioration des scripts

- Ajout d'un script de watch pour le CSS
- Intégration de la génération CSS dans le processus de build Hugo
- Ajout d'un script combiné pour le développement

## Comment utiliser

### Installation des dépendances

```bash
npm install
```

### Développement

```bash
# Pour lancer le serveur Hugo avec rechargement automatique du CSS
npm run dev:all

# Pour générer le CSS uniquement
npm run build:css

# Pour surveiller les changements CSS
npm run watch:css
```

### Production

```bash
# Pour construire le site pour la production
npm run build
```

## Bonnes pratiques à suivre

1. **Utiliser les composants personnalisés** plutôt que les classes Tailwind directement dans le HTML
2. **Ajouter de nouveaux composants** dans main.css plutôt que d'ajouter des classes Tailwind dans le HTML
3. **Organiser les composants** par catégories pour faciliter la maintenance
4. **Générer le CSS** avant de construire le site avec Hugo
