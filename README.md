# Élections des délégués · Terminale générale 4

Site de campagne de **Kaïs BATTAIA PISZCZ** et **Michel Kostenko**, pour l’élection du 28 septembre 2026. Le vote se déroule en classe : ce site présente le binôme et ses quatre engagements, il ne recueille pas de vote.

**Site public :** https://elections-tg4-kais-michel.kais9555.chatgpt.site

## Le projet

Site statique, en français, construit avec Svelte 5, SvelteKit, Tailwind CSS v4, GSAP et Lenis. L’animation d’entrée, la révélation du manifeste mot par mot, les cartes qui se superposent au défilement et la navigation mobile reprennent la structure et les interactions du projet AURA, adaptée à cette campagne.

Le projet est dérivé du modèle [AURA — Svelte GSAP Template](https://github.com/YusufCeng1z/svelte-gsap-template) par YusufCeng1z, publié sous licence MIT. Voir [LICENSE](./LICENSE).

## Développement

    npm ci
    npm run dev
    npm run build
    npm run preview

Le rendu statique est écrit dans le dossier dist. Les animations respectent le réglage système de réduction des animations.
