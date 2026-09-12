
***********************************************************************************************************************************************************************
								1. Contexte bancaire
***********************************************************************************************************************************************************************

L'octroi de crédit constitue une activité centrale pour les établissements financiers. La banque doit être capable d'évaluer les demandes reçues, de comprendre les caractéristiques des demandeurs 
et d'analyser les décisions d'octroi afin d'améliorer le pilotage de son processus de crédit.
Dans ce projet, nous exploitons un historique de 614 demandes de crédit contenant des informations relatives :
	- Au profil du demandeur
	- À sa situation familiale et professionnelle
	- À ses revenus
	- Au montant demandé
	- Aux caractéristiques du crédit
	- À son historique de crédit
	- À sa zone de résidence
	- Ainsi qu'à la décision finale d'octroi

Le projet est réalisé avec Power BI dans une logique de Business Intelligence et d'aide à la décision

***********************************************************************************************************************************************************************
								2. Problématique métier
***********************************************************************************************************************************************************************
« Comment l'analyse des demandes de crédit et des décisions d'octroi peut-elle fournir aux décideurs une meilleure compréhension des profils des demandeurs, des caractéristiques financières 
et des facteurs associés aux décisions, afin d'appuyer le pilotage du processus d'octroi ? »

L'objectif n'est pas de déterminer une relation causale entre une caractéristique et l'octroi d'un crédit.
Il s'agit plutôt d'identifier et de visualiser les associations observées dans les données historiques, afin de fournir aux décideurs une information structurée et exploitable.

************************************************************************************************************************************************************************
								3. Objectif Data / Business Intelligence
************************************************************************************************************************************************************************
L'objectif de ce projet est de construire une solution analytique permettant de :
		- Structurer les données de demandes de crédit
		- Analyser la population des demandeurs
		- Suivre les décisions d'octroi
		- Comparer les taux d'octroi selon différents profils
		- Analyser les caractéristiques financières des demandes
		- Étudier l'association entre l'historique de crédit et la décision
		- Explorer les différences observées entre certains groupes
		- Produire des KPI permettant de faciliter l'interprétation des données
		- Fournir un tableau de bord interactif destiné à l'aide à la décision

**********************************************************************************************************************************************************************
								4. Questions métier
**********************************************************************************************************************************************************************
	Le tableau de bord doit répondre à cinq questions principales.
	Question 1 — Vue d'ensemble : « Quelle est la structure globale des demandes et des décisions d'octroi ? »
		Analyse :
		- Nombre de demandes
		- Nombre d'accords
		- Nombre de refus
		- Taux d'octroi
		- Montant moyen demandé
		- Revenu moyen
		- Répartition des demandes selon les profils et les zones

	Question 2 — Historique de crédit 
				« Quelle association observe-t-on entre l'historique de crédit renseigné et la décision d'octroi ? »
	Analyse des catégories :
		- Historique favorable
		- Historique défavorable
		- Historique non renseigné
		L'analyse permettra notamment d'observer si les taux d'octroi diffèrent selon l'information de crédit disponible.

	Question 3 — Capacité financière
				« Comment le revenu et le montant demandé sont-ils associés à la décision d'octroi ? »
	Les principales variables étudiées seront :
		- Revenu du demandeur
		- Revenu du co-demandeur
		- Revenu total
		- Montant demandé
		- Ratio montant demandé / revenu total
	Le ratio utilisé dans ce projet est : LoanToIncomeRatio = LoanAmount / RevenuTotal


**********************************************************************************************************************************************************************
			Question 4 — Équité / Fairness
**********************************************************************************************************************************************************************
	
	Question centrale : « Observe-t-on des différences dans les taux d'octroi entre certains groupes de demandeurs ? »

	Les analyses pourront comparer notamment : Gender, Married, Education, Dependents, éventuellement Self_Employed. L'objectif est d'identifier des écarts observés entre groupes.
	Ces écarts ne seront pas automatiquement interprétés comme une discrimination. Compte tenu de la taille limitée du dataset, 
	cette analyse est considérée comme une analyse exploratoire de fairness et non comme une preuve statistique de discrimination.

***********************************************************************************************************************************************************************
			Question 5 — Complémentarité avec le Machine Learning
***********************************************************************************************************************************************************************
	
	« Comment l'analyse Business Intelligence complète-t-elle l'approche prédictive développée avec le Machine Learning ? »

	Deux approches complémentaires sont utilisées.
	Business Intelligence — Power BI répond principalement à la question : « Que s'est-il passé dans les décisions historiques et quels profils observe-t-on ? »

	Machine Learning — Le projet Machine Learning répond à une autre question : « Peut-on prédire la décision d'octroi pour une nouvelle demande ? »

	Le Machine Learning produit donc une capacité prédictive, tandis que Power BI transforme les données historiques en information analytique destinée à la décision.


