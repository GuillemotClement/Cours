Recap code HTTP
| Code | Message | Cas |
| --- | --- | ---|
| 200 | OK | Requête réussie (lecture, modification, suppression, etc.) |
| 201 | CREATED | Ressource créée avec succès |
| 400 | BAD REQUEST | Données invalides ou mal formées (ex: champs manquants, format incorrect) |
| 401 | UNAUTHORIZED | L'utilisateur n'est pas authentifié (mauvais identifiants, token manquant ou invalide) |
| 402 | PAYMENT REQUIRED | Paiement echouer |
| 403 | FORBIDDEN | L'utilisateur est authentifié mais n'a pas les droits nécessaires |
| 404 | NOT FOUND | Ressource demandée introuvable |
| 409 | CONFLICT | Conflit lors de la création (ex: email déjà utilisé) |
| 500 | INTERNAL SERVER ERROR | Erreur interne inattendue côté serveur |
| 501 | NOT IMPLEMENTED | Fonctionnalite non supportee ou pas encore dev |
