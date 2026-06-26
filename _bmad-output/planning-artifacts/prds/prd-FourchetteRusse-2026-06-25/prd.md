---
title: "PRD: FourchetteRusse"
status: final
created: 2026-06-25
updated: 2026-06-26
---

# PRD : FourchetteRusse

## Contexte

Tous les vendredis, une bande de **4 amis** mange ensemble. Et chaque vendredi, la même scène : « On fait quoi ? ». Personne ne veut trancher, et celui qui s'y colle récolte le « mais c'est *tout le temps* moi qui décide ». Le groupe a bien tenté un **roulement à la main**, mais sans outil pour le tenir : on oublie qui était de corvée, on rejoue les mêmes débats, et le système s'essouffle.

**FourchetteRusse** transforme cette indécision en jeu. Un duo **bot WhatsApp + site web** désigne le décideur par tirage au sort, enregistre ce qui a été mangé, et historise tout. La personne tirée **propose** le repas — elle ne l'impose pas ; la négociation qui suit reste entre potes, hors appli. Le but n'est pas de rendre justice : c'est de **donner des munitions pour se charrier**. Projet fun, pour *ce* groupe-là, sans prétention à scaler.

**Surfaces :** WhatsApp (geste rapide, public, théâtral) + site web (consultation détaillée). **Stakes :** hobby/perso.

## Objectifs & signaux de succès

Le produit marche quand :

- **On l'utilise vraiment** — chaque vendredi quelqu'un lance `/qui` au lieu de relancer le débat.
- **Le « c'est tout le temps moi » s'éteint** — ou mieux, devient une vanne (« regarde les stats, t'as été tiré 2× ce mois, arrête de pleurer 😏 »).
- **On décide plus vite** — moins de tergiversation en début de soirée.
- **Ça fait rire** — `/stat` devient un running gag du groupe.

Pas de seuil chiffré : pour un projet fun, ces signaux ressentis suffisent.

**Contre-signaux (à surveiller) :** l'appli devient une corvée et on arrête de lancer `/qui` ; ou les stats créent de vraies tensions au lieu de vannes. Si l'un des deux apparaît, le produit a raté son âme.

## Périmètre

**Dans le scope (v1) :** désigner (`/qui`), enregistrer le repas (`/quoi`), consulter les stats (résumé `/stat` dans WhatsApp + détail sur le web).

**Hors scope (frontières explicites) :**

- **La négociation du repas.** Accepter / refuser / contre-proposer reste une affaire de potes dans le chat, hors appli. Pas de vote, pas de validation, pas de messagerie intégrée.
- **Forcer l'équité.** Le tirage reste aléatoire. L'appli **montre**, elle ne corrige pas — elle n'impose jamais un tour de rôle « juste ».
- **La gestion lourde.** Pas de comptes individuels, pas d'admin, pas de multi-groupes. Un groupe, c'est tout.

## Un vendredi type

> 18 h, le groupe WhatsApp s'agite : « On fait quoi ce soir ? ». Au lieu de relancer le débat, **Léa** tape `/qui`. Le bot répond, un brin théâtral : « 🎲 Le sort a parlé… c'est **Tom** ! ». Tom râle pour la forme, propose une pizza ; la négociation suit dans le chat (hors appli). Après le repas, Tom tape `/quoi pizza 4 fromages`. Le bot devine : « Catégorie : 🍕 *Pizza* — c'est bon ? ». Tom valide. Plus tard, **Sam**, suspicieux, lance `/stat` : Tom a été tiré 3× ce mois. Vanne immédiate. Pour le détail, Sam ouvre le site web et exhibe l'historique des 6 derniers vendredis. Rires. Soirée lancée.

## Exigences fonctionnelles

> **Note sur les libellés.** Les commandes sont notées `/qui` `/quoi` `/stat` `/annule` à titre **illustratif**. Leurs libellés réels sont configurables et peuvent changer (voir FR17) — l'important est le geste, pas le mot exact.

### F1 — Tirage au sort (`/qui`)

- **FR1.** N'importe quel membre peut envoyer la commande de tirage dans le groupe WhatsApp ; le bot tire au sort une personne parmi les **membres humains du groupe** et l'annonce dans le chat.
- **FR2.** Le tirage est **purement aléatoire** : la même personne peut être tirée plusieurs fois de suite. Aucun roulement forcé, aucune exclusion du dernier tiré. **Retomber sur la même personne est assumé** — c'est tout le sel : ce sont les stats (et les vannes) qui rééquilibrent, jamais le code.
- **FR3.** L'annonce est **publique dans le chat** et volontairement un peu théâtrale (le « coup de dé » fait partie du fun).
- **FR4.** Chaque tirage est **historisé** (date + personne tirée) pour alimenter les stats.
- **FR5.** Une **commande d'annulation** supprime le **tirage courant** (et le repas éventuellement déjà rattaché) afin qu'il ne soit **pas comptabilisé dans les stats**. Cas d'usage : un tirage lancé par erreur. `[ASSUMPTION]` l'annulation cible le seul tirage courant et est accessible à n'importe quel membre.

> **Invariant — « tirage courant ».** À tout moment, le tirage courant est **le plus récent**. `/quoi` (FR6) et l'annulation (FR5) opèrent toujours sur ce tirage-là. Un nouveau tirage devient le tirage courant ; le précédent reste dans l'historique tel quel (avec ou sans repas). Cela évite toute ambiguïté quand plusieurs tirages s'enchaînent.

### F2 — Enregistrement du repas (`/quoi`)

- **FR6.** Après la soirée, un membre envoie la commande d'enregistrement suivie d'un **texte libre** pour noter le repas réellement mangé, rattaché au **tirage courant** (cf. invariant ci-dessus).
- **FR7.** Le bot **devine une catégorie** à partir du texte libre, la **propose**, et l'utilisateur **valide ou corrige** (boucle de confirmation). C'est ce qui garde les stats propres. `[ASSUMPTION]` si l'utilisateur ne répond pas à la proposition, la **catégorie devinée est retenue par défaut** (le repas n'est jamais perdu) et reste corrigeable plus tard.
- **FR8.** Les stats **agrègent par catégorie normalisée**, pas par texte exact : « pâtes bolo » et « pâtes carbo » comptent tous deux comme **« pâtes »**.
- **FR9.** Le repas enregistré (texte saisi + catégorie confirmée + date) est **historisé**.

