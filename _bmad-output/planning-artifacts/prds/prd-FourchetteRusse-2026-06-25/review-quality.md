---
title: "Revue qualité adversariale — PRD FourchetteRusse"
reviewer: "Relecteur qualité adversarial"
date: 2026-06-26
gate: pass-with-nits
---

# Revue qualité adversariale — PRD FourchetteRusse

## Cadrage de la revue

Projet fun/hobby pour 4 amis, PRD volontairement court, rigueur légère assumée. Cette
revue ne pénalise donc PAS l'absence de métriques chiffrées, de personas, de matrices de
traçabilité ou de sections entreprise — ce sont des choix délibérés et corrects. Les
findings ci-dessous portent uniquement sur la **clarté testable**, la **cohérence
interne**, les **trous logiques** et les **hypothèses risquées non signalées**, calibrés
au niveau d'enjeu réel.

## Verdict global

**Gate : pass-with-nits.**

Le PRD est étonnamment solide pour son format. Le scope est net, les frontières hors-scope
sont explicites (rare et précieux), les `[ASSUMPTION]` sont honnêtement balisés, et les
questions techniques sont correctement déportées vers l'architecture. Un dev pourrait
commencer à concevoir dès maintenant. Les findings restants sont des ambiguïtés locales sur
quelques FR, pas des trous structurels. Aucun bloquant.

---

## Findings

### HIGH — H1 : « dernier tirage non encore renseigné » (FR6) vs « tirage le plus récent » (FR5) — collision de cibles non résolue

FR5 dit que l'annulation supprime **le tirage le plus récent** (et son repas rattaché).
FR6 dit que `/quoi` se rattache au **dernier tirage non encore renseigné**. Ces deux
notions de « dernier » divergent dès qu'il y a deux tirages d'affilée sans repas
intermédiaire — scénario plausible (deux `/qui` lancés pour rire avant de manger). Exemple :
tirage A (sans repas), puis tirage B (sans repas). À ce moment, `/quoi pizza` se rattache
à quoi ? Au plus récent (B) ou au plus ancien non renseigné (A) ? Et `/annule` vise B ; mais
si l'utilisateur voulait corriger un repas saisi sur A, le comportement est indéfini.

Ce n'est pas testable en l'état : un dev devra inventer la règle. Impact réel faible (4 amis,
ils riront du bug), mais c'est exactement le genre de zone grise qui produit un comportement
surprenant le premier vendredi. **Recommandation :** une phrase qui pose un invariant simple,
p. ex. « il ne peut exister qu'un seul tirage ouvert à la fois ; lancer `/qui` alors qu'un
tirage est déjà ouvert le remplace / est refusé / empile (choisir) ». Trancher le cas
multi-tirages-ouverts suffit à lever l'ambiguïté FR5/FR6.

### MEDIUM — M1 : FR7 (boucle de confirmation) sans chemin de sortie ni timeout

FR7 décrit une boucle « le bot propose une catégorie, l'utilisateur valide ou corrige ».
Mais rien ne dit ce qui se passe si l'utilisateur **ne répond jamais** (cas ultra-fréquent
sur WhatsApp : on enregistre le repas, on pose le téléphone). Le repas reste-t-il en attente
indéfiniment ? Compte-t-il dans les stats avant confirmation ? La catégorie devinée est-elle
appliquée par défaut au bout d'un délai ? Comment « corriger » concrètement — on retape
`/quoi`, on répond en texte libre, on choisit dans une liste ?

C'est la FR la moins testable du document. Un dev ne peut pas implémenter la boucle sans
décider de l'état « en attente de confirmation ». **Recommandation :** une ligne sur le
comportement par défaut (p. ex. « sans réponse, la catégorie devinée est retenue ; corrigeable
plus tard via une commande ») et sur le format de la correction.

### MEDIUM — M2 : « membres humains du groupe au moment du `/qui` » — source de vérité du pool non définie

