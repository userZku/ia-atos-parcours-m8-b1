# Document de cadrage — CapGroup Services (Cas C)

## 1. Synthèse exécutive

CapGroup veut libérer la capacité RH aujourd'hui absorbée par le tri manuel, afin que les assistantes puissent traiter les dossiers et éviter l'accumulation des demandes en cas d'absence. Un classifieur supervisé des seuls objets est une piste à tester, sans faisabilité présumée : l'ancien essai de règles par mot-clé ne couvrait qu'environ la moitié des mails. Le corps est exclu de toute analyse automatique et doit rester dans le ticketing. Le test devra vérifier le seuil client de plus de 85 % de routage correct avant tout pilote ; la paie proche de la clôture reste un garde-fou humain. Budget indicatif : 20 000 à 30 000 euros la première année, maintenance comprise.

> **Imprévu client (14h30) — ce que ça change** : la DPO autorise l'analyse automatique de l'objet seulement ; le corps ne doit pas sortir du ticketing. Les sections 3 à 6 et le schéma sont ajustés : le test de faisabilité et les KPI portent sur l'objet seul, avec revue humaine dans le ticketing pour les cas incertains.

## 2. Besoin métier et contexte

**Demande exprimée :** « automatiser le tri vers les bonnes équipes ». **Besoin réel :** réduire le temps que les deux assistantes consacrent à lire et orienter les courriels, pour qu'elles puissent traiter les dossiers et éviter les accumulations et relances lors d'une absence. Aujourd'hui, les salariés écrivent à `rh@capgroup.fr` ; les assistantes lisent l'objet et le début du message, choisissent une catégorie puis le ticket part dans la file d'une équipe. La DPO limite toute analyse automatique à l'objet ; le corps reste dans le ticketing. Cette contrainte rend le taux de routage visé (>85 %) incertain, d'autant que l'ancien essai de règles par mot-clé dans l'objet ne réussissait qu'environ la moitié du temps. Les volumes annoncés sont environ 80 tickets/jour, jusqu'à 150 en pointe, à réconcilier avec les quelque 10 000 tickets sur trois ans. La cible de temps est dix minutes par jour ; le budget indicatif de première année, maintenance comprise, est de 20 000 à 30 000 euros. Une erreur de paie proche de la clôture peut faire manquer une régularisation : ce cas nécessite un garde-fou humain.

## 3. Données — mini-cours `02`

| Donnée | Existante / à acquérir | Volume, qualité estimée | Personnelle ? |
|---|---|---|---|
| Historique des tickets et catégories | Existante dans l'outil | Environ 10 000 sur trois ans selon une réponse ; environ 80/jour et jusqu'à 150 en pointe selon une autre. Chiffres à réconcilier. Catégories attribuées manuellement ; `autre` est fourre-tout et contenait le matériel avant l'ajout de cette catégorie l'an dernier. | Oui, selon le contenu des tickets. |
| Extrait CSV transmis | Existant, 20 lignes | Identifiant, date, catégorie et objet, sans corps. 6 `conges`, 6 `materiel`, 4 `mutuelle`, 3 `autre`, 1 `paie` ; aucun `formation`. Objets très répétitifs ; extrait non démontré représentatif. | Identifiants présents ; anonymisation et possibilité de ré-identification à vérifier. |
| Corps des courriels | Existant dans les tickets ; exclu de l'analyse automatique | La DPO interdit qu'il sorte du ticketing ; aucune extraction, analyse ou journalisation par le classifieur. Il reste consultable dans l'outil par les personnes habilitées selon les règles internes. | Oui : numéro de sécurité sociale, salaire et parfois santé (arrêt, hospitalisation). |
| Volumes agrégés par période et catégorie | À extraire de l'outil | Nécessaires pour vérifier moyenne, pics saisonniers, répartition et incohérence des totaux ; catégories historiques à interpréter avec prudence. | À produire sous forme agrégée, sans identifiants directs. |

