
***********************************************************************************************************************************************************************
								1. Contexte bancaire
***********************************************************************************************************************************************************************

	Le projet est réalisé avec Power BI dans une logique de Business Intelligence et d'aide à la décision
	L'octroi de crédit est une activité à fort enjeu pour les institutions financières : il faut développer le portefeuille tout en maîtrisant le risque de défaut. 
	Les données historiques de demandes de crédit permettent de comprendre la structure de la demande, les profils des demandeurs et les logiques d'octroi.
	Ce projet s'inscrit dans une démarche Business Intelligence & Data Analytics avec Power BI. Il vise à transformer des données brutes en insights actionnables pour la décision.
	Complémentarité avec le projet Machine Learning.

	Ce projet utilise le même jeu de données qu'un projet de ML consacré à la prédiction d'octroi. Les deux projets sont complémentaires mais répondent à des questions différentes :
		- Machine Learning : Peut-on prédire la décision d'octroi pour une nouvelle demande ?
		- Business Intelligence : Que révèlent les données historiques sur les structures de décision, et quelles associations peut-on mettre en évidence entre les caractéristiques des demandeurs et la décision finale ?
	Le projet BI ne cherche pas à reproduire le modèle prédictif. Il apporte une vision diagnostique et exploratoire pour comprendre le passé et éclairer les décisions futures.

*********************************************************************************************************************************************************************
								2. Problématique Métier
********************************************************************************************************************************************************************************
	L'enjeu n'est pas seulement de compter les accords et les refus. Il s'agit de comprendre quelles caractéristiques des demandes sont associées aux décisions observées, 
	et si ces associations sont structurelles ou dues à des cas isolés.
	La Problématique centrale : Comment exploiter les données historiques de demandes de crédit pour identifier des structures, 
				des écarts et des associations pertinentes entre les caractéristiques des demandeurs, leurs capacités financières et les décisions d'octroi ?

*************************************************************************************************************************************************************************
								3. Objectifs Data Analytics
*****************************************************************************************************************************************************************************
	les objectifs que visent ce travail sont:

		1.Caractériser la population des demandeurs.
		2.Mesurer la structure globale des décisions d'octroi.
		3.Étudier l'association entre l'historique de crédit et la décision.
		4.Comparer les caractéristiques financières (revenus, montants, ratios) entre accords et refus.
		5.Analyser la dispersion (variance, écart-type) pour détecter des profils à risque.
		6.Rechercher des effets de segmentation (croisement de variables).
		7.Transformer les résultats statistiques en recommandations métier.

************************************************************************************************************************************************************************
								4. Approche Analytique
***********************************************************************************************************************************************************************
		Le projet suit une démarche structurée :
		Données brutes >> Audit >> Nettoyage >> Feature Engineering >> Modélisation dimensionnelle >> DAX >> Analyse statistique >> Visualisation >> Interprétation >> Recommandations.
		Le dashboard est l'aboutissement de l'analyse, pas le point de départ. Chaque graphique répond à une question analytique précise.

************************************************************************************************************************************************************************
								5. Hypothèses Analytiques
***************************************************************************************************************************************************************************
		Les hypotheses sont :
			
			---> H1 - Profil : La structure des décisions varie selon les caractéristiques sociodémographiques (Genre, Situation familiale, Éducation, Statut professionnel, Zone).
			--> H2 - Historique de crédit : Les demandes avec un historique de crédit renseigné favorablement présentent un taux d'octroi significativement différent.\
			--> H3 - Capacité financière : Les accords et refus présentent des distributions de revenus et de montants demandés différentes.
			--> H4 - Poids de la demande : Le ratio Montant demandé / Revenu total est un indicateur discriminant de la décision.
			--> H5 - Stabilité des revenus : La dispersion (variance) des revenus est plus élevée chez les dossiers refusés que chez les dossiers acceptés.

*********************************************************************************************************************************************************************
								6. Dataset & Audit des Données