*********************************************************************************************************************************************************************
			5. Dataset
*********************************************************************************************************************************************************************
	
	Le dataset contient : 614 observations et 13 variables originales
	Chaque ligne représente une demande de crédit. Loan_ID identifie la demande et non nécessairement un client unique. Le dataset ne permet donc pas de réaliser :
			- Une analyse longitudinale des clients
			- Une analyse des remboursements
			- Une analyse des défauts dans le temps
			- Une analyse de portefeuille par période
			- Une analyse de rentabilité client
	Aucune date artificielle n'est ajoutée au dataset.

	Variable		Description		Type
-------------------------------------------------------------------------------------------------------------------
	Loan_ID		Identifiant de la demande	Identifiant
	Gender		Sexe du demandeur		Catégorielle
	Married		Situation matrimoniale	Catégorielle
	Dependents	Nombre de personnes à charge	Catégorielle
	Education		Niveau d'éducation		Catégorielle
	Self_Employed	Statut professionnel		Catégorielle
	ApplicantIncome	Revenu du demandeur	Numérique
	CoapplicantIncome	Revenu du co-demandeur	Numérique
	LoanAmount	Montant demandé		Numérique
	Loan_Amount_Term	Durée du crédit		Numérique / discrète
	Credit_History	Historique de crédit		Catégorielle
	Property_Area	Zone de résidence		Catégorielle
	Loan_Status	Décision d'octroi		Catégorielle

*****************************************************************************************************************************************
			6. Préparation des données
******************************************************************************************************************************************
	La préparation des données est réalisée avec Power Query. Les principales opérations sont :
	Gestion des valeurs manquantes
		Pour les variables catégorielles :
			- Gender → Non renseigné
			- Married → Non renseigné
			- Dependents → Non renseigné
			- Self_Employed → Non renseigné
			- Credit_History → Non renseigné
	Les valeurs manquantes des variables financières telles que LoanAmount sont conservées lorsqu'aucune justification ne permet une imputation fiable.
	 Aucune suppression systématique des lignes n'est effectuée.

*******************************************************************************************************************************************************
			7. Variables calculées
*******************************************************************************************************************************************************
	
	RevenuTotal = ApplicantIncome + CoapplicantIncome

	Cette variable permet d'analyser la capacité financière globale du dossier.
	Ratio montant demandé / revenu
	LoanToIncomeRatio = LoanAmount / RevenuTotal
	Lorsque le revenu total ne permet pas un calcul valide, le ratio reste non renseigné.

*************************************************************************************************************************************************************
			8. Traitement des valeurs extrêmes
**************************************************************************************************************************************************************
	Les valeurs extrêmes ne sont pas automatiquement supprimées. Une valeur statistiquement élevée n'est pas nécessairement une erreur métier. 
	La règle méthodologique retenue est : « Les valeurs extrêmes plausibles sont conservées. 
	Une valeur n'est considérée comme aberrante que lorsqu'un contrôle statistique et/ou métier fournit une justification suffisante pour la traiter. »


***************************************************************************************************************************************************************
			9. Modèle de données : en etoile
***************************************************************************************************************************************************************
	Un modèle en étoile a été construit dans Power BI. Nous avons creer  : 
		- Dim_profil : ID_profil, genre,Education, employed, married, niveau_charges
		- Dim_zone :ID_zone, Property_Area
		- Dim_credit : ID_credit, historique_credit ID_profil, ID_credit, ID_zone, Loan_ID, ApplicantIncome, CoapplicantIncome, RevenuTotal, LoanAmount, LoanToIncomeRatio
		- Fact_Demande :


	Le dataset source étant une table tabulaire unique de petite taille, un modèle plat aurait été suffisant techniquement. 
	Nous avons néanmoins construit un modèle dimensionnel afin de démontrer une démarche de modélisation adaptée aux usages analytiques de Power BI.
	Cette architecture permet de séparer : 
		- Les mesures et informations transactionnelles dans la table de faits
		- Les caractéristiques descriptives dans les dimensions
	Elle facilite également la création des mesures DAX et la maintenance du modèle.

*****************************************************************************************************************************************************************
			10. KPI principaux