FR1 + hypothèse du pool : le tirage porte sur « les membres humains du groupe au moment du
`/qui` ». D'où vient cette liste ? La plupart des intégrations WhatsApp (surtout non
officielles, justement listées en question ouverte) ne donnent pas de façon fiable la liste
des participants d'un groupe. Si le pool n'est pas dérivable de WhatsApp, il faut une liste
configurée en dur — ce qui est cohérent avec FR17/la config au démarrage, mais n'est écrit
nulle part. Risque : un membre absent du jour reste dans le pool, ou un nouveau venu n'y est
pas.

Hypothèse risquée semi-signalée. **Recommandation :** ajouter un `[ASSUMPTION]` explicite :
« le pool est une liste fixe configurée au démarrage » OU « dérivé dynamiquement de la
composition du groupe WhatsApp » — ce choix a un impact direct sur la faisabilité technique
et devrait au moins être nommé comme question ouverte aux côtés de l'intégration WhatsApp.

### MEDIUM — M3 : FR17 / déclenchement accidentel vs FR illustratives — la vraie contrainte de parsing est sous-spécifiée

FR17 justifie la configurabilité des libellés par « éviter les déclenchements accidentels
par un message normal du chat ». C'est un excellent réflexe. Mais le PRD ne dit jamais
**comment** une commande est reconnue : préfixe strict en début de message (`/qui`) ?
n'importe où dans le texte ? casse sensible ? Sans cette règle, « éviter les déclenchements
accidentels » n'est pas vérifiable — c'est une intention, pas une exigence testable. Le geste
théâtral public (FR3) aggrave le risque : un faux positif déclenche un tirage visible par
tous.

**Recommandation :** une phrase posant la règle de reconnaissance (p. ex. « commande = message
commençant exactement par le libellé configuré »). Léger, mais ça transforme FR17 en exigence
testable.

### LOW (nit) — N1 : `/stat` WhatsApp vs site web — périmètre temporel implicite

FR10 (`/stat` WhatsApp) ne dit pas sur quelle période il agrège (tout l'historique ? le mois
courant ?), alors que le web a des filtres explicites (FR16) et que le scénario parle de
« 3× ce mois ». Probablement « le mois en cours » par cohérence avec l'exemple, mais ce n'est
pas écrit. Trivial à trancher, mentionné pour complétude.

### LOW (nit) — N2 : `/annule` accessible « à n'importe quel membre » — frottement social non signalé

`[ASSUMPTION]` FR5 : l'annulation est ouverte à tous. Dans un produit dont l'âme est la
charrie autour des stats, n'importe qui peut effacer le dernier tirage (donc réécrire l'objet
même de la vanne). Ce n'est pas un bug — c'est cohérent avec la « simplicité radicale » — mais
c'est une décision sociale plus qu'un détail technique. À assumer consciemment. Aucune action
requise si c'est voulu.

---

## Ce qui est bien (pour calibrer la sévérité)

- **Frontières hors-scope explicites** (négociation, équité forcée, gestion lourde) : élimine
  les dérives de scope les plus probables. Excellent.
- **Contre-signaux de succès** (« l'appli devient une corvée », « les stats créent de vraies
  tensions ») : rare dans un PRD, et pertinent ici.
- **`[ASSUMPTION]` honnêtes et localisés** : le document signale ses propres zones molles au
  lieu de les masquer.
- **Séparation comment/quoi** : les questions techniques (intégration WhatsApp, hébergement,
  stockage, moteur de catégorisation) sont correctement déportées vers l'addendum architecture.
- **Le ton fun érigé en exigence non fonctionnelle** : cohérent avec l'intention produit, et
  testable de façon informelle (« est-ce que ça fait rire ? »).

## Synthèse décision-readiness

Un dev peut commencer à concevoir aujourd'hui. Avant d'écrire les premières lignes de la
logique métier, il devrait toutefois faire trancher **H1** (invariant « un seul tirage
ouvert ») et **M2** (origine du pool), car ces deux points conditionnent le modèle de données
et l'intégration WhatsApp. M1 et M3 peuvent être tranchés au moment d'implémenter les FR
concernées. Aucun de ces points ne justifie de bloquer le PRD.
