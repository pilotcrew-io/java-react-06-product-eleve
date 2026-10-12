# Lab 06 — Livrer une évolution et défendre ses choix

**4 h 30 · Java et React.** Demande produit : « J’ai lancé une tentative, puis rechargé la page. Je veux retrouver son état sans envoyer un nouveau paiement. Si le service de démo a été remis à zéro, je veux le comprendre. » Vous devez transformer cette phrase en un changement testable, le réaliser et en expliquer les limites.

## 1. Définir une tranche réalisable — 25 min

Écrire la story avec cinq critères obligatoires :

1. Une référence de paiement connue est consultée après rechargement avec GET ; aucun nouveau POST de paiement n’est envoyé.
2. Une demande initiale dont la réponse a été perdue peut être retrouvée avec sa commande, sa méthode et **sa clé d’origine** après une action explicite.
3. Le statut affiché après reprise provient du serveur ; une ancienne valeur stockée ne devient pas un succès affiché.
4. Une référence absente après réinitialisation mémoire produit un message clair et une action pour commencer une nouvelle démonstration.
5. Un enregistrement local incomplet ou illisible ne bloque pas l’interface et ne déclenche aucune création automatique.

Proposer un petit plan avec ordre, responsable et point de revue. En binôme, alterner conduite/relecture après la première preuve de reprise. Annoncer ce qui entre dans la séance ; identité utilisateur, historique multiappareils et persistance serveur restent des extensions hors temps.

## 2. Vérifier la lecture côté Java — 45 min

Reprendre `GET /api/payments/{id}`. Cette route doit retrouver le Payment actuel, y compris après un événement, sans appeler le mock PSP ni créer de clé. Ajouter une preuve de lecture avant et après transition, ainsi qu’un cas de référence inconnue. Vérifier que le code rend une vue cohérente si une lecture et un événement arrivent à proximité : ne retourner pas un objet interne mutable qu’un autre traitement modifie pendant la sérialisation.

Conserver le même format d’erreur 404 que les autres consultations, avec un code stable et un message adapté aux données fictives. Distinguer 404 (référence absente) et indisponibilité de la lecture (panne réseau ou serveur). Une erreur de consultation ne change jamais le paiement en FAILED.

Réaliser un test avec compteur de l’adaptateur : plusieurs GET ne doivent pas augmenter le nombre de créations. Documenter que les données restent perdues au redémarrage. Le cours ne transforme pas sessionStorage en base métier.

## 3. Mémoriser une intention minimale dans React — 70 min

Créer un module de stockage de session qui encapsule lecture, validation, écriture et suppression d’une seule entrée versionnée, par exemple `pilotcrew.demo.checkout.v1`. Conserver uniquement des références fictives et les données utiles à la reprise : version, orderId, method, idempotencyKey et, une fois reçu, paymentId. Aucun statut terminal, montant faisant autorité ni donnée personnelle n’est requis dans cette entrée.

Juste avant le premier POST du checkout, mémoriser l’intention. Quand la réponse arrive, compléter paymentId. Une erreur d’accès au stockage ne doit pas bloquer l’écran : conserver la tentative en mémoire et expliquer que la reprise après rechargement n’est pas disponible sur ce poste. Ne contourner pas les restrictions du navigateur.

Au montage, lire et vérifier la forme de l’entrée. S’il existe un paymentId valide, afficher un état « Recherche de la tentative » puis consulter son GET et reprendre le polling du lab 04 si nécessaire. S’il existe seulement l’intention, proposer « Retrouver ma tentative » : l’action renvoie le POST avec la commande, la méthode et la clé d’origine. Dans les deux cas, aucune nouvelle clé n’est inventée.

Si rien n’est enregistré, afficher le formulaire normal. Si l’entrée est corrompue, la mettre de côté ou la supprimer de façon contrôlée puis afficher une explication non bloquante. Le code de reprise reste distinct de l’effet qui charge le catalogue, pour éviter qu’un chargement de produits ne déclenche une écriture métier.

## 4. Donner des mots justes aux erreurs — 45 min

Comparer vos messages à ce que le service sait réellement :

| Observation | Message attendu dans son sens | Action proposée |
|---|---|---|
| Lecture PENDING | Tentative retrouvée, résultat en attente | Consulter de nouveau selon le budget |
| Lecture SUCCEEDED | Paiement simulé réussi | Voir la référence, terminer la démo |
| Lecture FAILED | Paiement simulé échoué | Terminer la démo ; nouvelle commande si autre essai |
| Lecture 404 après remise à zéro | Référence absente de ce service de démonstration | Commencer une nouvelle démo explicitement |
| Échec réseau | État non vérifiable pour le moment | Réessayer la consultation |
| Entrée locale invalide | Reprise locale indisponible | Revenir au formulaire sans nouvelle création automatique |

Ajouter « Nouvelle démonstration » : cette action explicite efface uniquement l’entrée locale de ce fil rouge et remet l’interface au point de départ. Elle n’est ni un remboursement ni une annulation. Ne vider pas tout le stockage du navigateur.

Vérifier le parcours au clavier, le focus au changement de vue et les annonces de statut sans répétition excessive. Les boutons décrivent des actions réelles : « Consulter » ou « Retrouver » lorsque l’on reprend, « Créer une commande fictive » quand une nouvelle simulation est demandée.

