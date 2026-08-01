# Plugin\_chaleur

# Chaleur MEL (readme du plugin)

Petit plugin QGIS que j'ai fait pour chercher une adresse dans la MEL et voir les infos sur la chaleur autour de la parcelle en vue peut être d'aider les agents immobilier à informer leur client.

Tapez une adresse, et le plugin affiche une fiche synthétique : à quel point ce secteur surchauffe, pourquoi, et quels points de vigilance en retenir. Il a été pensé notamment pour aider les professionnels de l'immobilier à donner à leurs clients une information claire sur le confort d'été d'un bien et de son quartier.

> \*\*Version actuelle : MVP (première version fonctionnelle). Donnée satellite de référence : nuit du 14 juillet 2026.\*\*

\---

## Ce que le plugin affiche

Pour l'adresse ou la parcelle sélectionnée, la fiche présente :

### Îlot de chaleur

* **Chaleur de surface** : l'écart de température des surfaces (sol, toitures) par rapport à la moyenne de la métropole, mesuré par satellite. Un chiffre positif = plus chaud que la moyenne, négatif = plus frais.
* **Chaleur de l'air la nuit** : quand la donnée est disponible, l'intensité de l'îlot de chaleur ressentie dans l'air la nuit (modélisation Météo-France). Pas dispo en MVP mais intégré dans le calcul.
* **Type de quartier (LCZ)** : une classification standardisée du tissu urbain (habitat dense, pavillonnaire, zone végétalisée…), qui explique en grande partie la sensibilité à la chaleur.

### Structure de l'îlot de chaleur

Les facteurs qui expliquent pourquoi il fait chaud ici :

* part de surfaces imperméables (bitume, béton) ;
* part de végétation (qui rafraîchit) ;
* emprise bâtie au sol ;
* hauteur moyenne des bâtiments.

### Sols argileux (RGA)

Le niveau d'exposition au retrait-gonflement des argiles — un risque distinct mais lié, car les sécheresses accompagnant les fortes chaleurs peuvent faire jouer les sols argileux et fissurer les bâtiments.

### Rue la plus proche

L'écart de chaleur du tronçon de voie le plus proche.

### Repères pour l'estimation

Points factuels pour aider un professionnel à situer le bien (confort d'été à examiner, atout de fraîcheur, diagnostic géotechnique éventuel…). Ce ne sont pas des estimations de valeur ni des diagnostics réglementaires.

\---

## Comment l'utiliser

* **Installation** : dans QGIS, menu *Extensions → Installer/Gérer les extensions → Installer depuis un ZIP*, puis sélectionnez le fichier du plugin. Un bouton « Chaleur MEL » apparaît dans la barre d'outils.
* **Premier lancement** : le plugin télécharge automatiquement le pack de données de la métropole (quelques centaines de Mo). Ensuite, tout fonctionne en local ; seule la recherche d'adresse nécessite une connexion internet.
* **Consultation** : cliquez sur le bouton, puis soit tapez une adresse dans le panneau de droite et cliquez un résultat, soit cliquez directement une parcelle sur la carte. La fiche s'affiche dans le panneau, et la parcelle est surlignée en orange.

> \*\*NB : en MVP le pack est à télécharger\*\*

\---

## D'où viennent les données

Toutes les données sont publiques et en libre accès, issues de sources officielles :

|Information|Source|
|-|-|
|Chaleur de surface (satellite)|ECOSTRESS (NASA)|
|Type de quartier (LCZ)|Cerema|
|Chaleur de l'air la nuit|Météo-France (projet MApUCE)|
|Végétation (NDVI)|Sentinel-2, programme Copernicus|
|Imperméabilisation|Copernicus (High Resolution Layer)|
|Parcelles, bâtiments, voies|IGN (Parcellaire Express, BD TOPO)|
|Sols argileux (RGA)|BRGM|

Le pack de données est versionné : chaque mise à jour (nouvelle scène satellite, données rafraîchies) donne lieu à une nouvelle version (V2, V3…) que le plugin peut proposer de télécharger.

\---

## Limites et précautions de lecture

Ce plugin est un outil d'information et de sensibilisation, pas un instrument de mesure ni un document opposable. Quelques points importants à comprendre :

* **Température de surface ≠ température de l'air.** Le satellite mesure la chaleur de la peau des surfaces (sol, toits), qui peut être bien plus élevée que l'air ambiant en journée. C'est un excellent révélateur des zones qui surchauffent, mais ce n'est pas le thermomètre sous abri de la météo.
* **Une photo à un instant donné.** La chaleur de surface correspond au passage du satellite à une date et une heure précises (ici, une nuit d'été 2026). Elle reflète les conditions de ce moment-là, pas une moyenne sur l'année.
* **Précision limitée sur les très petites parcelles.** Chaque pixel satellite couvre environ 70 mètres. Pour une parcelle plus petite que cette taille (fréquent en centre-ville), la valeur affichée est celle du voisinage immédiat — le plugin le signale alors clairement dans la fiche.
* **Données de zonage, pas d'expertise du bien.** La classification LCZ et l'aléa argiles sont établis à l'échelle du quartier ou du secteur (cartes géologiques au 1/50 000 pour le RGA). Ils décrivent un contexte, pas l'état exact d'une parcelle donnée.
* **Ce n'est pas une estimation immobilière.** Les « repères pour l'estimation » sont des points d'attention factuels destinés à nourrir une discussion. Ils ne constituent ni une estimation de valeur, ni un diagnostic réglementaire (DPE, état des risques, étude géotechnique…). Pour un document opposable sur les risques, référez-vous au dispositif officiel (rapport ERRIAL sur Géorisques, diagnostics réglementaires).

\---

## Aspects techniques (pour les curieux)

* Développé en Python pour QGIS 3.28 et suivants.
* La chaleur de surface est calculée en approche « intra-urbaine » : l'écart de chaque endroit par rapport à la moyenne de toute la métropole, à un instant donné.
* La recherche d'adresse s'appuie sur la Base Adresse Nationale via la Géoplateforme de l'IGN.

\---

## Licence

Plugin open source (GPL v2 ou ultérieure). Données sous licences ouvertes de leurs producteurs respectifs (voir tableau ci-dessus). Pour signaler un bug ou proposer une amélioration, utilisez le suivi de bugs indiqué dans les métadonnées du plugin.

\---

## Attendus V1

* Hébergement du pack de données pour téléchargement direct
* Affichage si possible des évolutions de température sur un an
* Possibilité de cliquer directement sur la carte pour voir afficher les données
* Réussir à trouver des données chaleur de nuit exploitable (attente retour météo France)

