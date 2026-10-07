# RP Nation — feuille de route

Version modifiée d'Azgaar's Fantasy Map Generator pour un RP nation joué sur Discord.
Ce fichier est la mémoire du projet : la demande initiale, les décisions prises, et l'avancement.

## La demande initiale (à ne jamais perdre de vue)

1. **Le MJ garde la main sur tout** : sa version contient tous les outils d'Azgaar, plus les nôtres.
2. **Les joueurs reçoivent chaque semaine un fichier** qu'ils ne peuvent ni modifier, ni ouvrir sur l'Azgaar officiel,
   consulté dans une interface d'où **toutes les options de modification ont été retirées**.
3. **Brouillard de guerre individuel** sur une carte commune, selon la progression de chacun.
4. **Économie propre au MJ** : garder les ressources d'Azgaar, leur répartition par biome et les crafts, pouvoir créer
   ses propres crafts, et rendre l'économie **vivante** : biens volés, échangés, transformés, vendus, monnaie qui circule
   sur de vrais marchés. Azgaar ne calcule qu'une photo figée ; on veut un film, tour après tour.
5. **Recrutement des armées détaillé**, en gardant le simulateur de bataille d'Azgaar.
6. **Féodalité multicouche** : royaumes, provinces, plus des couches de cantons et de fiefs, et des chaînes
   d'allégeance de profondeur libre.
7. **Fiches cliquables** pour les joueurs : cliquer une ville connue montre ce que le joueur en sait.
8. **Renseignement et espionnage** quand la négociation ou l'alliance ne suffit pas.

## Principes de conception retenus

- **Le temps du MJ est la ressource rare** : tout ce qui est chiffré est calculé par le programme.
- **Un tour par semaine** (≈ un mois en jeu), 3 ordres majeurs par joueur et par tour.
- **Ce que le joueur ne doit pas voir n'est pas dans son fichier.** Le brouillard n'est pas un voile, c'est une absence
  de données. C'est aussi ce qui rend le fichier joueur inexploitable sur l'Azgaar officiel.
- **Tout vit dans la carte**, pas dans un tableur : panneau « Données de jeu » côté MJ, fiches côté joueurs.
- **Nos ajouts sont rangés à part** (`src/**/feudal*`, `src/**/rp-*`, `docs/rp/`) pour pouvoir récupérer
  les mises à jour d'Azgaar sans tout casser.

## Féodalité

- **Territoire et allégeance sont séparés.** La carte administrative (fiefs, cantons, provinces) est fixe et sert
  à l'économie et aux revendications. La carte féodale (qui a prêté hommage à qui) est dynamique.
- **Chaque maison a un seul suzerain direct.** La chaîne complète se déduit en remontant.
- **Les titres sont conférés, pas calculés** : aucun seuil de fiefs. Seule règle : on ne confère qu'un titre inférieur
  au sien ; le titre de roi exige couronne et légitimité. Le titre donne le prestige, les **moyens** donnent les capacités.
- Titres : roi > gouverneur (charge révocable) > seigneur suzerain > seigneur banneret > seigneur de bannière
  (répétable) > seigneur châtelain. Profondeur libre.
- **« Le vassal de mon vassal n'est pas mon vassal »** : les ordres, le tribut et l'ost passent par la chaîne.
  Un suzerain qui convoque l'ost mobilise toute sa mouvance, maillon par maillon. Une défection emporte la branche.
- **Le taux de tribut est une clause de l'hommage**, négociée entre vassal et seigneur.
- Obligations : aide, ost, conseil / protection, justice. Rupture légitime si le seigneur faillit ; sinon félonie.
  Appel possible au niveau supérieur.
- **Affichage** : teinte = royaume puis branche ; épaisseur de frontière = niveau où les chaînes de deux fiefs voisins
  se séparent ; zoom progressif, mode focus sur une maison, panneau arbre.
- Les États d'Azgaar suivent le sommet des chaînes ; les provinces deviennent administratives.

## Joueurs

- **Arrivée** : le monde est plein de maisons PNJ ; un nouveau joueur reprend une maison de rang châtelain ou seigneur,
  4 tours de protection, stocks alignés sur la médiane de son rang.
