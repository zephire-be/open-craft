# Cahier des charges — Système de verrouillage wikivan

**Version :** 1.0
**Date :** 2026-06-22
**Auteur :** Alexei (facilité par session brainstorming structurée)
**Statut :** Spécification de référence — base pour le prototypage V1

---

## 1. Contexte & objectifs

### 1.1 Projet wikivan

wikivan est un système modulaire d'aménagement de van. Les modules (cuisine, lit, rangements, frigo, eau, etc.) se montent, démontent et se déplacent dans le van comme des briques Lego sur une grille 2D régulière.

### 1.2 Rôle du verrou

Le verrou est le **composant d'interface universel** entre les modules wikivan et l'interface (plancher / parois du van, ou autres modules). Sa qualité conditionne :

- La **sécurité** des occupants en route (modules ne deviennent pas projectiles)
- L'**ergonomie quotidienne** (rapidité de pose/dépose)
- La **modularité** du système (capacité à reconfigurer le van)
- La **durabilité** du van (verrou doit survivre à la durée de vie du véhicule)

### 1.3 Objectifs de ce document

Définir les **exigences que tout système de verrou wikivan doit satisfaire**, indépendamment de la solution technique retenue. Ce document est la **référence d'évaluation** pour les concepts de design (WikiCam, WikiClip, ou futurs).

### 1.4 Niveaux d'exigence

Convention RFC 2119 :
- **MUST** / **DOIT** : exigence non-négociable, abandon du concept si non respectée
- **SHOULD** / **DEVRAIT** : exigence forte, dérogation possible avec justification documentée
- **MAY** / **PEUT** : exigence souhaitable, optimisation produit

---

## 2. Glossaire

| Terme | Définition |
|---|---|
| **Module** | Élément d'aménagement amovible (cuisine, lit, tiroir, frigo, etc.) |
| **Interface** | Surface portante du van (plancher, parois, plafond) ou face d'un autre module |
| **Grille** | Quadrillage d'ancrages réguliers au pas U sur l'interface |
| **U** | Unité de pas de la grille = **100 mm** (1U = 10 cm) |
| **Verrou** | Mécanisme assurant la fixation d'un module à un point d'ancrage |
| **Point d'ancrage** | Nœud de la grille où un verrou peut s'attacher |
| **Bus adapter** | Module spécialisé qui sert d'interface entre wikivan et un écosystème externe (Systainer, L-Boxx, etc.) |
| **WikiCam** | Concept de verrou candidat #1 (rotation + serrage auto-énergisé) |
| **WikiClip** | Concept de verrou candidat #2 (push-pull à détente calibrée) |
| **WikiStrap** | Sangle secondaire obligatoire pour la sécurité crash (associée à WikiClip) |

---

## 3. Périmètre

### 3.1 Dans le périmètre

- Verrou module ↔ interface van (plancher, parois, plafond)
- Verrou module ↔ autre module (empilement, juxtaposition)
- Système de sangles secondaires si nécessaire à la sécurité crash
- Compatibilité avec écosystèmes externes via Bus adapters

### 3.2 Hors périmètre

- Design des modules eux-mêmes (uniquement leur **interface de verrou**)
- Design de l'aménagement intérieur du van (plancher, isolation, électricité)
- Verrous "antivol" (cadenas, traçabilité) — éventuellement option future
- Compatibilité avec ancrages existants du van (L-track, ISOFIX, etc.)

---

## 4. Décisions architecturales (ADR)

### ADR-01 : Architecture en grille Lego 2D

**Décision :** L'interface van et les faces de connexion des modules sont organisées en **grille régulière 2D** au pas U=100 mm. Les modules ont des dimensions quantifiées (xU × yU × zU) et se placent à n'importe quel nœud de la grille.

**Rationale :** Permet la modularité libre style Lego. Conditionne tout le système.

**Conséquence :** Tout concept de verrou doit s'intégrer dans cette grille. Les concepts à rails continus (type queue d'aronde Arca-Swiss) sont **exclus**.

### ADR-02 : Pas U = 100 mm

**Décision :** Pas de grille = **100 mm**.

**Rationale :**
- Décimal pur (1U=10cm), lecture mentale zéro-friction
- Compatible avec la taille du verrou (Ø ≤ 80 mm)
- Plateau van 1200×2400 mm = 12U × 24U = 288 nœuds, structurellement viable
- Tableau de 10 → calculs de design triviaux

