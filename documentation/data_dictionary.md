
*********************************************************************************************************************************************************
*********************************************************************************************************************************************************
	Dictionnaire des données
*********************************************************************************************************************************************************
	Ce document décrit les variables du jeu de données initial et les variables dérivées créées lors de la phase de Feature Engineering.

	1. Variables originales (dataset dbcredit.csv)
	------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
		Variable		Type					Description
	--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
		Loan_ID		Texte					Identifiant unique de la demande de crédit.
		Gender		Texte					Sexe du demandeur (Male/Female).
		Married		Texte					Situation matrimoniale (Yes/No).
		Dependents	Texte					Nombre de personnes à charge (0=pas de charge, 1=faible, 2=moyen, 3+=forte).
		Education	Texte	Niveau d’éducation (Graduate/Not Graduate).                Diplomé ou non - diplomé
		Self_Employed	Texte					Statut professionnel (Yes/No).
		ApplicantIncome	Numérique				Revenu du demandeur.
		CoapplicantIncome	Numérique				Revenu du co-demandeur.
		LoanAmount	Numérique				Montant du prêt demandé (en milliers).
		Loan_Amount_Term	Numérique				Durée du prêt (en mois).
		Credit_History	Numérique				Historique de crédit (1 = bon, 0 = mauvais).
		Property_Area	Texte					Zone de résidence (Urban, Semiurban, Rural).
		Loan_Status	Texte					Variable cible (Y = Accord, N = Refus).


	2. Variables dérivées (Feature Engineering)
	--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
	
	Variable		Type		Formule / Règle					Description
	--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------	
	RevenuTotal	Numérique	ApplicantIncome + CoapplicantIncome			Capacité financière brute du ménage.
	LoanToIncomeRatio	Numérique	LoanAmount / RevenuTotal				Poids du montant demandé relativement au revenu. Si RevenuTotal = 0, alors null.
	
	Credit_History_Label	Texte		Catégorisation de Credit_History	« ayant un historique »,                  Ayant eu un credit par le passé
							« pas historique », « non_renseigné ».

	statut_pret		Texte		Si statut pret = "Y" alors « Accord », sinon « Refus ».		Libellé métier de la décision.

	niveau_charges 	Texte		Catégorisation de Dependents	Aucune charge, Charge faible, 
							Charge moyenne, Charge forte, Non renseigné.