## 5. Vérifier la tranche et diagnostiquer — 50 min

Écrire les tests des cinq critères d’acceptation. Dans le test de reprise avec paymentId, inspecter les appels et exiger zéro POST de création. Dans celui sans paymentId, vérifier l’en-tête d’idempotence inchangé. Dans celui d’état périmé, faire retourner 404 par le serveur et vérifier qu’aucun faux terminal n’est affiché.

Ajouter au parcours navigateur local : créer une tentative, recharger la page, retrouver la même référence, envoyer l’événement local et observer le terminal. À la demande de l’encadrant, arrêter puis relancer le serveur mémoire et montrer le message de référence absente ; ce test ne prétend pas que le paiement métier a échoué.

Le formateur fournit un incident, par exemple un statut local affiché avant le GET ou une clé régénérée à la reprise. Pendant un temps borné, noter symptôme, hypothèses, observation discriminante et correction. Ne remplacer pas tout le module pour éviter d’expliquer la cause. Rejouer le test qui démontre la réparation.

## 6. Livrer, faire relire et prendre du recul — 35 min

Exécuter le script de vérification sur le commit présenté. Rédiger un court message de revue : problème utilisateur, changement réalisé, comportement obtenu, preuves et limites. La revue porte sur le diff local ou une PR qui existe réellement. Le relecteur pose une question Java et une question React ; noter sa remarque et votre réponse.

Démonstration en cinq minutes : intention, création, reload, même référence, événement, terminal, puis erreur de consultation. Si une preuve n’a pas été exécutée, le dire. Terminer par une minute en anglais qui explique le résultat et une limite technique.

Dans le bilan, choisir deux pratiques acquises et deux besoins de progression. Les six ateliers représentent **27 heures d’entraînement encadré**. Ils ne constituent pas cinq à dix années d’expérience professionnelle et ne promettent pas l’autonomie d’un profil senior. Un portfolio honnête décrit la simulation, les tests réellement obtenus et la capacité à expliquer ses choix.

## Déroulé minuté — 270 minutes

| Plage | Activité | Durée |
|---|---|---:|
| 00:00–00:25 | Clarifier la reprise, écrire les critères et choisir la tranche | 25 min |
| 00:25–01:10 | Vérifier lecture serveur, erreurs et absence de création lors du suivi | 45 min |
| 01:10–02:20 | Implémenter la reprise React et la mémoire minimale de session | 70 min |
| 02:20–03:05 | Soigner messages, clavier et traitement des références périmées | 45 min |
| 03:05–03:55 | Tester les scénarios et diagnostiquer un incident imposé | 50 min |
| 03:55–04:30 | Faire la revue, démontrer la version et rédiger le bilan professionnel | 35 min |

## Contrat commun du fil rouge

La boutique **PilotCrew Démo** ne vend rien. Elle présente le produit `demo-ticket`, libellé « Atelier fictif », à `unitAmountMinor: 2500`, devise `EUR`, `fictional: true`. Une quantité entière de **1 à 5** produit une commande dont le montant est calculé par le serveur. Dans cette simulation EUR seulement, 2500 unités mineures s’affichent 25,00 € fictifs. Aucun autre nombre de décimales ni aucune conversion de devise n’est traité.

| Objet | Champs publics attendus |
|---|---|
| Product | `id`, `label`, `unitAmountMinor`, `currency`, `fictional` |
| Order | `id`, `productId`, `quantity`, `amountMinor`, `currency`, `fictional` |
| Payment | `id`, `orderId`, `amountMinor`, `currency`, `method`, `status`, `fictional` |

Le corps de création de commande accepte uniquement `productId` et `quantity`. Le corps de création de paiement accepte uniquement `method`. Un montant, une devise ou un statut ajoutés par le client ne doivent jamais devenir une autorité : **ce contrat les refuse en 400**. Le code utilise les identifiants retournés, jamais un identifiant d’exemple comme s’il existait déjà.

| Route | Résultat contractuel |
|---|---|
| `GET /api/products` | 200, tableau de produits fictifs |
| `POST /api/orders` | 201, commande créée et en-tête Location |
| `GET /api/orders/{id}` | 200, commande ; 404 si référence absente |
| `POST /api/orders/{id}/payments` | À partir du lab 03 : 202, tentative PENDING ; Idempotency-Key obligatoire |
| `GET /api/payments/{id}` | À partir du lab 03 : 200, état serveur ; 404 si référence absente |
| `POST /api/webhooks/mock-psp` | À partir du lab 04 : réception d’un événement signé par le simulateur local |

Les erreurs structurées emploient `application/problem+json` et au moins `type`, `title`, `status`, `detail`, `code`. `code` est une extension de ce projet. Le texte est destiné à l’utilisateur ; aucune pile d’exception ne sort dans la réponse. Les codes HTTP du cours sont des choix de contrat précis ; ils ne décrivent pas toutes les API de paiement existantes.

Toutes les données métier sont en mémoire dans une JVM : un redémarrage les efface. Cette limite est visible dans la documentation et dans les scénarios. Le cours ne met en place ni identité client, ni base durable, ni gestion de stock, ni remboursement, ni autorisation financière réelle.