**Alternatives rejetées :** 50 mm (chevauchement des verrous adjacents), 96 mm MFT (mauvaise lecture cm), 90 mm (perd décimal pour gagner sur Systainer marginalement).

### ADR-03 : Architecture "Bus + Adapters"

**Décision :** La compatibilité avec écosystèmes externes (Festool Systainer, Bosch L-Boxx, Milwaukee Packout, bouteilles de gaz, cassettes d'eau, etc.) se résout via des **modules adaptateurs spécialisés** ("Bus adapters") qui parlent wikivan d'un côté et l'écosystème externe de l'autre.

**Rationale :** Sépare les préoccupations. La grille reste pure. L'écosystème wikivan peut s'étendre à de nouveaux standards sans toucher l'interface principale.

**Variantes Systainer-Bus prévues :** Drawer (tiroir), Rack (rails ouverts), Foot (plaque-pied universelle).

### ADR-04 : Coexistence multi-verrous

**Décision :** Plusieurs technologies de verrou peuvent coexister sur la même grille, à condition de **partager le perçage de base** sur l'interface femelle. Un point d'ancrage peut être équipé en WikiCam, WikiClip, ou les deux.

**Rationale :** Permet de spécialiser le verrou selon le module (lourd/permanent vs léger/quotidien).

---

## 5. Exigences fonctionnelles (FR)

### FR-01 — Rétention sous charge nominale

Le verrou **DOIT** retenir un module à son point d'ancrage sous toutes les charges générées par l'usage normal du van :
- Charge statique du module (poids propre + contenu)
- Vibrations de roulage
- Freinage normal
- Ouverture/fermeture de tiroirs ou trappes du module
- Appui d'un occupant (forces ponctuelles ≤ 80 daN)

### FR-02 — Démontage / remontage utilisateur

Le verrou **DOIT** permettre à un utilisateur seul, sans formation spéciale, de poser et déposer un module en moins de 60 secondes.

Le verrou **DEVRAIT** permettre ces opérations sans outil dédié (clé Allen standard 6/8 mm acceptable).

### FR-03 — Orientation libre

Le verrou **DOIT** fonctionner identiquement pour les modules fixés au **sol**, aux **parois latérales**, et au **plafond**. La performance ne doit pas dépendre de la gravité.

### FR-04 — État identifiable

L'utilisateur **DOIT** pouvoir déterminer en moins de 3 secondes si un verrou est engagé ou non, par signal **visuel**, **tactile** ou **sonore**.

### FR-05 — Sécurité freinage d'urgence

Le verrou **DOIT** retenir un module en cas de freinage d'urgence (décélération équivalente à 1g) sans aucune dégradation.

### FR-06 — Sécurité crash / tonneau

Le système global (verrou + accessoires de sécurité éventuels) **DOIT** retenir un module de 40 kg en cas de tonneau (forces multi-axiales jusqu'à 20g).

**Note :** cette exigence peut être satisfaite par le verrou seul (WikiCam) ou par le verrou + dispositif secondaire (WikiClip + WikiStrap).

### FR-07 — Multi-directionnalité

Le verrou **DOIT** résister aux efforts dans toutes les directions cardinales (vertical haut/bas, latéral gauche/droite, avant/arrière). L'architecture **DEVRAIT** être isotrope (résistance équivalente toutes directions).

### FR-08 — Liberté de positionnement

Le verrou **DOIT** permettre l'ancrage d'un module à n'importe quel nœud compatible de la grille, sans contrainte d'ordre d'installation, et sans dépendre des modules voisins.

### FR-09 — Démontage au milieu d'un van plein

Le verrou **DOIT** permettre la dépose d'un module situé au milieu d'un van plein, sans avoir à déplacer les modules voisins.

### FR-10 — Trois états distincts (souhaitable)

Le verrou **DEVRAIT** offrir 3 états distincts :
1. **Désengagé** — module non maintenu
2. **Engagé** — module en place mais pas verrouillé
3. **Verrouillé** — module sécurisé pour la route

Le verrou **PEUT** offrir un 4ème état "cadenassable" (anti-vol).

### FR-11 — Réparabilité sur la route

Le verrou **DEVRAIT** être réparable sur la route par l'utilisateur avec un kit de spare parts pesant moins de 500 g.

Le verrou **DOIT** comporter une **pièce d'usure remplaçable** distincte de la pièce structurelle.

### FR-12 — Universalité interface

Un seul design de verrou **DOIT** couvrir les interfaces module↔van et module↔module. Pas de variantes selon la position.

---

## 6. Exigences non-fonctionnelles (NFR)

### NFR-01 — Force de rétention

| Direction | Force minimale par verrou |
|---|---|
| Arrachement vertical | **≥ 200 daN** (cible 40 kg × 5g facteur sécurité) |
| Cisaillement latéral | **≥ 150 daN** |
| Couple / arrachement perpendiculaire | **≥ 100 daN** |

Pour un module de plus de 40 kg, la conception prévoit **plusieurs verrous distribués** (typiquement 4 aux coins + N intermédiaires si nécessaire).

### NFR-02 — Durée de vie en cycles

Le verrou **DOIT** supporter au minimum :

| Profil d'usage | Cycles d'engagement/dégagement |
|---|---|
| Module "permanent" (lit, batterie, cuisine) | ≥ **500 cycles** sur 20 ans |
| Module "quotidien" (tiroir, trappe, accessoire) | ≥ **50 000 cycles** sur 20 ans |

Pièce d'usure remplaçable acceptée pour atteindre ces cibles.

### NFR-03 — Tolérance environnementale

Le verrou **DOIT** rester fonctionnel dans les conditions suivantes (force de rétention ≥ 80% de la valeur nominale) :

| Condition | Plage |
|---|---|
| Température | **-20°C à +60°C** |
| Humidité relative | **20% à 95%** (sans condensation visible) |
| Exposition UV | équivalent **5 ans en extérieur tempéré** |
| Poussière / sciure | accumulation jusqu'à **2 mm** dans le réceptacle |
| Vibrations longue durée | équivalent **100 000 km** de route mixte |

### NFR-04 — Vitesse d'usage

| Geste | Temps cible | Geste à 1 main ? |
|---|---|---|
| Engagement d'un module | **≤ 30 s** | Souhaitable |
| Dégagement d'un module | **≤ 30 s** | Souhaitable |
| Engagement + verrouillage complet | **≤ 60 s** | — |

### NFR-05 — Acoustique

Le verrou **DOIT** être silencieux en roulage. Pas de craquement, couinement ou vibration audible perceptible depuis l'habitacle.

### NFR-06 — Mode de défaillance

Le verrou **DEVRAIT** se dégrader **progressivement et visiblement** plutôt que brutalement. L'utilisateur doit pouvoir détecter l'usure avant la rupture.

Une rupture brutale **PEUT** être acceptée pour la pièce d'usure remplaçable, à condition que la pièce structurelle reste intacte.

---

## 7. Contraintes de fabrication

### CF-01 — CNC 3 axes uniquement

Tous les composants **DOIVENT** être fabricables sur une **CNC 3 axes** standard d'atelier maker (Shapeoko, X-Carve, Avid, OneFinity, ou équivalent).

Sont **interdits** :
- 4 axes, 5 axes
- Tour
- Soudure
- Forge / moulage / sintering
- Impression 3D **comme procédé principal** (acceptée comme procédé accessoire pour pièces secondaires)

Sont **acceptés** :
- Mèches dovetail 45° (3 axes possible)
- Plongée verticale + opérations 2D dans le plan XY

### CF-02 — Économie de matière

Le verrou **DOIT** être conçu pour minimiser les chutes :
- Pièces utilisables tirées d'une chute carrée de **≤ 100×100 mm** par exemplaire
- Imbrication possible sur plaque standard 1220×2440 mm
- Pas de matière en surplus "esthétique"

### CF-03 — Quincaillerie acceptée

Les composants externes acceptés sont :
- Vis bois et inserts filetés bois standard (M4, M5, M6, M8)
- Goupilles bois Ø6-Ø10 mm
- Inserts en polymère technique (POM, PETG, PA) usinables CNC plat
- Lames ressort acier 0,3-1 mm
- Sangles polyester avec quincaillerie Fastex / cliquet standard

**Refusé** : connecteurs propriétaires non-réapprovisionnables, électronique active.

### CF-04 — Temps CNC

Le temps d'usinage cumulé par verrou (mâle + femelle) **DEVRAIT** rester inférieur à **10 minutes**.

### CF-05 — Coût unitaire matière + quincaillerie

**Cible budgétaire :** ≤ **5 € par verrou** (production atelier, hors temps de main d'œuvre).

---

## 8. Critères d'acceptation (résumé)

Un concept de verrou est considéré **viable pour V1 prototype** si :

- ✅ Toutes les exigences **MUST/DOIT** sont satisfaites
- ✅ Au moins 80% des exigences **SHOULD/DEVRAIT** sont satisfaites
- ✅ Le coût matière est ≤ 5 €
- ✅ Le temps CNC est ≤ 10 min
- ✅ Le proto passe les **tests A, B, C** de la checklist (cf. document `tests-verrous.md`)

Un concept est **promu en V2** s'il passe en plus :

- ✅ Les tests **environnementaux** (E1-E5)
- ✅ Les tests **crash** (C1-C3)
- ✅ Un **test utilisateur** avec au moins 3 personnes différentes

---

## 9. Hors périmètre / décisions reportées

Les questions suivantes sont **explicitement reportées** à une itération ultérieure :

- **Antivol / traçabilité** : pas d'exigence dans V1. Le verrou est conçu pour la sécurité mécanique, pas la sécurité contre le vol.
- **Étanchéité IP** : pas d'exigence. Le verrou est dans un environnement intérieur sec en usage nominal.
- **Compatibilité avec rails existants L-track / ISOFIX du van** : non requise en V1. Le plateau wikivan se monte par-dessus l'aménagement van existant.
- **Esthétique signature** : le verrou est caché, l'esthétique n'est pas un critère V1.
- **Indicateur électronique d'état** : non requis. Indication mécanique suffit.

---

## 10. Concepts candidats retenus

Deux concepts ont été identifiés comme **candidats viables** lors de la phase de brainstorming. Voir documents de design séparés pour leurs spécifications détaillées.

### 10.1 WikiCam — Rotation + serrage auto-énergisé

- **Principe** : disque "escargot" Ø50 mm avec rampe hélicoïdale en spirale logarithmique. Insertion verticale + rotation 90° à la clé Allen.
- **Forces** : Auto-énergisation sous charge (la rampe se serre plus fort sous traction), résistant au tonneau seul, tolérant aux variations dimensionnelles du bois (humidité), tout-CNC simple.
- **Faiblesses** : Geste plus lent (clé Allen ×4), pas d'usage à 1 main.
- **Cas d'usage cible** : Modules permanents et lourds (lit, batterie, frigo, cuisine).

### 10.2 WikiClip + WikiStrap — Push-pull asymétrique + sangles secondaires

- **Principe** : Insert POM à 2 bras flexibles avec rampes asymétriques (entrée 30°, retenue 60°). Engagement par push gravitaire (~50 N), dégagement par pull manuel (~200 N). Sangles polyester secondaires obligatoires pour le crash.
- **Forces** : UX quotidienne ultra-fluide (2 secondes, 1 main, click audible), zéro outil, fluage POM faible.
- **Faiblesses** : Pas tonneau-friendly natif → exige WikiStrap en roulage. Cycle de vie POM à valider.
- **Cas d'usage cible** : Modules manipulés au quotidien (tiroirs, trappes, accessoires).

### 10.3 Coexistence

Les deux concepts **partagent la même grille** et le même perçage de base, et peuvent **coexister sur un même van** pour des usages différents.

---

## 11. Questions ouvertes (à trancher avant V1 final)

1. **Force seuil exact du WikiClip** : 150 N ou 250 N ? Test utilisateur à conduire.
2. **Matériau de l'insert WikiClip** : POM (recommandé) vs PETG imprimé 3D vs lame ressort acier ?
3. **Nombre de verrous par module** : 4 coins suffisent jusqu'à quelle taille de module ? À partir de quand ajouter des points intermédiaires ?
4. **Géométrie précise des sangles WikiStrap** : 2 en X suffit, ou faut-il un système plus structuré (4 sangles en pyramide) ?
5. **Pas de variation Allen** : 6 mm ou 8 mm pour le WikiCam ? Trade-off couple vs accessibilité de l'empreinte.

---

## 12. Références

- **Document brainstorming complet** : `brainstorming/brainstorming-session-2026-06-22-1043.md` (33 questions, 4 domaines d'inspiration, 3 concepts dont 2 retenus, 1 abandonné)
- **Checklist de tests** : à produire (`tests-verrous.md`) — 32 tests organisés en 4 itérations
- **Spec V1 WikiCam** : à produire (`design-wikicam-v1.md`)
- **Spec V1 WikiClip** : à produire (`design-wikiclip-v1.md`)
- **Inspirations industrielles** : conteneurs maritimes (twist-locks), escalade (cames SLCD logarithmiques), aviation cargo (L-track), photo (Arca-Swiss — abandonné)

---

## 13. Historique des versions

| Version | Date | Auteur | Notes |
|---|---|---|---|
| 1.0 | 2026-06-22 | Alexei | Première version après session brainstorming structurée (Phases 1-4) |
