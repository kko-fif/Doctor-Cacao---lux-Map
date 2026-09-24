# Démographie médicale au Luxembourg — version analytique

Cette version remplace l'affichage centré sur les établissements par une analyse centrée sur le **médecin**.

## Données
- `data/medecins.json` : 613 médecins issus de la collecte Doctena 2026, structurés par médecin, spécialité et lieux d'exercice.
- `data/population-rnpp-2026.json` : population communale RNPP au 01/07/2026.
- `data/population-statec-2021-2026.json` : évolution 2021→2026 issue de STATEC/LUSTAT DF_X021, via Data.public.lu.
- `data/master.json` : conservé uniquement comme archive/traçabilité de la version précédente ; la page ne l'utilise plus.

## Principes
- Un médecin n'est compté qu'une fois par commune, même s'il possède plusieurs adresses dans cette commune.
- Un médecin exerçant dans plusieurs communes peut être compté dans chacune pour l'analyse locale, mais reste unique au niveau national.
- La carte affiche un point par médecin à sa première position géocodée validée. Un très léger décalage visuel évite la superposition de médecins partageant la même adresse.
- Les établissements ne sont plus affichés.
- Les tranches d'âge prévues sont : `<30`, `30–34`, `35–39`, `40–44`, `45–49`, `50–54`, `55–59`, `60–64`, `65+`, `Inconnu`.
- Aucun âge individuel n'est estimé. Tant qu'une source fiable n'est pas disponible, la valeur reste `Inconnu`.

## Indicateurs disponibles
- médecins identifiés ;
- médecins / 10 000 habitants ;
- habitants / médecin ;
- évolution de population 2021–2026 ;
- filtre par spécialité ;
- filtre par tranche d'âge ;
- comparaison indicative avec le benchmark ObSanté 2023 (2 663 médecins praticiens toutes spécialités).

## Sources publiques
- RNPP / CTIE : population communale au 01/07/2026.
- STATEC / LUSTAT `DF_X021` : population par canton et commune, série annuelle.
- Observatoire national de la santé : factsheets sur les professionnels de santé.

La collecte Doctena n'est pas exhaustive et ne doit pas être interprétée comme un registre national officiel.


## Indicateurs d’âge

Les catégories autorisées sont strictement : `<30`, `30–34`, `35–39`, `40–44`, `45–49`, `50–54`, `55–59`, `60–64`, `65+`, `Inconnu`.

Trois indicateurs sont prêts dans l’interface et dans les calculs communaux :
- **55+** = `55–59` + `60–64` + `65+`
- **60+** = `60–64` + `65+`
- **65+** = `65+`

Le dénominateur est toujours le nombre de médecins dont la tranche d’âge est documentée. Les valeurs `Inconnu` sont exclues du dénominateur afin de ne pas sous-estimer artificiellement les proportions. Aucune tranche d’âge n’est imputée ou estimée sans source fiable.


## Enrichissement nominatif 23/09/2026

La base a été enrichie avec un premier passage ciblé sur la médecine générale et la pédiatrie à partir du registre ordinal public du Collège médical, complété par la liste nationale Doctena actuelle. Les profils explicitement marqués « Ne pratique plus » ont été exclus. Les ajouts conservent une source nominative dans `source_professionnelle` et `source_type`.

Après ce passage :
- médecine générale : 278 médecins identifiés ;
- pédiatrie : 50 médecins identifiés ;
- total base : 680 médecins.

Ces effectifs restent inférieurs aux praticiens officiels ObSanté (794 MG et 156 pédiatres en 2023), car le registre ordinal et les annuaires publics n'ont pas encore été réconciliés exhaustivement lettre par lettre et les définitions d'activité ne sont pas identiques.


## Référentiel médecine générale 2023

ObSanté : 1 277 inscrits au RDPS, 1 074 autorisés à exercer, 842 professionnellement actifs, 794 praticiens et 101 en pension/retraite parmi les inactifs. Le fichier `data/referentiel-mg-officiel-2023.json` conserve ces définitions. Les statuts individuels du registre public sont conservés séparément lorsqu'ils sont explicitement documentés (ex. `Remplaçant`, `Ne pratique plus`).

## Audit médecine générale — Collège médical (23/09/2026)

Un audit nominatif du registre ordinal public a été intégré pour les lettres **A à I**.