**Constats sur l'extrait :** les objets sont très répétitifs, `materiel` apparaît malgré son absence de la liste initiale, et `formation` n'apparaît pas dans ces 20 lignes. La représentativité et l'anonymisation ne sont pas établies. L'essai Outlook sur « congé » réussissait environ une fois sur deux et échouait sur des objets génériques. Ce résultat ne préjuge pas d'un classifieur supervisé, mais impose un test hors production, sur objets uniquement, avec catégories de référence et résultats par catégorie. Si le seuil de plus de 85 % n'est pas atteint, ne pas activer le routage automatique.

## 4. Risques et conformité — mini-cours `04` et `07`

**Usage réel** : aujourd'hui, les assistantes choisissent manuellement la catégorie et l'équipe destinataire traite le ticket. À terme, le système pourrait proposer ou appliquer une catégorie à partir du seul objet. La DPO interdit l'analyse automatique du corps et son transfert hors du ticketing ; le système ne doit donc ni le récupérer pour le modèle, ni l'enregistrer dans ses journaux. La DSI signale une API de lecture et de modification de catégorie ; les droits limités aux champs autorisés, le transfert déclenché et la correction humaine restent à confirmer. Aucun traitement de fond ou décision sur un droit RH n'est décrit.

**Qualification AI Act** : qualification provisoire, non haut risque au vu de l'usage décrit. Il s'agit d'orienter des demandes vers un service, non de recruter, évaluer les salariés ou répartir leur travail selon leur comportement. L'usage ne relève pas non plus d'une interaction avec un chatbot ni de génération de contenu nécessitant la transparence de l'article 50. Requalifier si le système sert ensuite à évaluer les salariés, surveiller leur comportement ou prendre des décisions RH substantielles.

**RGPD** : les tickets contiennent des données personnelles ; les informations de santé relèvent aussi de l'article 9, dont une exception applicable doit être identifiée avant leur traitement. La DPO fixe une limite de traitement : seul l'objet peut être analysé automatiquement et le corps reste dans le ticketing ; vérifier que l'objet lui-même ne révèle pas de données sensibles et appliquer la minimisation. Base légale à instruire avec le DPO : intérêt légitime de l'employeur à traiter les demandes internes efficacement, sous réserve de nécessité, mise en balance et information des salariés ; ne pas la tenir pour acquise. Pas de profilage des salariés identifié : la catégorie porte sur la demande. L'article 22 n'est pas présumé applicable au simple routage vers une équipe qui traite ensuite le ticket ; réexaminer les deux conditions cumulatives si l'automatisation produit une décision exclusivement automatisée à effet juridique ou similaire significatif, notamment en cas de retard de paie.

| Risque (éthique, métier, conformité) | Niveau | Obligation ou raison | Traitement à prévoir dans l'architecture |
|---|---|---|---|
| Mauvais routage d'une demande de paie avant clôture | 🔴 | Peut faire manquer une régularisation et dégrader la confiance du salarié. | Escalade ou validation humaine pour ce cas ; mesurer les résultats paie séparément. |
| Analyse ou sortie accidentelle du corps du ticket | 🔴 | La DPO autorise l'analyse automatique de l'objet uniquement ; le corps contient santé et numéros de sécurité sociale et ne doit pas sortir du ticketing. | N'exposer au modèle que le champ objet via liste blanche ; exclure le corps des payloads, journaux et jeux d'entraînement ; tester la non-transmission ; arrêter le pilote si l'API ne garantit pas cette séparation. |
| Catégories historiques bruitées (`autre`, ancien `materiel`) | 🟠 | Libellés manuels et taxonomie modifiée ; risque de mesurer ou apprendre sur des catégories incohérentes. | Auditer et documenter les libellés, isoler les cas ambigus et suivre les performances par catégorie. |
| Accès ou modification non autorisés via l'API du ticketing | 🔴 | L'API pourrait lire les tickets et changer leur catégorie ; droits et sécurité inconnus. | Authentification, droits minimaux, journalisation et validation des écritures à définir. |
| Routage automatique difficile à contester ou corriger | 🟠 | Le processus de validation par les équipes destinataires n'est pas clarifié. | Prévoir une correction humaine traçable et une reprise des erreurs. |

**Sécurité du modèle** — menaces plausibles à confirmer selon l'exposition retenue :

