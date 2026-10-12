# Évaluation — jr-06-product · 20 points

| Critère | Points | Preuve attendue |
|---|---:|---|
| Story, critères et plan de tranche | 3 | Cinq critères vérifiables, ordre de travail et revue prévue. |
| Lecture serveur cohérente et sans création | 3 | GET avant/après transition, inconnu404 et compteur PSP inchangé. |
| Reprise React avec référence ou intention | 5 | GET sans POST si id connu ; même clé si réponse perdue ; stockage versionné minimal. |
| Messages, clavier et données locales invalides | 3 | Absence, réseau, attente et échec distingués ; nouvelle démo ciblée. |
| Tests, scénario navigateur et diagnostic | 4 | Critères couverts, même référence après reload, incident rouge puis corrigé. |
| Revue, démonstration et bilan honnête | 2 | Version, remarques du pair, pitch et limites explicites des 27 heures. |
| **Total** | **20** | |
## Règles de restitution

Un fichier présent n’obtient pas à lui seul tous les points : montrer son comportement et pouvoir l’expliquer. Les cas négatifs sont aussi importants que le nominal. Le code rendu ne contient aucune vraie donnée bancaire ou identité PSP ; tous les scénarios restent locaux et fictifs.

Associer chaque preuve à la version remise. Le journal mentionne l’acquis repris du lab précédent et toute base fournie par l’encadrant. Une revue locale est recevable lorsqu’aucun dépôt distant n’est utilisé. Ne pas créer de fausse URL de commit, de pull request ou de rapport.

En cas de panne d’outillage documentée, l’encadrant distingue les critères vérifiés sur un poste de secours, les contrôles relus mais non exécutés et les preuves reportées. Un test navigateur non exécuté ne reçoit pas l’étiquette « E2E réussi ». Une reprise de preuve peut être organisée ; le résultat obtenu reste daté et attribué au bon environnement.

Pour valider la séance, le formateur dispose d’un ensemble fonctionnel sur les objectifs obligatoires, de tests pertinents, d’un diagnostic expliqué et de limites honnêtes. Une extension ne compense pas un montant client accepté, une duplication masquée ou un faux succès affiché.

## Questions d’oral

1. Expliquer la différence entre reprise et nouvelle intention.
2. Justifier chaque donnée conservée dans la session.
3. Présenter les compétences acquises sans les confondre avec une expérience senior.
