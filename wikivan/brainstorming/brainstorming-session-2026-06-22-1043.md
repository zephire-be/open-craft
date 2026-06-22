---
stepsCompleted: [1, 2, 3, 4]
inputDocuments: []
session_topic: 'Design d''un système de verrouillage universel pour les modules d''aménagement du van wikivan'
session_goals: 'Cahier des charges + inventaire des types de verrous + 2-3 propositions de design à prototyper + checklist de tests'
selected_approach: 'progressive-flow'
techniques_used: ['Question Storming', 'Cross-Pollination', 'Constraint Mapping', 'Morphological Analysis', 'Failure Analysis']
ideas_generated: '33 questions cahier des charges, 4 domaines explorés, 3 concepts (2 retenus), 32 tests'
outputs:
  - '../cahier-des-charges-verrous.md'
context_file: ''
---

# Brainstorming Session Results

**Facilitator:** Alexei
**Date:** 2026-06-22

## Session Overview

**Topic:** Design d'un système de verrouillage universel (un seul standard) pour tous les modules d'aménagement du van — projet wikivan
**Context:** Aménagement van / hardware — les "locks" désignent les mécanismes physiques de fixation, sécurisation et solidarisation entre modules d'aménagement modulaires.

### Objectifs (livrables attendus)

1. **Cahier des charges** des verrous — exigences communes à tous les modules
2. **Inventaire** des types de verrous envisageables
3. **2-3 propositions de design** prêtes à prototyper physiquement
4. **Checklist de tests** pour valider les prototypes

### Contraintes transverses