*********************************************************************************************************************************************************************
		Notre audit presente les informations issues de notre dataset :
			- Source : dbcredit.csv (614 demandes, 13 variables)
			- Variable cible : Loan_Status (Y = Accord, N = Refus).
			- Valeurs manquantes identifiées : Gender (13), Married (3), Dependents (15), Self_Employed (32), LoanAmount (22), Loan_Amount_Term (14), Credit_History (50).

		Traitement des manquants
			- Catégorielles : Remplacées par la modalité "Non renseigné". Cela évite de transformer une information inconnue en information négative 
			- Numériques (LoanAmount, Loan_Amount_Term) : Conservées en null. Pas de remplacement arbitraire par 0.
			- CoapplicantIncome = 0 : Interprété comme l'absence de co-demandeur, pas comme une anomalie.

************************************************************************************************************************************************************************
								7. Feature Engineering Analytique
************************************************************************************************************************************************************************

	Deux variables dérivées sont créées pour enrichir l'analyse :

			- RevenuTotal = ApplicantIncome + CoapplicantIncome. Représente la capacité financière brute du ménage.
			- LoanToIncomeRatio = LoanAmount / Revenu_total : représente le poids du montant demandé relativement au revenu. 
								Note : Ce n'est pas un DTI complet, faute de données sur les charges.

************************************************************************************************************************************************************************
								8. Modélisation de Données (Schéma en Étoile)
**************************************************************************************************************************************************************************
	
	Bien qu'un modèle plat soit suffisant, un modèle dimensionnel a été choisi pour démontrer une bonne pratique analytique.

			- Fact_Demande (Table de faits) : Loan_ID, ApplicantIncome, CoapplicantIncome, Revenu_total, LoanAmount, LoanToIncome_ratio, statut_pret, clés étrangères (ID_Profil, ID_Credit, ID_Zone).
			- Dim_Profil : genre, married, niveau_charges (Aucune charge, Faible, Moyenne, Forte, Non renseigné), Education, Self_Employed.
			- Dim_Credit : Loan_Amount_Term, Credit_History.
			- Dim_Zone : Property_Area.