- **Absence annoncée** : régence de 4 tours max par un intendant PNJ (ordres défensifs).
- **Disparition** : 1 tour → intendant ; 2 → avertissement ; 3 → régence, vassaux libres ; 6 → déshérence.
- **Réattribution** : testament > suzerain joueur > nouveau joueur > reste PNJ et revendicable. Jamais le jugement seul du MJ.
- **PNJ** : une ligne de données (loyauté 0–10, puissance calculée, un trait, un état). Le MJ n'intervient que sur événement.

## Conquête

- États d'un territoire : occupé → contrôlé (3 tours de garnison) → possédé (revendication, traité ou coût en Prestige).
- Revendications : héritage, mariage, titre ancien, décision du suzerain, fabrication en Prestige.
- Durée de siège selon le fort (1 à 5 tours), réduite par les engins.

## Économie vivante

- On garde les biens, la répartition par biome, les recettes, les marchés et le commerce d'Azgaar.
- Ajouts : bouton « Avancer d'un tour » qui part de l'état précédent ; stocks persistants ; biens périssables ;
  prix avec inertie (±20 %/tour) ; trésors réels et séparés ; monnaie qui circule ; droit de frappe et inflation.
- Biens **localisés** (entrepôt, convoi, armée, navire) ; marchandises en vrac + objets notables uniques.
- Actions : acheter/vendre (le volume fait bouger le prix), échanger, transformer, transporter, voler, taxer le passage.
- Éditeur de recettes pour les crafts du MJ. **Le recrutement consomme des biens réels.**
- Chocs : récoltes variables, catastrophes (zones d'Azgaar), blocus.
- Les prix étrangers relèvent du renseignement.
- Avant ouverture : simuler 50 à 100 tours à vide pour vérifier la stabilité.
- Question ouverte : détail bien par bien, ou catégories simplifiées avec détail au clic ?

## Armées

- Réservoir d'hommes par fief (~10 % de la population, +5 %/tour).
- Ost féodal (gratuit, 4 tours de service) et troupes permanentes (entretien).
- Unités avec prérequis (bâtiments, biens, moyens) ; expérience Recrue → Aguerrie → Vétéran → Élite.
- Une unité régionale par type de culture d'Azgaar. Batailles dans le simulateur d'Azgaar.

## Renseignement

- Niveaux par joueur et par lieu : 0 inconnu, 1 aperçu, 2 estimé (fourchettes), 3 précis, 4 infiltré.
- Chaque information est **datée** ; une info ancienne s'affiche grisée.
- Sources : ses terres, vue directe, éclaireurs, pacte de renseignement, déclarations (peuvent mentir), espionnage.
- Réseaux d'espions (nombre selon le rang), missions (infiltration, sabotage, subornation, désinformation, complot),
  contre-espionnage automatique par score de Sécurité.

## Étapes de réalisation

| # | Étape | État |
|---|-------|------|
| 0 | Copie GitHub, mise en ligne automatique, nettoyage des automatismes d'Azgaar | fait |
| 1 | Données féodales : fiefs, cantons, maisons, hommages, titres, enregistrées dans le .map | à faire |
| 2 | Éditeur MJ des fiefs et des maisons (peinture des fiefs, création des hommages) | à faire |
| 3 | Carte féodale : frontières imbriquées, mode focus, panneau arbre | à faire |
| 4 | Panneau « Données de jeu » MJ sur les bourgs et les fiefs | à faire |
| 5 | Export joueur : découpe par joueur, brouillard, format illisible par l'Azgaar officiel | à faire |
| 6 | Visualiseur joueur en lecture seule, fiches cliquables | à faire |
| 7 | Renseignement et espionnage | à faire |
| 8 | Économie vivante : tour par tour, stocks, prix, trésors, éditeur de recettes | à faire |
| 9 | Armées : réservoirs, ost, recrutement consommant des biens | à faire |
| 10 | Résolution du tour complète ; simulation à vide pour l'équilibrage | à faire |
| 11 | Bot Discord (optionnel) | plus tard |