- Fabricable en **CNC 3 axes** (simplicité d'usinage, peu de setups)
- **Économie de matière**
- Recherche d'**équilibre** entre les deux

### Session Setup

**Approche :** Flux progressif (Progressive Technique Flow)

**Séquence de techniques :**
- **Phase 1 — Exploration :** Question Storming + Cross-Pollination
- **Phase 2 — Reconnaissance de motifs :** Constraint Mapping
- **Phase 3 — Développement :** Morphological Analysis
- **Phase 4 — Action :** Failure Analysis

## Technique Execution Results

### Phase 1 — Question Storming

**Objectif :** générer la matière première du cahier des charges via questions ouvertes.

**33 questions générées, organisées par domaine :**

#### Visibilité / intégration visuelle
- **Q4** — Le verrou doit-il être visible ou caché dans le module ?
- **Q5** — Si caché : comment sait-on qu'il est verrouillé (indicateur visuel, sonore, tactile) ?
- **Q6** — Si visible : est-ce un élément de design signature wikivan ou le plus neutre possible ?
- **Q7** — Faut-il deux types : "structurel/permanent caché" (lit) et "fonctionnel/quotidien visible" (trappes) ?

#### Outillage / ergonomie de déverrouillage
- **Q8** — Outil nécessaire pour déverrouiller, ou verrou auto-bloquant ?
- **Q9** — Si outil : standard (allen 6 pans) ou dédié (clé wikivan) ?
- **Q10** — "Auto-bloquant" = se verrouille seul à l'insertion (gravitaire/clipsage), ou résiste seul aux vibrations sans s'engager activement ?
- **Q11** — Faut-il 3 états : engagé / verrouillé / cadenassé (anti-vol) ?

#### Géométrie / directionalité
- **Q12** — Verrouillage uniquement par le bas, ou par tous les côtés ?
- **Q13** — Combien de points de fixation par module (un central vs plusieurs répartis) ?
- **Q14** — Même verrou pour modules sol/mur/plafond, ou 3 variantes ?
- **Q15** — Direction d'insertion imposée ou libre ?

#### Forces & sécurité crash
- **Q16** — La force d'accroche doit-elle supporter un tonneau ?
- **Q17** — Norme de référence (ECE R17, données crash RV) ou cible propre (40 kg max + 20g) ?
- **Q18** — Verrou = maillon le plus solide, ou fusible volontaire qui lâche pour absorber l'énergie ?
- **Q19** — Design isotrope (résiste pareil toutes directions) ou direction privilégiée acceptée ?

#### Matériaux & process
- **Q20** — Verrou dans le même matériau que le module ?
- **Q21** — Si même matériau : pièce d'usure remplaçable nécessaire ?
- **Q22** — Combien de process différents acceptés (CNC bois + tournage + impression 3D) ?
- **Q23** — Fixations du commerce (vis, inserts, aimants) autorisées comme briques de base, ou bannies ?

#### Interface van & réparabilité
- **Q24** — Verrous spéciaux pour l'interface module-van ?
- **Q25** — Verrou remplaçable, ou solidaire du module (si cassé → on jette) ?
- **Q26** — S'aligner sur standards existants (L-track aviation, T-slot 8020, Sortimo) ou rail wikivan propriétaire ?
- **Q27** — Si remplaçable : par l'utilisateur sur la route ou retour atelier ? Consommable bon marché ou pièce durable ?
- **Q28** — Un design unique module-module ET module-van, ou deux familles assumées ?
- **Q29** — Rôle anti-vol / traçabilité (numéros de série) ou hors sujet ?

#### Cycle de vie / usure
- **Q30** — Combien de cycles avant défaillance ?
- **Q31** — Profil d'usage : 50 cycles (lit, 25 ans) vs 50 000 cycles (trappe quotidienne) — même verrou pour les deux ?
- **Q32** — Défaillance progressive et visible, ou brutale acceptable ?
- **Q33** — Indicateur d'usure et intervalle de maintenance, ou fit & forget ?

**Domaines non couverts** (à reprendre en Phase 2 ou plus tard) : étanchéité/poussière, acoustique en roulant, coût cible, esthétique signature, délai de fab unitaire.

**Phase 1 — TERMINÉE** (transition vers Cross-Pollination).

### Phase 2 — Cross-Pollination

#### Domaine 1 — Conteneurs maritimes (twist-locks) ✅ RETENU comme candidat

**Mécanique :** cône métallique inséré gravitairement dans un *corner casting* normalisé, rotation 90° → baïonnette ovale qui bloque en cisaillement.

**Principes transférables à wikivan :**
- Baïonnette quart-de-tour avec géométrie ovale (passe/bloque selon orientation)
- Engagement gravitaire à l'insertion + sécurisation manuelle ensuite
- Format d'interface unique normalisé (un seul "casting" partout)
- 100% CNC 3 axes (tout est dans le plan horizontal)
- Économie de matière maximale (un ovale + un perçage)

**Pièges :**
- Besoin de gravité → moins évident pour modules suspendus/muraux
- Risque de rotation inverse sous charge sans goupille de sécurité secondaire

#### Concept "WikiCam" — fusion twist-lock × came logarithmique ✅ CANDIDAT #1

**Description :** disque-escargot Ø 40-60 mm avec rampe hélicoïdale spirale logarithmique en face supérieure. Insertion gravitaire dans une lumière trou-de-serrure, rotation 90° à la clé Allen → la rampe se coince contre une butée fixe et tire le module en serrage.

**Adresse les requis :** Q4 (caché), Q8-9 (clé Allen standard), Q10-11 (auto-bloquant + 3 états possibles), Q12-15 (isotrope, fonctionne toutes orientations), Q16-19 (auto-énergisation sous charge crash), Q20-23 (CNC 3 axes + insert métallique mini), Q24/Q28 (même pièce module-module et module-van), Q30-33 (insert d'usure remplaçable).

**À valider en proto :** angle de rampe vs angle de friction, pression locale, précision usinage trou-de-serrure, tenue aux vibrations longues.

**Estimation CNC :** 3-5 min mâle + 2-3 min femelle dans CPL bouleau 18 mm, 2 pièces tirées d'une chute 80×80 mm.

#### 🧱 CONTRAINTE ARCHITECTURALE — "Lego baseplate" (décision majeure)

**Décision Alexei :** l'interface van = plateau quadrillé avec ancrages à pas régulier. Les modules ont des tailles quantifiées (1U, 2U, 3U…) et peuvent se poser à n'importe quelle position de la grille.

**Implications pour le verrou :**
- Ancrages omniprésents sur toute la grille (pas seulement à des positions définies)
- Chaque coin de module tombe sur un nœud de grille (compatible avec 4 cams par module si tailles xU)
- **Question ouverte critique : quel est le pas U ?** (50 mm / 100 mm / 96 mm MFT Festool / 25.4 mm L-track / autre)

#### Domaine 3 — Aviation cargo L-track ✅ Inspiration architecturale

**Principes transférables retenus :** positionnement quasi-continu (1D Lego), engagement par glissement + plunger gravitaire, écosystème d'accessoires partagés, standard d'interface unique.

**Décision dérivée :** la grille wikivan est une généralisation 2D du L-track (plaque trouée régulièrement), pas un rail extrudé.

#### 🎯 DÉCISION MAJEURE — Pas U = 100 mm + écosystème "Bus adapters"

**Pas U = 100 mm** — décimal pur, lecture cm directe (1U=10cm).

**Architecture "bus + adapters"** : la grille ne connaît que WikiCam. La compatibilité avec écosystèmes externes (Systainer, L-Boxx, Packout, bouteilles gaz, cassettes eau, etc.) se résout par des **modules adaptateurs spécialisés** dans le catalogue, sans contaminer la grille.

**Trois variantes Systainer-Bus retenues au catalogue :**

| Variante | Format | Cas d'usage | Espace perdu |
|---|---|---|---|
| **Bus-Drawer** | 5U×4U×3U (500×400×300 mm), Systainer en tiroir sur glissières | Van vie/famille, protection poussière | ~30% interne |
| **Bus-Rack** | Châssis squelettique 2 rails au pas U, Systainers à vue empilés | Van workshop assumé, esthétique atelier | ~0% |
| **Bus-Foot** | Plaque-pied 12mm vissée sous Systainer existant, embarque 4 WikiCams | Rétrocompatibilité collection Festool existante | ~0% |

**Plateau van standard 1200×2400 mm = 12U × 24U = 288 trous d'ancrage WikiCam.**

**Modules de référence (en U=100 mm) :**
- Tiroir Systainer-Drawer : 5U × 4U × 3U
- Module cuisine compact : 6U × 4U
- Module évier : 6U × 5U
- Module frigo 12V Dometic : 6U × 4U
- Module lit (largeur 80 cm) : 9U × 22U
- Bouteille de gaz 6kg : 3U × 3U × 4U
- Cassette eau Thetford : 4U × 2U × 5U

#### Concept "WikiClip" — snap-fit asymétrique à libération calibrée ✅ CANDIDAT #2

**Description :** mécanisme à deux bras flexibles en POM (insert d'usure) avec rampes asymétriques : rampe d'entrée 30° (engagement gravitaire ~50 N) et rampe de retenue 60° (libération manuelle ~200 N). Engagement = push, libération = pull. Aucun outil en routine.

**Stratégie 2 couches :** WikiClip optimisé pour l'ergonomie quotidienne + **WikiStrap** (sangles polyester 25mm avec Fastex + cliquet ratchet, 2 par module en X) pour la sécurité crash/tonneau.

**UX différenciateur :** geste à 1 main possible, 2 secondes par module, click audible.

**À valider en proto :** calibration angles ramps, cycle de vie POM (>100k cycles), force seuil 150-250N, stabilité latérale sans sangle.

#### Domaine 4 — Arca-Swiss : queue d'aronde universelle ✅ Inspiration concept #3

**Principes transférables :** positionnement continu, engagement par glissement, levier excentrique unique, auto-centrage, force massive pour la taille.

#### Concept "WikiDovetail" — sliding queue d'aronde + levier excentrique ❌ ABANDONNÉ

**Description :** plateau van avec rainures queue d'aronde 45° parallèles, modules à patins glissés + levier excentrique.

**Raison de l'abandon :** incompatible avec la grille Lego 2D. La grille est un invariant architectural prioritaire pour wikivan (décision Alexei). WikiDovetail oblige à glisser depuis le bout du rail → impossible d'insérer un module au milieu d'un van plein.

**Conservé pour mémoire** au cas où une future version "rail" du wikivan ferait sens.

#### 🏆 Duo finaliste à prototyper (WikiDovetail abandonné, voir ci-dessus)

| Critère | 🥇 WikiCam | 🥈 WikiClip |
|---|---|---|
| Philosophie | Rotation + serrage auto-énergisé | Push-pull + détente calibrée |
| Geste pose | Allen ×4 quarts-de-tour (~30s) | Push (~2s) |
| Outil routine | Clé Allen 8 mm | Aucun |
| Architecture | Grille Lego ✅ | Grille Lego ✅ |
| Force vs tonneau | Très bonne (auto-énergisée) | Faible (besoin WikiStrap secondaire) |
| 1 main | Non | **Oui** |
| Démontage au milieu | ✅ | ✅ |
| Cas d'usage cible | Modules "permanents" (lit, batterie, cuisine) | Modules "quotidiens" (trappes, tiroirs, accessoires) |

**Insight stratégique :** les deux concepts sont **complémentaires plutôt que concurrents**. Un même van peut équiper certains points d'ancrage en WikiCam (modules lourds, structurels) et d'autres en WikiClip (modules manipulés quotidiennement). Les deux partagent la même grille et le même perçage de base.

**Phase 2 — TERMINÉE.**

### Phase 3 — Morphological Analysis ✅ TERMINÉE

**3 specs V1 produites, puis WikiDovetail abandonné en finale → 2 specs retenues.**

#### Spec WikiCam V1
- Ø disque 50 mm × épaisseur 15 mm en CPL bouleau 18 mm
- Rampe spirale log, montée 5 mm sur 270°, angle effectif ~10°
- Interface Allen 8 mm
- Insert HPL 2 mm sur zone d'usure
- Pièce femelle : pocket 52×52×5 mm + butée vissée 2 vis 4×16
- Temps CNC : ~6 min/lock. Coût matière : ~0,80 €.

#### Spec WikiClip V1
- Insert POM 6 mm épaisseur, 2 bras opposés profil "C" tapered
- Rampes 30°/60° → forces engagement 50 N, libération 200 N
- Fente "sablier" femelle intégrée au perçage grille (compatible WikiCam)
- WikiStrap : sangle polyester 25 mm, 800 daN, Fastex + cliquet, 2× X par module
- Temps CNC : ~3 min bois + 1 min POM. Coût matière : ~0,50 € (+5 € sangles/module).

#### Approche de prototypage : 3 mini-bancs 300×300 mm
Pas de plateau van complet pour la V1. Permet itération rapide et comparaison côte-à-côte.
**Investissement total proto : ~50 min CNC + ~30 € matière + quincaillerie.**

### Phase 4 — Failure Analysis ✅ TERMINÉE

**32 tests dérivés des modes de défaillance**, organisés en 4 catégories :
- **A. Tests communs** (15 tests) : fonctionnels, mécaniques quantifiés, environnementaux, crash
- **B. Tests spécifiques WikiCam** (7 tests) : auto-serrage, couple Allen, cycle HPL, vibration, précision, cisaillement
- **C. Tests spécifiques WikiClip** (9 tests) : calibration forces, fatigue POM, fluage, click audibility, WikiStrap
- **Plan d'exécution** : 4 itérations avec 3 gates de décision intermédiaires

**Outillage de test recommandé** : peson digital, pied à coulisse, comparateur, banc test maison. Investissement total ~100-150 €.

## Synthèse finale

**Outputs produits par la session :**
- ✅ **Cahier des charges complet** (33 exigences → document propre `../cahier-des-charges-verrous.md`)
- ✅ **Inventaire des types de verrous** (4 domaines explorés + 3 concepts dérivés)
- ✅ **2 propositions de design V1** prêtes à prototyper (WikiCam + WikiClip)
- ✅ **Checklist de 32 tests** avec plan d'exécution en 4 itérations

**Décisions architecturales majeures actées :**
- Grille Lego 2D au pas U = 100 mm
- Architecture "Bus + Adapters" pour la compatibilité écosystèmes externes
- Coexistence acceptée des deux concepts sur la même grille
- Stratégie de sécurité crash à 2 couches (WikiClip + WikiStrap)

**Prochaines actions concrètes :**
1. Dessiner les fichiers CNC des 2 concepts (CAD)
2. Approvisionner CPL bouleau 18mm, plaque POM 6mm, plaque HPL 2mm, quincaillerie
3. Fabriquer les 3 bancs test
4. Exécuter Test Plan itérations 1-2





