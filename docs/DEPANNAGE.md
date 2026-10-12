# Dépannage — jr-06-product

Relever la première erreur, la commande, la version et le contexte. Une correction conserve le comportement demandé et ajoute ou rejoue une preuve ; retirer une assertion ne répare pas le problème.

| Symptôme | Cause plausible | Diagnostic discriminant | Résolution attendue |
|---|---|---|---|
| Le rechargement affiche immédiatement SUCCEEDED | L’écran fait confiance au statut mémorisé | Modifier l’entrée locale puis observer les requêtes au montage | Ne conserver qu’une piste de reprise et attendre le statut du GET. |
| Le reload crée une autre tentative | L’effet relance le checkout avec une nouvelle clé | Comparer les POST et en-têtes avant/après rechargement | Avec paymentId utiliser GET ; sans id reprendre la même intention sur action explicite. |
| Une référence absente est affichée FAILED | Le frontend traduit toute erreur en terminal | Faire renvoyer 404 puis une panne réseau dans deux tests | Séparer absence et indisponibilité du statut métier FAILED. |
| Une entrée session illisible bloque toute la page | JSON.parse ou l’accès au stockage n’est pas protégé | Insérer une chaîne invalide et recharger | Traiter lecture/validation/erreur dans le module de stockage et garder une issue utilisable. |
| Nouvelle démo efface des données sans rapport | Le code vide tout sessionStorage | Inspecter l’action et les clés restantes | Supprimer uniquement la clé du cursus après action explicite. |
| Le GET change le nombre d’appels PSP | La consultation réutilise une méthode de création | Comparer compteur avant et après plusieurs lectures | Séparer lecture de référence et création d’intention. |
| La démonstration dépend d’un ancien serveur | La preuve ne correspond pas au commit présenté | Identifier processus, build et version des rapports | Redémarrer sa version construite, rejouer les vérifications et indiquer toute limite. |

Si un port est occupé, identifier le processus avant de l’arrêter. Ne jamais arrêter globalement toutes les JVM ou tous les processus Node d’un poste partagé. Si une commande dépend d’un fichier demandé dans l’atelier, le construire d’abord : les dépôts élèves sont des supports, pas des solutions prêtes à lancer.