*****************************************************************************************************************************************************************
	les principaux KPI sont : 
		***** Volume : 
			-Nombre de demandes
			- Nombre d'accords
			- Nombre de refus
		***** Décision :
			- Taux d'octroi
			- Taux de refus
		**** Financier :
			- Revenu moyen
			- Montant moyen demandé
			- Revenu total moyen
			- Ratio moyen montant/revenu
		**** Profil :
			- Répartition des demandeurs
			- Répartition par niveau d'éducation
			- Répartition par situation matrimoniale
			- Répartition par zone

*********************************************************************************************************************************************************************
			11. Pages du dashboard
*********************************************************************************************************************************************************************
	Page 1 — Vue d'ensemble
		« Comprendre rapidement la structure globale des demandes et des décisions. »

	Visualisations : KPI Cards, taux d'octroi, accords/refus, répartition géographique, profils des demandeurs, indicateurs financiers.


	Page 2 — Credit History & décision
		« Analyser l'association entre l'historique de crédit renseigné et la décision. »
	
	Visualisations : taux d'octroi par Credit_History, nombre de demandes, comparaison favorable / défavorable / non renseigné.

	Page 3 — Capacité financière
		« Étudier l'association entre les revenus, le montant demandé et la décision. »

	Visualisations : revenu moyen par décision, montant moyen demandé, ratio montant/revenu, segmentation des demandes selon le poids du montant demandé.

	Page 4 — Équité / Fairness
		« Identifier les différences observées de taux d'octroi entre groupes. »

	Comparaisons : Gender, Married, Education, Dependents, autres caractéristiques pertinentes. L'analyse sera présentée comme exploratoire.


	Page 5 — Complementarité BI et Machine Learning (ML)
		« Quelles sont les observations communes et dissemblantes entre BI et ML. »

	Comparaisons : affichage de resultats clés et conlusion

*********************************************************************************************************************************************************************
			12. Technologies utilisées
*********************************************************************************************************************************************************************
	nous avons  utilisée pour ce projet Power BI :
		
		- Power BI Desktop
		- Power Query
		- DAX
		- Git
		- GitHub


*********************************************************************************************************************************************************************
			13. Méthodologie
*********************************************************************************************************************************************************************
	Le projet suit les étapes suivantes :
		1. Compréhension du problème métier  >>>  2. Audit des données  >>>  3. Nettoyage avec Power Query  >>>  4. Transformation des variables   >>>  5. Modélisation en étoile >>>
		6. Création des mesures DAX  >>>  7. Construction du dashboard  >>> 8. Analyse des résultats  >>>  9. Documentation  >>>  10. Publication GitHub
	
	Chaque étape est documentée afin de rendre le projet reproductible.

***********************************************************************************************************************************************************************
			14. Limites du projet
***********************************************************************************************************************************************************************
	les limites de ce projets  sont:
		- Taille du dataset : Le dataset contient seulement 614 demandes. Les résultats doivent donc être interprétés avec prudence.
		- Absence de dimension temporelle : Le dataset ne contient pas de date. Il n'est donc pas pertinent de construire artificiellement une évolution mensuelle, 
						une évolution annuelle ou une tendance temporelle.
		- Absence de données de remboursement : Le dataset ne permet pas d'analyser les remboursements, les impayés, les défauts, ni la performance réelle des crédits.
		- Absence d'identifiant client : Loan_ID identifie une demande et non nécessairement un client. Une analyse de fidélité ou de comportement longitudinal n'est donc pas possible.
		- Analyse de fairness : Les écarts observés entre groupes ne constituent pas à eux seuls une preuve de discrimination. 
				Ils doivent être considérés comme des signaux nécessitant éventuellement des analyses statistiques et métier complémentaires.

*************************************************************************************************************************************************************************
			15. Résultats attendus
************************************************************************************************************************************************************************
	À la fin du projet, le dashboard devra permettre à un décideur de :
		- Comprendre la structure des demandes
		- Identifier les profils les plus représentés
		- Comparer les taux d'octroi
		- Analyser les caractéristiques financières des dossiers
		- Observer l'association entre historique de crédit et décision
		- Identifier certains écarts entre groupes
		- Disposer d'indicateurs synthétiques pour faciliter l'analyse
		- Comprendre comment la Business Intelligence complète une approche Machine Learning

	EN conclusion:
		Ce projet met en œuvre une démarche de Business Intelligence appliquée à l'analyse des demandes de crédit.
		L'objectif n'est pas de remplacer le Machine Learning, mais de démontrer la complémentarité entre deux approches :
		« Le Machine Learning cherche à prédire une nouvelle décision ; Power BI cherche à comprendre les décisions observées. »
