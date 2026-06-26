# Réconciliation Brief → PRD — FourchetteRusse

**Date :** 2026-06-26
**Sources :** `brief.md`, `addendum.md`
**Cible :** `prd.md`

## Verdict global

Le PRD est **très fidèle** aux sources. Il préserve l'âme du projet (le fun comme
exigence, pas comme habillage), reprend mot pour mot les signaux de succès, le hors-scope,
la vision, et **ajoute même de la valeur** utile et bien dans le ton : les contre-signaux
(« l'appli devient une corvée », « les stats créent de vraies tensions »), le scénario
« Un vendredi type », et la boucle de confirmation de catégorie (qui tranche proprement
une question laissée ouverte par l'addendum).

La rigueur reste légère et le PRD reste court — conforme au ton voulu. Aucune section
lourde ne manque et il ne faut pas en réclamer.

Restent quelques **pertes qualitatives réelles**, listées par ordre d'importance.

## Écarts / pertes de sens

### 1. Le roulement manuel qui s'est essoufflé (perte de contexte fondateur)

Le brief (l.22) pose **trois** racines au problème, dont celle-ci :

> « Le roulement existe déjà… mais à la main. Le groupe a tenté un roulement
> hebdomadaire, sans outil pour le tenir : on oublie qui était de corvée, on rejoue
> les mêmes discussions, et le système s'essouffle. »

C'est le **pourquoi historique** du produit : ils ont déjà essayé une solution, qui a
échoué faute d'outil. Le PRD ne reprend nulle part ce constat. Or il justifie l'existence
même de l'historisation (FR4, FR9) et du caractère automatisé du tirage. Sans lui, le
produit ressemble à une lubie ; avec lui, c'est la réponse à un échec vécu.

**Recommandation :** une phrase dans « Contexte » du type « le roulement manuel s'essoufflait
faute de mémoire — d'où l'historisation ».

### 2. « C'est tout le sel » / « s'auto-équilibre en rigolant » (ton aplati en mécanique)

Le brief (l.34) justifie l'aléatoire pur de façon vivante :

> « on peut retomber deux fois de suite sur la même personne — c'est tout le sel, puisque
> les stats sont justement là pour qu'on **s'auto-équilibre en rigolant**. »

Le PRD couvre la mécanique (FR2 : aléatoire pur, pas de roulement forcé) mais perd
l'**intention** : l'aléatoire n'est pas une limite technique assumée, c'est *volontairement*
ce qui fait le jeu, et les stats sont l'**auto-régulation par la vanne** (pas un correctif).
FR2 lu seul peut donner envie à un dev de « corriger » l'injustice — exactement ce que le
brief veut éviter.

**Recommandation :** ajouter à FR2 (ou en note) le rationale « rejouer la même personne fait
partie du jeu ; l'équilibrage passe par la vanne, pas par le code ».

### 3. « La personne tirée propose le repas » (rôle du décideur hors récit)

Le brief (l.34) définit clairement le rôle : le tiré **propose**, il n'impose pas — la suite
(accepter/râler/contre-proposer) est hors appli. Le PRD ne porte cette idée que dans le
scénario narratif (« Tom râle, propose une pizza »). Elle n'apparaît jamais dans le Contexte
ni dans les FR. Mineur, mais c'est le pivot narratif (désigner ≠ décider seul), et il mérite
une ligne hors du récit pour ne pas dépendre d'une anecdote.

## Non-écarts (vérifiés, bien préservés)

- **Le fun comme exigence** : explicitement érigé en exigence transverse (« un `/stat` plat
  et corporate serait un échec produit »). Excellent, c'est même renforcé.
- **Négociation hors scope** + rationale : conservé fidèlement (Hors scope + scénario).
- **Catégorisation / agrégation** : l'addendum laissait 3 pistes ouvertes ; le PRD tranche
  pour la confirmation utilisateur (FR7) et garde l'exemple « pâtes bolo + pâtes carbo = pâtes ».
- **Mot de passe partagé / pas de comptes** : repris (FR12, exigences transverses).
- **Vision « vendredis plus légers »** : reprise quasi mot pour mot.
- **Questions techniques ouvertes** de l'addendum : reportées proprement vers l'archi.
