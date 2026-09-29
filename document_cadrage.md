# Document de cadrage — CapGroup Services (Cas C)

## 1. Synthèse exécutive

CapGroup veut libérer la capacité RH aujourd'hui absorbée par le tri manuel, afin que les assistantes puissent traiter les dossiers et éviter l'accumulation des demandes en cas d'absence. Une classification supervisée des tickets est une piste à évaluer, sous réserve de la qualité des catégories et d'un traitement maîtrisé des données. Les objectifs annoncés sont de passer d'environ une heure de tri par jour à dix minutes et d'orienter directement plus de 85 % des tickets vers la bonne équipe. Le garde-fou prioritaire est de ne pas retarder une demande de paie proche de la clôture ni exposer les données personnelles ou de santé. Budget indicatif : 20 000 à 30 000 euros la première année, maintenance comprise.

> **Imprévu client (14h30) — ce que ça change** : aucun imprévu communiqué à ce stade ; à compléter après réception de l'information.

## 2. Besoin métier et contexte

**Demande exprimée :** « automatiser le tri vers les bonnes équipes ». **Besoin réel :** réduire le temps que les deux assistantes consacrent à lire et orienter les courriels, pour qu'elles puissent traiter les dossiers et éviter les accumulations et relances lors d'une absence. Aujourd'hui, les salariés écrivent à `rh@capgroup.fr` ; les assistantes lisent l'objet et le début du message, choisissent une catégorie puis le ticket part dans la file d'une équipe. Le client cite environ 80 tickets par jour en moyenne, jusqu'à 150 en période de pointe, avec des pics pour la mutuelle en janvier, les congés en mai-juin et la paie en fin de mois ; ces volumes restent à rapprocher des quelque 10 000 tickets annoncés sur trois ans. Le tri prend environ une heure chaque matin pour deux personnes ; la cible annoncée est dix minutes et plus de 85 % d'orientation directe correcte. Le budget de première année inclut la maintenance. Une erreur est généralement reprise avec un à deux jours de retard, mais une erreur de paie proche de la clôture peut faire manquer une régularisation. Les tickets peuvent contenir numéro de sécurité sociale, salaire et informations de santé ; confidentialité et traitement humain des cas sensibles sont des contraintes majeures.

## 3. Données — mini-cours `02`

| Donnée | Existante / à acquérir | Volume, qualité estimée | Personnelle ? |
|---|---|---|---|
| Historique des tickets et catégories | Existante dans l'outil | Environ 10 000 sur trois ans selon une réponse ; environ 80/jour et jusqu'à 150 en pointe selon une autre. Chiffres à réconcilier. Catégories attribuées manuellement ; `autre` est fourre-tout et contenait le matériel avant l'ajout de cette catégorie l'an dernier. | Oui, selon le contenu des tickets. |
| Extrait CSV transmis | Existant, 20 lignes | Identifiant, date, catégorie et objet, sans corps. 6 `conges`, 6 `materiel`, 4 `mutuelle`, 3 `autre`, 1 `paie` ; aucun `formation`. Objets très répétitifs ; extrait non démontré représentatif. | Identifiants présents ; anonymisation et possibilité de ré-identification à vérifier. |
| Corps des courriels | Existant dans les tickets, non transmis | Contenu non évalué. Le client indique que des informations personnelles se trouvent parfois dans le corps. Accès et échantillon anonymisé à discuter avant toute analyse. | Oui : numéro de sécurité sociale, salaire et parfois santé (arrêt, hospitalisation). |
| Volumes agrégés par période et catégorie | À extraire de l'outil | Nécessaires pour vérifier moyenne, pics saisonniers, répartition et incohérence des totaux ; catégories historiques à interpréter avec prudence. | À produire sous forme agrégée, sans identifiants directs. |

**Constats sur l'extrait :** les objets sont très répétitifs, `materiel` apparaît malgré son absence de la liste initiale, et `formation` n'apparaît pas dans ces 20 lignes. La représentativité et l'anonymisation ne sont pas établies. Une règle Outlook basée sur « congé » ne triait qu'environ la moitié des courriels et échouait sur des objets génériques ; le seul objet ne suffit donc pas à démontrer un tri fiable.

## 4. Risques et conformité — mini-cours `04` et `07`

**Usage réel** : aujourd'hui, les assistantes choisissent manuellement la catégorie et l'équipe destinataire traite le ticket. À terme, le système pourrait proposer ou appliquer une catégorie ; la DSI indique que l'API du ticketing peut lire les tickets et changer leur catégorie, mais le déclenchement du transfert et le droit de correction humaine restent à confirmer. Aucun traitement de fond ou décision sur un droit RH n'est décrit.

**Qualification AI Act** : qualification provisoire, non haut risque au vu de l'usage décrit. Il s'agit d'orienter des demandes vers un service, non de recruter, évaluer les salariés ou répartir leur travail selon leur comportement. L'usage ne relève pas non plus d'une interaction avec un chatbot ni de génération de contenu nécessitant la transparence de l'article 50. Requalifier si le système sert ensuite à évaluer les salariés, surveiller leur comportement ou prendre des décisions RH substantielles.

**RGPD** : les tickets contiennent des données personnelles ; les informations de santé relèvent aussi de l'article 9, dont une exception applicable doit être identifiée avant leur traitement. Base légale à instruire avec le DPO : intérêt légitime de l'employeur à traiter les demandes internes efficacement, sous réserve de nécessité, mise en balance et information des salariés ; ne pas la tenir pour acquise. Pas de profilage des salariés identifié : la catégorie porte sur la demande. L'article 22 n'est pas présumé applicable au simple routage vers une équipe qui traite ensuite le ticket ; il faut réexaminer les deux conditions cumulatives (décision exclusivement automatisée et effet juridique ou similaire significatif), notamment si un routage automatique peut retarder une régularisation de paie.

