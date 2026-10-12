# Mémos 06 — Livrer une tranche dont on peut expliquer le résultat

## Une demande devient observable

« Reprendre mon paiement » est ambigu : faut-il créer une nouvelle tentative, retrouver une référence ou vérifier un état ? Les critères du lab lèvent ces ambiguïtés. Une tranche verticale inclut le comportement React, la lecture Java et sa preuve. La découper en « faire le backend » puis « faire le frontend » sans résultat utilisateur intermédiaire rend la revue moins concrète.

## Une session conserve une piste, pas une vérité métier

Le stockage de session peut aider cette page à retrouver une référence. Il peut être absent, invalide ou contenir une information ancienne. Le cours conserve une intention versionnée et le serveur reste l’autorité pour statut et montant. Lire une chaîne `SUCCEEDED` dans le navigateur ne prouve pas qu’un événement a été validé dans l’API.

Une référence manquante après redémarrage mémoire signifie que notre processus ne la connaît plus. Elle ne démontre pas l’échec d’un paiement. Le message produit doit distinguer absence, indisponibilité et échec terminal réel du simulateur.

## Une nouvelle action n’est pas une reprise

Retrouver une référence connue est une lecture. Retrouver une création dont la réponse a été perdue réutilise l’intention et la clé déjà enregistrées. Commencer une nouvelle démonstration est une autre action explicite. Nommer séparément ces actions empêche une boucle automatique de créations et aide le pair à vérifier le produit.

## Planifier et revoir à petite échelle

Un objectif, quelques critères et une définition de terminé donnent une base commune au binôme. La séance emprunte des pratiques de transparence, inspection et adaptation ; elle n’est pas un Sprint Scrum complet. Le Scrum Guide sert de référence de vocabulaire, et non de justification à une conformité inventée. [Scrum Guide](https://scrumguides.org/scrum-guide.html).

## Une revue utile discute un changement

Le relecteur cherche à savoir quel problème a été résolu, quels comportements ont changé et comment on le sait. Un diff limité facilite cette discussion. Une note de revue explique la raison d’un choix — par exemple un GET au rechargement — et son effet observable — aucune nouvelle tentative. Les rapports deviennent des preuves parce qu’ils sont rattachés à cette version.

## Un bilan professionnel crédible

« J’ai construit et testé un checkout simulé » est vérifiable. « Je maîtrise les paiements de production » demanderait d’autres expériences et responsabilités. Nommer ses limites ne diminue pas le travail réalisé ; cela aide à choisir la prochaine pratique : base transactionnelle, autorisation d’accès, exploitation, collaboration prolongée ou intégration d’un prestataire dans un cadre approprié.

## Questions de compréhension

1. Pourquoi un GET de reprise ne doit-il pas appeler PspGateway ?
2. Que faut-il enregistrer avant la première demande si sa réponse peut être perdue ?
3. Que signifie 404 après redémarrage dans cette simulation ?
4. Quel critère d’acceptation est protégé par chacun des tests nouveaux ?
5. Quelle expérience vous manque encore pour assumer une application de paiement en production ?