| Menace | Plausibilité sur ce cas | Mitigation proposée | Risque résiduel |
|---|---|---|---|
| Objet générique ou formulé pour tromper le classement | 🟠 Les expéditeurs contrôlent l'objet ; les règles Outlook fondées sur « congé » échouaient sur « petite question ». | Évaluer les objets seuls, calibrer un seuil de confiance et envoyer les cas incertains en revue humaine dans le ticketing. | Une formulation ambiguë peut rester impossible à classer automatiquement. |
| Corps transmis ou journalisé par erreur via l'API | 🔴 L'API peut lire des tickets ; son périmètre de champs n'est pas confirmé et le corps est interdit hors du ticketing. | Liste blanche de l'objet, compte API à privilèges minimaux, journaux sans texte, test de payload et blocage si le corps est présent. | Défaut de configuration ou comportement inattendu du fournisseur ; audit périodique nécessaire. |

Prompt injection et extraction de modèle ne sont pas retenues à ce stade : aucun LLM/RAG ni service d'inférence exposé n'est défini. Le poisoning n'est pas retenu pour un apprentissage hors ligne sans boucle de réentraînement confirmée ; réévaluer si les corrections alimentent automatiquement les données d'entraînement.

## 5. Architecture cible et sobriété — mini-cours `05`

**Schéma de composants** : voir [schema_archi_cible.md](schema_archi_cible.md). Piste à valider : classifieur supervisé recevant uniquement l'objet, avec catégories historiques auditées. Commencer en mode observation, sans écriture ; mesurer le résultat sur un jeu de test indépendant. L'ancien essai Outlook à environ 50 % ne valide pas cette approche : le routage automatique n'est envisagé que si le seuil client de plus de 85 % est atteint sur objets seuls et si les droits API garantissent l'exclusion du corps. Les cas incertains et la paie proche de la clôture passent en revue humaine dans le ticketing.

**Sobriété — LLM refusé en première version.** Une classification de catégories finies ne justifie pas la génération de texte. On écarte RAG, base vectorielle et agent, qui ajouteraient exposition et complexité sans besoin établi. Réexaminer seulement si un classifieur supervisé évalué ne satisfait pas les KPI.

## 6. Indicateurs, seuils, questions ouvertes — mini-cours `03`

| Indicateur | Cible | Seuil d'acceptabilité | Comment on le mesure |
|---|---|---|---|
| Temps quotidien de tri manuel | Passer d'environ 1 h à 10 min/jour (cible client) | ≤ 10 min/jour ; confirmer si l'heure de départ est cumulée pour les deux personnes | Chronométrage du temps réellement consacré au tri, même période avant/après et volumes comparables. |
| Routage correct au premier envoi depuis l'objet seul | > 85 % des tickets dans la bonne équipe (cible client) | > 85 % sur tous les tickets du jeu de test ; aucun lancement automatique si le test hors production échoue | Comparer la catégorie proposée depuis l'objet à une référence vérifiée ; publier le score global et par catégorie, ainsi que la part envoyée en revue humaine. |
| Régularisations de paie manquées à cause du routage | 0 cas (garde-fou proposé, à valider avec RH) | Aucun cas acceptable ; délai maximal de routage avant clôture à définir | Revue des tickets de paie proches de la clôture et des retards/erreurs associés avec l'équipe paie. |

**Prochaines étapes** : 1) faire valider par la DSI/DPO une extraction strictement limitée à l'objet et vérifier l'absence du corps dans payloads, journaux et données d'entraînement ; 2) réconcilier les volumes, auditer `autre`/`materiel` et constituer un jeu de test représentatif avec catégories vérifiées ; 3) tester en mode observation sur objets seuls, mesurer le >85 % global et par catégorie, la couverture et les cas de paie ; n'activer l'écriture qu'après validation humaine et respect des seuils.

**Question ouverte (notes d'entretien §3)** : Quel délai maximal d'orientation resterait acceptable pour un ticket de paie reçu le 24 du mois, même si le tri automatisé réduit le temps global ?

**Question DSI/DPO** : Comment garantir techniquement que le traitement automatique ne reçoit et ne journalise que l'objet, sans récupérer le corps du message ?