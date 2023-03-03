# Stage-en-python-et-analyse-de-donn-es

1. Lire le fichier CSV et stocker les données dans une structure de données appropriée.
```
import pandas as pd

# importation des donnees a partir du fichier Data.csv
data = pd.read_csv('Data.csv')

#affichage des regions distinctes 
print('::::::::::::les  regions :::::::::::')
regions = data['region'].unique()
print(regions)


print('\n')
# Affichage des produits distincts 
print('::::::::::::les produits :::::::::::')
produits = data['produit'].unique()
print(produits)





ventes = {}
couts = {}
benefices = {}
```

2. Calculer les ventes totales et le bénéfice pour chaque région.
```
for index, row in data.iterrows():
    # calcul des ventes, des couts et des benefices pour chaque region
    region = row['region']
    quantite_vendue = row['quantite_vendue']
    prix_unitaire = row['prix_unitaire']
    cout_total = row['cout_total']
    
    # mise a jour des ventes et des couts pour la region correspondante
    if region not in ventes:
        # Si la region n'a pas encore ete traitee, initialisation des ventes et des couts
        ventes[region] = quantite_vendue * prix_unitaire
        couts[region] = cout_total
    else:
        # si la region a deja ete traitee, ajout des ventes et des couts actuels aux ventes et aux couts existants
        ventes[region] += quantite_vendue * prix_unitaire
        couts[region] += cout_total
        
# calcul des benefices pour chaque region
for region in ventes:
    benefices[region] = couts[region]- ventes[region] 
    
    
print('\n')
# affichage des ventes totales par region
print('::::::::::::Ventes totales par region :::::::')
for region in regions:
    print('Ventes totales pour la region',region,'===',ventes[region])
print('\n')

#affichage des benefices par region
print('::::::::::::benefices par region :::::::::::')
for region in regions:
    print('benefices de la region',region,'===',benefices[region])
 
```


3. Afficher le pourcentage de ventes totales de chaque produit pour l'ensemble de l'entreprise.
```

#calcul le pourcentage de ventes pour chaque produit
ventes_totales = 0
ventes_par_produit = {}
 
#calcul des ventes totales et des ventes par produit
for index, row in data.iterrows():
    produit = row['produit']
    quantite_vendue = row['quantite_vendue']
    prix_unitaire = row['prix_unitaire']
    # calcul de la somme des ventes totales
    ventes_totales += quantite_vendue * prix_unitaire
    # calcul des ventes par produit
    if produit not in ventes_par_produit:
        ventes_par_produit[produit] = quantite_vendue * prix_unitaire
    else:
        ventes_par_produit[produit] += quantite_vendue * prix_unitaire

#calcul du pourcentage de ventes pour chaque produit

for produit in ventes_par_produit:
    ventes_par_produit[produit] = ventes_par_produit[produit] / ventes_totales * 100

#affichage des resultats
print('\n')
print('::::::::::Pourcentage de ventes par produit :::::::::::')
for produit in produits:
    print('Pourcentage de ventes par le produit :',produit,' est ',ventes_par_produit[produit])

```

4.Ajouter une fonctionnalité pour afficher les dates avec les ventes les plus élevées pour chaque région.
```
print('::::::::::les dates avec les ventes les plus elevees pour chaque region :::::::::::')
for region in regions:
    #selectionner les donnees de la region actuelle
    x = data[data['region'] == region]
    #trouver la quantite de vente maximale pour la region actuelle
    max_ventes = x['quantite_vendue'].max()
    #trouver la date associee a la vente maximale pour la region actuelle
    date = x[x['quantite_vendue'] == max_ventes]['date'].values[0]
    #afficher la region et la date avec les ventes les plus elevees
    print('Pour la region ',region,' la date avec les ventes les plus elevees est le ',date)
 
```

5.Ajouter une fonctionnalité pour afficher les régions avec la plus grande marge bénéficiaire.
```
print('::::::::::les regions avec la plus grande marge beneficiaire. :::::::::::')
regions_best = [] #regions avec la plus grande marge beneficiaire
marge_best = 0 #la plus grande marge beneficiaire

for region in regions:
    #calcul de la marge beneficiaire pour la region courante
    marge = benefices[region]
    #verification si la marge beneficiaire de cette region est plus grande que la plus grande marge beneficiaire enregistree jusqu'a present
    if marge > marge_best:
        regions_best = [region]  #si oui, on met a jour la liste des regions ayant la plus grande marge beneficiaire
        marge_best = marge       #et on met a jour la plus grande marge beneficiaire
    #si la marge beneficiaire est egale a la plus grande marge beneficiaire, on ajoute la region courante a la liste des regions avec la plus grande marge beneficiaire
    elif marge == marge_best:
        regions_best.append(region)

#affichage des resultats
print('Les regions avec la plus grande marge beneficiaire sont :')
for region in regions_best:
    print(region)
```

