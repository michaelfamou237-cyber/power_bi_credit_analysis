
*******************************************************************************************************************************************
		Méthodologie analytique
********************************************************************************************************************************************

	Ce document détaille la démarche suivie pour transformer les données brutes en recommandations stratégiques.

	1. Audit des données: 

		* Volume : 614 demandes de crédit, 13 variables initiales.
		* Valeurs manquantes : Identifiées sur Gender (13), Married (3), Dependents (15), Self_Employed (32), LoanAmount (22), Loan_Amount_Term (14), Credit_History (50).
		* Doublons : Aucun doublon complet identifié. Loan_ID est unique.

	2. Principes de nettoyage (Power Query) : 

		* Variables catégorielles : Les valeurs manquantes ont été remplacées par la modalité « Non renseigné ». Ce choix méthodologique évite de transformer une information inconnue en information négative (ex : Credit_History = Non renseigné n’est pas équivalent à Credit_History = Mauvais).
		* Variables numériques : LoanAmount et Loan_Amount_Term ont été conservées en null (pas de remplacement arbitraire par 0).
		* CoapplicantIncome = 0 : Interprété comme l’absence de co-demandeur, pas comme une anomalie.
		* Valeurs extrêmes : Conservées, car elles représentent des situations réelles (hauts revenus).

	3. Feature Engineering:
	Nous avons crée de nouvelles features :

		* Création de RevenuTotal pour mesurer la capacité financière globale.
		* Création de LoanToIncomeRatio pour mesurer le poids relatif de la demande.
		* Catégorisation de Credit_History en 3 classes pour affiner l’analyse (ayant, pas, non renseigné).

	4. Modélisation dimensionnelle (schéma en étoile) : 

	Bien qu’un modèle plat soit suffisant pour 614 lignes, un modèle en étoile a été adopté pour démontrer une bonne pratique analytique :

		* Fact_Demande : Contient les mesures quantitatives et la décision.
		* Dim_Profil : Caractéristiques démographiques (Genre, Éducation, Situation familiale…).
		* Dim_Credit : Caractéristiques du prêt (Durée, Historique).
		* Dim_Zone : Localisation géographique.

	5. Approche statistique : 

		* Utilisation de mesures DAX descriptives (Moyenne, Médiane, Écart-type) et non plus seulement des sommes.
		* Utilisation de Box Plots pour visualiser la dispersion des revenus.
		* Analyse croisée via Arbre de Décomposition pour identifier les segments.

	6. Limites de l’analyse : 
	
		* Absence de dimension temporelle : Impossible d’analyser l’évolution mensuelle ou annuelle.
		* Absence de données post-octroi : Pas de suivi des remboursements, donc pas d’analyse de la performance réelle des prêts.
		* Taille de l’échantillon : 614 lignes. Les segmentations très fines peuvent produire des groupes à faible effectif.
		* Corrélation n’est pas causalité : Les associations observées (ex : historique --->  décision) ne sont pas des preuves de causalité.