- 413 lignes contenant « Médecine générale » ont été relevées.
- 329 profils disposent d'une adresse au registre et sont classés « Actif » dans cet audit technique.
- 48 sont explicitement marqués « Ne pratique plus ».
- 5 sont explicitement marqués « Remplaçant ».
- 31 restent « À vérifier » (notamment adresse/statut insuffisant dans le registre public).
- 138 lignes ont été rapprochées de fiches déjà présentes dans la V4.
- 275 nouvelles fiches ont été ajoutées.
- Lorsque le Collège ne donnait pas d'adresse mais qu'une adresse Doctena existait déjà dans la V4, cette adresse a été conservée comme source distincte.

Le fichier `data/college-medical-mg-a-i.csv` contient la piste d'audit ligne par ligne. Cette version a désormais reçu une **extension I-Z** du registre ordinal. L'audit I-Z contient 851 entrées médicales : 694 issues d'un passage toutes spécialités (I/J/K/L/N/O/Q/U/V/X/Y/Z) et 157 issues d'un passage prioritaire médecine générale + pédiatrie (M/P/R/S/T/W). Ces six dernières lettres ne constituent donc pas encore un export exhaustif de toutes les spécialités du Collège médical.

Pour le benchmark national, la référence officielle ObSanté 2023 est distincte : 1 277 MG inscrits au RDPS, 1 074 autorisés à exercer, 842 professionnellement actifs et 794 praticiens. Ces catégories administratives ne sont pas équivalentes au simple statut visible dans l'annuaire du Collège médical.


## Extension Collège médical I-Z — 23/09/2026

- 851 entrées médicales auditées.
- 140 rapprochées d'une fiche déjà présente dans la V4.
- 711 nouvelles fiches ajoutées.
- Statuts relevés dans l'audit I-Z : 703 « Actif apparent », 94 « Ne pratique plus », 48 « À vérifier », 6 « Remplaçant ».
- Après normalisation des libellés, la base contient 846 fiches comportant « Médecine générale », dont 706 incluses dans l'indicateur actif technique.
- Elle contient 93 fiches « Pédiatrie », dont 84 incluses dans l'indicateur actif technique.
- Le terme « actif technique » signifie ici : non exclu par un statut négatif/à vérifier dans la base ; il ne remplace pas la définition administrative ObSanté/CCSS/CNS de « professionnellement actif ».

Fichiers d'audit : `data/college-medical-medecins-i-z.csv` et `data/audit-college-medecins-i-z.json`.

### Mise à jour du benchmark MG
La référence la plus récente intégrée est la factsheet ObSanté 2025 (données 2023) : 1 254 inscrits, 1 064 autorisés, 829 professionnellement actifs et 779 praticiens. Elle remplace dans l’interface les chiffres plus anciens du rapport méthodologique 2024.


## Audit A→H toutes spécialités (mise à jour 23/09/2026)

- A, B, C et E : audit nominatif du registre du Collège médical, toutes spécialités médicales.
- D, F, G et H : couverture toutes spécialités consolidée avec les médecins déjà présents dans la V4 ; chaque ligne conserve sa provenance.
- Dentistes, pharmaciens et psychothérapeutes non médecins sont exclus du référentiel médical.
- Les statuts `Ne pratique plus`, `Remplaçant` et `À vérifier` sont conservés mais exclus par défaut des indicateurs actifs.
- Fichier de contrôle : `data/medecins-a-h-toutes-specialites-consolide.csv`.
- Fichier fusionné A→Z : `data/college-medical-medecins-a-z.csv`.

### Portée à retenir
Le fichier `data/medecins-a-h-toutes-specialites-consolide.csv` remplace l'ancien bloc A→H limité à la médecine générale. Il couvre désormais toutes les spécialités présentes dans le référentiel consolidé. Pour A/B/C/E, les lignes ont été vérifiées directement dans le registre du Collège médical ; D/F/G/H sont complétées à partir de la V4 consolidée, avec la provenance conservée dans le CSV.


## Démographie d’âge officielle

Le filtre individuel par âge a été supprimé, car les dates de naissance/tranches d’âge ne sont pas disponibles nominativement de façon suffisamment fiable. La page affiche à la place une structure d’âge agrégée officielle ObSanté par spécialité lorsqu’une factsheet est intégrée. La V4 inclut actuellement Médecine générale et Pédiatrie (données 2023).

## Couche pression future 60+

La V4 comprend un classement des communes par hausse estimée du ratio habitants/médecin dans un scénario descriptif où la part nationale officielle des praticiens de 60 ans et plus sort de l'offre sans remplacement. Cette couche est disponible uniquement pour les spécialités disposant d'une structure d'âge officielle intégrée. Elle ne constitue pas une prévision individuelle de départ en retraite.