### F3 — Stats rapides dans WhatsApp (`/stat`)

- **FR10.** La commande de stats affiche dans le chat un **résumé rapide** : répartition des tirages par personne + palmarès des repas par catégorie.
- **FR11.** Le format est **concis et adapté à WhatsApp** (texte + emoji), pensé pour la vanne « sur le pouce » — pas pour l'analyse fine (ça, c'est le web).

### F4 — Site web de stats détaillées

- **FR12.** Un **site web compagnon** affiche les stats détaillées, protégé par un **mot de passe partagé unique** (pas de comptes individuels).
- **FR13.** Vue **tirages par personne** : compteur + classement.
- **FR14.** Vue **palmarès des repas par catégorie**, présentée sous forme de graphique.
- **FR15.** **Historique chronologique** des vendredis (frise/journal : qui a décidé, ce qui a été mangé, quand).
- **FR16.** **Filtres par période** (mois / depuis le début / plage de dates) applicables aux vues de stats.

### F5 — Configuration des commandes

- **FR17.** Les **libellés des commandes** (tirage, enregistrement, stats, annulation) sont **configurables** et ne sont pas figés dans le code. Ils peuvent être redéfinis — y compris vers des libellés inhabituels ou dans une autre langue — afin **d'éviter les déclenchements accidentels** par un message normal du chat. La reconnaissance se fait sur un **préfixe/format strict** (le message doit correspondre exactement au libellé configuré, en début de message) pour limiter les faux positifs. `[ASSUMPTION]` configuration faite une fois au démarrage (pas d'UI de réglage en libre-service en v1).

## Exigences transverses (non fonctionnelles)

- **Simplicité radicale.** Pas de comptes, pas d'admin, pas de multi-groupes. Si une fonction demande une page de config, elle est probablement hors scope v1.
- **Le ton fun est une exigence, pas un habillage.** Les réponses du bot et le `/stat` doivent *donner des munitions pour vanner* — un `/stat` plat et corporate serait un échec produit, même si techniquement correct.
- **Dépendance WhatsApp.** Le canal principal repose sur WhatsApp ; sa disponibilité conditionne les gestes `/qui` `/quoi` `/stat`.
- **Données privées, faible volume.** Quelques entrées par semaine, lisibles uniquement par les 4 (mot de passe partagé). Pas d'exigence de conformité au-delà du bon sens « ça reste entre nous ».

## Hypothèses

- `[ASSUMPTION]` Un **seul groupe WhatsApp** et un seul bot ; pas de gestion multi-groupes.
- `[ASSUMPTION]` Le bot **s'exclut lui-même** du pool de tirage ; le pool = les membres humains du groupe au moment du `/qui`.
- `[ASSUMPTION]` Le **vendredi n'est pas automatisé** : c'est un membre qui lance le tirage (pas de relance programmée par le bot en v1).

## Questions ouvertes (pour l'architecture / addendum)

Ces points sont techniques (le *comment*) et seront tranchés à l'étape architecture — voir `addendum.md` :

- **Intégration WhatsApp** : API WhatsApp Business officielle vs solution non officielle vs autre canal — impact faisabilité et coût.
- **Source du pool de tirage (FR1)** : récupérer la liste des membres humains du groupe dépend de l'intégration WhatsApp retenue et n'est pas garantie (surtout en non officiel). **Repli** si indisponible : une liste de prénoms configurée une fois (rejoint la config de FR17).
- **Hébergement** du bot + du site web.
- **Stockage** de l'historique (tirages + repas + catégories).
- **Moteur de catégorisation** : comment le bot devine la catégorie (mots-clés, liste de référence, autre).

## Vision

À court terme, FourchetteRusse est un outil **purement perso** : il sert *ce* groupe de 4, et c'est déjà tout son intérêt. S'il prend bien, on pourrait *éventuellement* l'ouvrir à d'autres petits groupes (colocs, familles) — piste très secondaire, à n'explorer que si le plaisir d'usage est au rendez-vous. Le succès ne se mesure pas en utilisateurs : il se mesure en **vendredis plus légers**.
