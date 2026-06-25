# Addendum — FourchetteRusse

Détails de profondeur capturés pendant la discovery, destinés aux étapes aval (PRD, architecture). Ne fait pas partie du brief lui-même.

## Commandes du bot WhatsApp (esquisse)

| Commande | Rôle |
|----------|------|
| `/qui`   | Tire une personne au sort parmi le groupe et l'annonce dans le chat. |
| `/quoi`  | Enregistre le repas finalement mangé pour le tirage en cours. |
| `/stat`  | Affiche la répartition : qui a été tiré combien de fois, et le palmarès des repas. |

Libellés des commandes à confirmer au PRD (français vs autre, alias, format de réponse).

## Regroupement / normalisation des repas (à creuser au PRD)

Besoin exprimé : « pâtes bolo » et « pâtes carbo » doivent compter comme **« pâtes »** dans les stats. Le repas est saisi en texte libre mais agrégé par **catégorie**. Pistes à trancher en aval :

- **Catégories prédéfinies** (pâtes, burger, pizza, sushi, cuisine maison…) avec assignation à la saisie — simple, mais rigide.
- **Mapping mots-clés → catégorie** (le texte contient « pâtes » → catégorie « pâtes ») — souple, mais à maintenir.
- **Confirmation utilisateur** : le bot propose une catégorie devinée, l'utilisateur valide/corrige — plus fiable, un aller-retour de plus.

Décision : à prendre au PRD/archi. Le brief ne tranche volontairement pas.

## Frontière « négociation hors appli » — rationale

La validation du repas (accepter / refuser / contre-proposer) est explicitement **exclue** du produit. Raison : elle se fait déjà naturellement entre potes dans le chat WhatsApp, et la modéliser ferait exploser le scope (vote, états, messagerie) pour un projet fun à 4. Garder l'appli sur « désigner + enregistrer + montrer » est ce qui la rend construisible en v1.

## Questions techniques ouvertes (pour archi)

- **Intégration WhatsApp** : API WhatsApp Business officielle vs solution non officielle vs autre canal de notif — impact sur faisabilité et coût.
- **Identité des membres** : groupe fixe de 4. Mapping simple (4 noms en dur ?) plutôt que système de comptes.
- **Authentification web** : nécessaire ou interface ouverte/lien privé, vu l'usage perso ?
