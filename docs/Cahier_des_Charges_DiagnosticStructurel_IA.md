# CAHIER DES CHARGES

## Logiciel de Diagnostic Structurel intégrant l'Intelligence Artificielle

**Version 1.1 | Mai 2026**

*Document confidentiel — Usage interne*

---

## Sommaire

- [1. Introduction et contexte](#1-introduction-et-contexte)
  - [1.1 Contexte du projet](#11-contexte-du-projet)
  - [1.2 Objectifs généraux](#12-objectifs-généraux)
  - [1.3 Périmètre du projet](#13-périmètre-du-projet)
- [2. Description fonctionnelle](#2-description-fonctionnelle)
  - [2.1 Module 1 — Relevés terrain](#21-module-1--relevés-terrain)
  - [2.2 Module 2 — Analyse IA des désordres](#22-module-2--analyse-ia-des-désordres)
  - [2.3 Module 3 — Gestion de la donnée et apprentissage](#23-module-3--gestion-de-la-donnée-et-apprentissage)
- [3. Expérience utilisateur (UX)](#3-expérience-utilisateur-ux)
  - [3.1 Principes de design](#31-principes-de-design)
  - [3.2 Profils utilisateurs](#32-profils-utilisateurs)
  - [3.3 Ergonomie mobile](#33-ergonomie-mobile)

---

## 1. Introduction et contexte

### 1.1 Contexte du projet

Le diagnostic structurel constitue une étape fondamentale dans l'évaluation de la sécurité et de la durabilité des ouvrages bâtis. Aujourd'hui, cette activité repose largement sur l'expertise humaine, des relevés manuels sur site et une interprétation empirique des désordres observés. Ces pratiques, bien que robustes, restent chronophages, sujettes à variabilité interopérateur et difficilement capitalisables à grande échelle.

Dans ce contexte, le présent projet vise à développer un outil numérique mobile intégrant l'intelligence artificielle, conçu spécifiquement pour les experts réalisant des diagnostics structurels sur site. L'outil a pour ambition de :

- Faciliter et standardiser les relevés réalisés lors des missions de diagnostic
- Aider à l'identification automatique de la nature des désordres à partir de photos prises sur site
- Capitaliser les données collectées pour améliorer progressivement les performances du modèle IA

### 1.2 Objectifs généraux

L'outil poursuit trois objectifs stratégiques majeurs :

| # | Objectif | Bénéfice attendu |
|---|----------|------------------|
| 1 | Simplification des relevés terrain | Gain de temps, réduction des erreurs de saisie |
| 2 | Identification IA des désordres | Aide à la décision, homogénéisation des diagnostics |
| 3 | Apprentissage continu par la data | Amélioration progressive de la précision du modèle |

### 1.3 Périmètre du projet

Le périmètre du présent cahier des charges couvre :

- Le module de relevé terrain (saisie guidée, formulaires intelligents, export PDF)
- Le module d'analyse d'images (détection et classification des désordres)
- Le module de gestion et d'apprentissage de la donnée
- L'interface utilisateur mobile destinée aux experts terrain
- Le back-office de supervision et de gestion des missions

**Sont exclus du présent périmètre :** la gestion comptable des missions, la rédaction automatisée de rapports d'expertise juridique (version 1.0).

---

## 2. Description fonctionnelle

### 2.1 Module 1 — Relevés terrain

#### 2.1.1 Création et gestion des missions

- Création d'une mission de diagnostic depuis l'application mobile ou le back-office
- Saisie des informations de l'ouvrage : adresse, type de structure, date de construction estimée, usage (résidentiel, ERP, industriel, infrastructure…)
- Association d'un ou plusieurs experts à la mission
- Consultation de l'historique des missions passées sur un même ouvrage

#### 2.1.2 Formulaires de relevés guidés

L'outil propose des formulaires dynamiques adaptés au type d'ouvrage diagnostiqué. Chaque formulaire guide l'expert pas à pas selon une méthodologie standardisée.

- Relevés par zones / éléments structurels : fondations, planchers, poteaux, poutres, voiles, façades, toiture
- Saisie rapide par cases à cocher, curseurs d'intensité (1 à 5), menus déroulants et champs texte libres
- Intégration de listes de contrôle (checklists) configurables selon la mission
- Possibilité de fonctionnement hors ligne avec synchronisation différée
- Géolocalisation automatique des relevés

#### 2.1.3 Prise de photos et annotations

- Accès direct à l'appareil photo depuis le formulaire de relevé
- Association automatique de chaque photo à un élément structurel et à une zone
- Outil d'annotation manuelle sur image (flèches, cercles, texte)
- Horodatage et géolocalisation embarqués dans les métadonnées de la photo
- Possibilité d'importer des photos existantes depuis la galerie

#### 2.1.4 Export PDF du relevé : synthèse, coupes et schémas

À l'issue (ou en cours) d'une mission, l'expert doit pouvoir générer un document PDF de synthèse reprenant l'ensemble des données saisies sur site, enrichi de représentations graphiques (coupes et schémas) facilitant la compréhension et la localisation des désordres.

- **Génération automatique d'un rapport PDF :** compilation de toutes les données du relevé (informations ouvrage, checklists, valeurs saisies, observations, photos annotées) dans un document structuré et mis en page professionnellement
- **Insertion de coupes techniques :** bibliothèque de coupes types préconfigurées (coupe de mur, plancher, fondation, toiture, poteau-poutre…) sur lesquelles l'expert peut positionner les désordres relevés directement depuis l'application
- **Schémas annotés :** outil de croquis simplifié (dessin à main levée, formes géométriques, légendes, cotations) permettant de réaliser un schéma de principe de l'ouvrage ou de la zone diagnostiquée
- **Localisation des désordres sur plan/coupe :** report automatique des points de relevé (avec numérotation) sur la coupe ou le schéma sélectionné, avec renvoi vers la fiche de désordre correspondante
- **Mise en page du PDF :** page de garde personnalisable (logo du cabinet, intitulé de mission, date, intervenant), sommaire automatique, pagination, synthèse par zone et annexes photographiques
- **Export et partage :** génération à la demande depuis le terrain (même hors ligne, finalisation à la synchronisation), export local, envoi par e-mail ou partage via lien sécurisé
- **Modèles de rapport configurables :** choix entre plusieurs trames de rapport (rapport de visite, rapport de diagnostic complet, fiche de désordre unitaire) selon le besoin de la mission

### 2.2 Module 2 — Analyse IA des désordres

#### 2.2.1 Identification automatique par analyse d'image

Le module d'analyse d'image constitue le cœur technologique de l'outil. À partir d'une ou plusieurs photos soumises par l'expert, le système produit une analyse automatisée des désordres visibles.

Les catégories de désordres identifiables (liste évolutive) :

| Catégorie | Exemples de désordres |
|-----------|------------------------|
| Fissuration | Fissures de retrait, fissures structurelles, lézardes, microfissures |
| Corrosion / carbonatation | Épaufrures, armatures apparentes, taches de rouille, bullage |
| Humidité / infiltrations | Remontées capillaires, moisissures, efflorescences, salpêtres |
| Déformations | Flambement, désaffleurements, tassements différentiels, dévers |
| Désordres maçonnerie | Déliaisonnements, décollements, joints dégradés, faïençage |
| Dégradations diverses | Éclatements, défauts d'enrobage, cavités, impacts |

#### 2.2.2 Caractérisation des désordres

Pour chaque désordre détecté, l'IA fournit :

- La nature du désordre (catégorie + sous-catégorie)
- La localisation sur l'image (bounding box ou masque de segmentation)
- Un indice de sévérité (léger / modéré / sévère / critique)
- Un score de confiance de la prédiction (en %)
- Des hypothèses d'origine probable (causes potentielles)
- Des recommandations préliminaires de surveillance ou d'intervention

#### 2.2.3 Validation et correction par l'expert

Les résultats produits par l'IA sont toujours soumis à la validation de l'expert terrain. Ce principe garantit la fiabilité des données et alimente en retour l'apprentissage du modèle.

- Affichage des résultats IA avec possibilité de valider, modifier ou rejeter chaque détection
- Ajout manuel de désordres non détectés par le modèle
- Toute correction validée est enregistrée comme donnée d'entraînement

### 2.3 Module 3 — Gestion de la donnée et apprentissage

#### 2.3.1 Base de données des désordres

- Stockage structuré de l'ensemble des images collectées et des annotations associées
- Classement par type de structure, nature de désordre, sévérité, localisation géographique
- Exportation des datasets pour entraînement du modèle IA
- Traçabilité complète : expert, date, mission, ouvrage

#### 2.3.2 Cycle d'apprentissage continu

L'amélioration du modèle suit un cycle itératif :

| Étape | Description |
|-------|-------------|
| 1 — Collecte | Les experts collectent des images et valident/corrigent les détections IA |
| 2 — Annotation | Les données validées sont intégrées au dataset d'entraînement annoté |
| 3 — Entraînement | Le modèle est réentraîné périodiquement par l'équipe technique |
| 4 — Validation | Les nouvelles performances sont évaluées sur un jeu de test dédié |
| 5 — Déploiement | Le modèle amélioré est déployé dans l'application après validation |

#### 2.3.3 Tableau de bord des performances IA

- Suivi des métriques du modèle : précision, rappel, F1-score par catégorie de désordre
- Historique des versions du modèle et comparatif des performances
- Alertes en cas de dérive de performance détectée

---

## 3. Expérience utilisateur (UX)

### 3.1 Principes de design

L'application est destinée à un usage terrain dans des conditions parfois difficiles (luminosité variable, mains occupées, port de gants, connexion instable). Les principes UX suivants sont non négociables :

- **Simplicité d'usage :** toute fonctionnalité clé doit être accessible en 3 clics maximum
- **Robustesse offline :** aucune perte de données en cas de coupure réseau
- **Lisibilité :** contrastes élevés, typographie grande (minimum 14pt), icônes explicites
- **Rapidité :** fluidité de navigation, pas d'attente lors des saisies locales
- **Guidage contextuel :** aide en ligne, tooltips et messages d'erreur clairs

### 3.2 Profils utilisateurs

| Profil | Rôle | Fonctions principales |
|--------|------|------------------------|
| Expert terrain | Réalise les diagnostics sur site | Relevés, photos, validation IA, schémas/coupes, export PDF, consultation mission |
| Chef de projet | Supervise les missions | Création missions, affectation experts, consultation rapports PDF |
| Administrateur | Gère l'outil et les données | Gestion utilisateurs, paramétrage, suivi modèle IA, gestion des modèles de rapport |

### 3.3 Ergonomie mobile

- Interface à une main possible pour les actions courantes
- Mode sombre disponible pour les environnements peu éclairés
- Retour haptique sur les actions importantes
- Compatibilité avec les accessoires Bluetooth (stylets, claviers externes — utiles pour le croquis de schémas)