| Risque (éthique, métier, conformité) | Niveau | Obligation ou raison | Traitement à prévoir dans l'architecture |
|---|---|---|---|
| Mauvais routage d'une demande de paie avant clôture | 🔴 | Peut faire manquer une régularisation et dégrader la confiance du salarié. | Escalade ou validation humaine pour ce cas ; mesurer les résultats paie séparément. |
| Divulgation de données personnelles ou de santé | 🔴 | Données présentes dans les corps ; exigences RGPD, minimisation et exception art. 9 à déterminer. | Limiter les données traitées, contrôler les accès et éviter l'envoi de corps non protégés à un service externe. |
| Catégories historiques bruitées (`autre`, ancien `materiel`) | 🟠 | Libellés manuels et taxonomie modifiée ; risque de mesurer ou apprendre sur des catégories incohérentes. | Auditer et documenter les libellés, isoler les cas ambigus et suivre les performances par catégorie. |
| Accès ou modification non autorisés via l'API du ticketing | 🔴 | L'API pourrait lire les tickets et changer leur catégorie ; droits et sécurité inconnus. | Authentification, droits minimaux, journalisation et validation des écritures à définir. |
| Routage automatique difficile à contester ou corriger | 🟠 | Le processus de validation par les équipes destinataires n'est pas clarifié. | Prévoir une correction humaine traçable et une reprise des erreurs. |

**Sécurité du modèle** — menaces plausibles à confirmer selon l'exposition retenue :

| Menace | Plausibilité sur ce cas | Mitigation proposée | Risque résiduel |
|---|---|---|---|
| Texte de ticket trompeur ou ambigu faisant dévier le classement | 🟠 Les expéditeurs contrôlent l'objet et le contenu ; les objets génériques ont déjà fait échouer les règles Outlook. | Normaliser les entrées, mesurer la confiance, envoyer les cas incertains à une personne et suivre les erreurs. | Un objet inédit ou volontairement trompeur peut encore être mal classé. |
| Fuite de données par l'accès au modèle, ses journaux ou ses données d'apprentissage | 🔴 Les corps peuvent contenir paie, identifiant national et santé ; aucun fournisseur de modèle ni contrôle de conservation n'est encore défini. | Ne pas transmettre de corps avant validation DPO/sécurité ; minimiser ou masquer les données, limiter les droits et contrôler la conservation. | Des données sensibles peuvent échapper au masquage ou rester dans des journaux fournisseur. |

Prompt injection et extraction de modèle ne sont pas retenues à ce stade : aucun LLM/RAG ni service d'inférence exposé n'est défini. Le poisoning n'est pas retenu pour un apprentissage hors ligne sans boucle de réentraînement confirmée ; réévaluer si les corrections alimentent automatiquement les données d'entraînement.

## 5. Architecture cible et sobriété — mini-cours `05`

**Schéma de composants** : voir [schema_archi_cible.md](schema_archi_cible.md). Piste à valider : classifieur de texte supervisé, entraîné uniquement après audit et harmonisation des catégories historiques. Commencer en mode observation, sans écriture dans le ticketing ; activer le routage automatique uniquement après test concluant et validation de l'API. Les cas incertains passent en revue humaine ; les tickets de paie proches de la clôture sont prioritaires, avec un délai à convenir.

**Sobriété — LLM refusé en première version.** Une classification de catégories finies ne justifie pas la génération de texte. On écarte RAG, base vectorielle et agent, qui ajouteraient exposition et complexité sans besoin établi. Réexaminer seulement si un classifieur supervisé évalué ne satisfait pas les KPI.

## 6. Indicateurs, seuils, questions ouvertes — mini-cours `03`

| Indicateur | Cible | Seuil d'acceptabilité | Comment on le mesure |
|---|---|---|---|
| Temps quotidien de tri manuel | Passer d'environ 1 h à 10 min/jour (cible client) | ≤ 10 min/jour ; confirmer si l'heure de départ est cumulée pour les deux personnes | Chronométrage du temps réellement consacré au tri, même période avant/après et volumes comparables. |
| Routage correct au premier envoi | > 85 % des tickets dans la bonne équipe (cible client) | > 85 % ; valider la mesure par catégorie, notamment `paie` et `materiel` | Tickets dirigés correctement au premier envoi / tickets évalués ; contrôle humain d'un échantillon représentatif. |
| Régularisations de paie manquées à cause du routage | 0 cas (garde-fou proposé, à valider avec RH) | Aucun cas acceptable ; délai maximal de routage avant clôture à définir | Revue des tickets de paie proches de la clôture et des retards/erreurs associés avec l'équipe paie. |

**Prochaines étapes** : 1) réconcilier les volumes annoncés (10 000 sur trois ans, 80/jour et pics à 150) et auditer la taxonomie `autre`/`materiel` ; 2) tester en mode observation sur des tickets représentatifs, anonymisés et relus humainement, sans écrire dans le ticketing ; 3) faire valider par DSI/DPO les droits API, le périmètre de texte accessible et le parcours de revue humaine avant tout pilote actif.

**Question ouverte (notes d'entretien §3)** : Quel délai maximal d'orientation resterait acceptable pour un ticket de paie reçu le 24 du mois, même si le tri automatisé réduit le temps global ?