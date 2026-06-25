---
title: "Product Brief: FourchetteRusse"
status: draft
created: 2026-06-25
updated: 2026-06-25
---

# Product Brief : FourchetteRusse

## Résumé

Tous les vendredis, une bande de 4 amis se réunit pour manger ensemble — et tous les vendredis, la même scène se rejoue : « On fait quoi ? Fast food ? On cuisine ? » Personne ne veut trancher, et quand quelqu'un s'y colle, l'autre rengaine son éternel « mais c'est *tout le temps* moi qui décide ».

**FourchetteRusse** transforme cette galère en jeu : un tirage au sort désigne le décideur de la semaine via WhatsApp, le bot l'annonce, et tout est historisé. L'appli ne rend pas justice — elle **donne des munitions pour se charrier**. C'est un projet fun, pour 4 potes, dont le seul vrai objectif est de remettre du rire là où il y avait de l'hésitation.

## Le problème

Chaque vendredi soir, le même rituel d'indécision :

- **Personne ne veut décider.** Choisir le repas, c'est s'exposer : si ça ne plaît pas, c'est ta faute. Résultat, tout le monde se renvoie la balle et la soirée démarre dans le flou.
- **Le sentiment d'injustice.** Sans trace de qui a décidé les semaines passées, le débat tourne au ressenti : « c'est *toujours* moi » contre « mais non, t'exagères ». Impossible de trancher, parce que personne ne compte vraiment.
- **Le roulement existe déjà… mais à la main.** Le groupe a tenté un roulement hebdomadaire, sans outil pour le tenir : on oublie qui était de corvée, on rejoue les mêmes discussions, et le système s'essouffle.

Rien de dramatique — c'est un irritant de pote, pas un problème de société. Mais c'est récurrent, et ça gâche les premières minutes d'une soirée censée être détente.

## La solution

Un duo **bot WhatsApp + interface web**, centré sur deux gestes simples : **désigner** et **enregistrer**.

- **Le tirage (`/qui`).** N'importe qui lance la commande dans le groupe WhatsApp ; le bot tire une personne au sort et l'annonce. C'est rapide, public, et un peu théâtral — le coup de dé fait partie du fun.
- **L'enregistrement du repas (`/quoi`).** Après coup, on note ce qui a été mangé. Les stats **regroupent intelligemment** les libellés : « pâtes bolo » et « pâtes carbo » comptent tous les deux comme **« pâtes »**, pour des stats par catégorie plutôt que par texte exact.
- **Les stats (`/stat` + web).** À tout moment, on voit la répartition : qui a été tiré combien de fois, et ce qu'on mange le plus. C'est l'arme anti-« c'est tout le temps moi » — sauf qu'ici les chiffres servent à **vanner**, pas à arbitrer.

La personne tirée propose le repas ; la négociation qui suit (on accepte, on râle, on contre-propose) reste entre potes, **hors appli**. Et l'aléatoire est assumé : on peut retomber deux fois de suite sur la même personne — c'est tout le sel, puisque les stats sont justement là pour qu'on **s'auto-équilibre en rigolant**.

## Pour qui

Une bande de **4 amis** qui mangent ensemble chaque vendredi. Proches, déjà réunis sur un groupe WhatsApp commun, motivés par le fun plus que par l'efficacité. Le produit est conçu d'abord pour *ce* groupe-là — pas de prétention à scaler.

## Critères de succès

On saura que FourchetteRusse marche quand :

- **On l'utilise vraiment** — chaque vendredi, quelqu'un lance `/qui` au lieu de relancer le débat « on fait quoi ».
- **Le débat « c'est tout le temps moi » s'éteint** — ou mieux, il se transforme en vanne (« regarde les stats, t'as été tiré 2× en un mois, arrête de pleurer 😏 »).
- **On décide plus vite** — moins de temps perdu à tergiverser en début de soirée.
- **Ça fait rire** — l'âme du projet : `/stat` devient un running gag du groupe.

Pas de seuil chiffré : pour un projet fun, ces signaux ressentis suffisent.

## Hors scope (frontières explicites)

- **La négociation du repas.** Accepter / refuser / contre-proposer reste une affaire de potes, hors appli. On ne construit ni vote, ni messagerie, ni validation dans le produit.
- **Forcer l'équité.** Le tirage reste aléatoire ; l'appli n'impose jamais un tour de rôle « juste ». Elle montre, elle ne corrige pas.
- **La gestion lourde.** Pas de comptes complexes, pas d'admin, pas de multi-groupes à gérer pour la v1. Quatre noms, un groupe, c'est tout.

## Vision

À court terme, FourchetteRusse est un outil **purement perso** : il sert *ce* groupe de 4 amis, et c'est déjà tout son intérêt. S'il prend bien, on pourrait *éventuellement* l'ouvrir à d'autres petits groupes (colocs, familles) — mais c'est une piste très secondaire, à n'explorer que si l'envie et le plaisir d'usage sont au rendez-vous. Le succès ne se mesure pas en utilisateurs : il se mesure en vendredis plus légers.