9. Mesures DAX Principales
•	Volume : Nombre Demandes, Nombre Accords, Nombre Refus.
•	Taux : Taux d'octroi = DIVIDE([Nombre Accords], [Nombre Demandes]).
•	Valeur : Montant total demandé, Montant moyen demandé, Revenu moyen.
Statistiques avancées (le cœur de l'analyse)
•	Revenu médian (plus robuste que la moyenne face aux valeurs extrêmes).
•	Écart-type Revenu (mesure la dispersion/stabilité des revenus).
•	Variance Revenu (pour appuyer l'hypothèse H5).

****************************************************************************************************************************************************************************************************
								9. Architecture Analytique du Dashboard (5 Pages)
***************************************************************************************************************************************************************************************************
		Nous allons concevoir 05 pages pour ce travail qui constitueront l'ensemble de notre travail analytique, nous repondrons a certaines questions bien precises:

			Page 1 : Vue d'ensemble des demandes
				Question : Quelle est la structure globale des demandes et des décisions ?
				Visuels a produire pour la page: Cartes KPI (Volume, Taux d'octroi, Nombre de demades, Nombres de refus), Répartition géographique (Carte), 
							Répartition selon les attributs du profil consigné en Dim

			Page 2 :  Historique de crédit et décision
				Question : Quelle association observe-t-on entre l'historique et la décision ?
				Visuels : Matrice (Lignes : Credit_History, Colonnes : Décision, Valeurs : Taux d'octroi). Mise en évidence du segment "Non renseigné".
			
			Page 3 — Capacité financière et dispersion
				Question : Comment le revenu et le montant demandé sont-ils associés à la décision ?
				Visuels : Boîtes à moustaches (Box Plots) pour montrer la distribution des revenus par décision (met en évidence la différence de dispersion). 
					Nuage de points (Scatter Plot) pour la relation Revenu_total vs LoanAmount avec lignes de tendance
				Analyse du LoanToIncomeRatio par décision

			Page 4 — Profil et segmentation croisée
				Question : Quels profils présentent des structures de décision différentes ?
				Visuels : Arbre de décomposition (Decomposition Tree) pour explorer le taux d'octroi par croisement (ex : Historique → Zone → Éducation). 		
					Matrice de corrélation visuelle.

			Page 5 — Synthèse décisionnelle
				Question : Quels enseignements opérationnels en tirer ?
				Contenu : Pas de graphiques complexes. Des zones de texte structurées avec :
						- Les 3 principaux constats statistiques.
						- Les limites de l'analyse.
						- Les recommandations stratégiques concrètes.

*********************************************************************************************************************************************************************************
							10. Méthodologie d'Interprétation
********************************************************************************************************************************************************************************

		Chaque résultat est interprété selon 4 niveaux pour éviter les conclusions hâtives :
	
		1. Observation : Que montrent les données ?
		2. Comparaison : Quel écart entre les groupes ?
		3. Interprétation : Quelle hypothèse métier peut l'expliquer ?
		4. Limite : Qu'est-ce que les données ne permettent pas de conclure ? (Corrélation n'est pas causalité).

*******************************************************************************************************************************************************************************
							11. Limites Analytiques
*******************************************************************************************************************************************************************************
		Nous avons plusieurs limites notamment : 

			- Taille du dataset : Le dataset contient seulement 614 demandes. Les résultats doivent donc être interprétés avec prudence.

			- Absence d'identifiant client : Loan_ID identifie une demande et non nécessairement un client. 
						Une analyse de fidélité ou de comportement longitudinal n'est donc pas possible.

			- Absence de dimension temporelle : Le dataset ne contient pas de date. Il n'est donc pas pertinent de construire artificiellement une évolution mensuelle, 
						annuelle ou une tendance temporelle. c'est impossible d'analyser les tendances, saisonnalités ou évolutions.

			- Absence de données post-octroi : Pas de remboursements, retards ou défauts. On analyse la décision d'octroi, pas la performance du crédit.

			- Revenu # Capacité de remboursement : Le dataset ne contient pas les charges, dettes ou patrimoines.

			- Taille de l'échantillon : 614 demandes. Les segmentations fines (ex : Femme, Mariée, 3+ charges, Freelance) peuvent produire de très petits groupes, limitant la significativité statistique.

*******************************************************************************************************************************************************************************
							12. Recommandations Stratégiques (Issues de l'Analyse)
******************************************************************************************************************************************************************************
		Les recommandations suivantes sont issues de notre travail d'analyses :
			- Créer un "Score de Stabilité" : Puisque la variance des revenus est très différente entre accords et refus, intégrer un indicateur de régularité des revenus dans le scoring.
			- Traitement spécifique des "Non renseignés" : Si le taux d'octroi pour Credit_History = Non renseigné est très faible, envisager des produits alternatifs (micro-crédit avec garanties) plutôt qu'un refus systématique.
			- Analyse du ratio d'endettement : Utiliser le LoanToIncomeRatio comme seuil d'alerte pour demander des garanties supplémentaires.

*************************************************************************************************************************************************************************************
							13. Organisation du Repository
*************************************************************************************************************************************************************************************
Nom du dossier principal : powerbi-credit-analysis/

	racine principale : README.md
	dossier : data/ - dbcredit.csv
	dossier : powerbi/ credit_analysis.pbix
	dossier : screenshots/ overview.png
			credit_history.png
			financial_analysis.png
			segmentation.png
			synthesis.png
	dossier : docs/ methodology.md (Optionnel)


*********************************************************************************************************************************************************************
							14. Technologies utilisées
*********************************************************************************************************************************************************************
	nous avons  utilisée pour ce projet Power BI :
		
		- Power BI Desktop
		- Power Query
		- DAX
		- Git
		- GitHub

	EN conclusion:
		Ce projet met en œuvre une démarche de Business Intelligence appliquée à l'analyse des demandes de crédit.
		L'objectif n'est pas de remplacer le Machine Learning, mais de démontrer la complémentarité entre deux approches :
		« Le Machine Learning cherche à prédire une nouvelle décision ; Power BI cherche à comprendre les décisions observées. »
