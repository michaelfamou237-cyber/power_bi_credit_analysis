
# Power Bi - Credit Analysis
 Analyse exploratoire des demandes de credit avec Power BI

# I - Objectif principal

Analyser les caracteristiques des demandeurs et des demandes de credit afin d'identifier les facteurs
associés a l'octroi ou au refus de crédit. ?


# II-  les questions secondaires : squelette de notre analyse par page et tableau de bord avec Insights

	1 Executive Overview : quelle est la situation globales des demandes ?   

	2- Profil des demandeurs : Qui demandent les credits ? dresser le portait des demandes 

	3- Analyses financieres : Quelle relations entre revenus, montant et octroi ?

	4- Analyse de l'octroi : Quels profils obtiennent/refusent le credit

	5- Risk Analysis : Quels dossiers correspondent les caracteristiques les plus risquees 

NB: on va construire une storystelling : 

	Demandes ---> Profils ---> Situation financieres ------> Decision ----> Risque 


III - Les axes des dashboards : Penser aux groupes dans les variables 

axe 1 : performance du porte feuille

combien de demandes ? combien acceptees 
KPI
nombre de demandes
nombre d accords
nombre de refus 
taux d octroi / refus
montant moyen demandee
Revenu moyen

axe 2 : Profil des demandeurs

recherche:
taux d'octroi selon le genre
taux d octroi selon les maries
taux d octroi selon l education
taux d octroi selon les charges
taux d octroi selon l'emploi/ ou non
NB: c est juste une decouverte de donnees pas de relation de ce que cachnet les donnees

axe 3: Capacite financiere

nouvelle variable:
revenu total du menage = revenu demandeur + revenu codemandeur
ratio_endettement_menage = montant de pret / revenu_menage 
ratio_endettement =  montant de pret  / revenu_demandeur
recherche :
la relation entre les revenus 


axe 3: 

recherche : Quels types de credits sont associees aux decisions d'octroi

recherche :
la relation entre les revenus , historique de credit et l octroi de credit


axe 4: 
rechercher : le taux d'octroi varie selon la zone de residence ?

croiser = zone * Education
croiser =zone * Income
croiser = zone * octroi

axe 5: 

croisement:
Education * revenu * decision
historique de pret * montant
Revenu * montant du pret
















# Technologies

- Power BI
- Power Query
- DAX
- Git / Github
