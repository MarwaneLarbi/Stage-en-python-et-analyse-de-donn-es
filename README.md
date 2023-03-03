# Stage-en-python-et-analyse-de-donn-es

### 1. Lire le fichier CSV et stocker les données dans une structure de données appropriée.
j'ai importé la bibliothèque pandas et j'ai utilisé la fonction read_csv pour lire un fichier CSV appelé "Data.csv".

Ensuite, j'ai affiché les régions et les produits distincts dans les données en utilisant la méthode unique(). Enfin, j'ai créé des dictionnaires vides pour stocker les ventes, les coûts et les bénéfices.
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

### 2. Calculer les ventes totales et le bénéfice pour chaque région.

j'ai utilisé   la méthode "iterrows()" de pandas pour parcourir chaque ligne du dataframe "data" et calculer les ventes, les coûts et les bénéfices pour chaque région.

Pour chaque ligne, je récupère la région, la quantité vendue, le prix unitaire et le coût total de chaque produit vendu. Il met ensuite à jour les ventes et les coûts pour la région correspondante, en initialisant les ventes et les coûts s'il s'agit d'une nouvelle région, ou en ajoutant les ventes et les coûts actuels aux ventes et aux coûts existants pour cette région.

Une fois que les ventes et les coûts ont été calculés pour chaque région, on calcule les bénéfices en soustrayant les ventes des coûts. Enfin,  affiche les ventes totales et les bénéfices pour chaque région.
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
### 3. Afficher le pourcentage de ventes totales de chaque produit pour l'ensemble de l'entreprise.
Tout d'abord, j'ai créé une variable "ventes_totales" qui stocke la somme totale des ventes, et un dictionnaire "ventes_par_produit" qui contient les ventes par produit.

Ensuite, j'ai parcouru chaque ligne du dataframe "data" en utilisant la méthode "iterrows()" de pandas. Pour chaque ligne, j'ai récupéré le nom du produit, la quantité vendue et le prix unitaire. J'ai calculé la somme totale des ventes en multipliant la quantité vendue par le prix unitaire, et j'ai ajouté ce montant à "ventes_totales". J'ai également mis à jour le dictionnaire "ventes_par_produit" en ajoutant les ventes pour le produit correspondant.

Une fois que j'ai calculé les ventes totales et les ventes par produit, j'ai calculé le pourcentage de ventes pour chaque produit en divisant les ventes par produit par les ventes totales et en multipliant par 100.

Enfin, j'ai affiché les résultats en parcourant les produits et en affichant le pourcentage de ventes pour chaque produit.

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

### 4.Ajouter une fonctionnalité pour afficher les dates avec les ventes les plus élevées pour chaque région.
J'ai parcouru chaque région de la liste "regions" et sélectionné les données correspondantes dans le dataframe "data". Ensuite, j'ai trouvé la quantité de vente maximale pour chaque région en utilisant la méthode "max()" de pandas. 

Puis, j'ai trouvé la date associée à la vente maximale en sélectionnant la ligne où la quantité de vente est maximale pour cette région.

Enfin, j'ai affiché la région et la date avec les ventes les plus élevées pour chaque région.
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

### 5.Ajouter une fonctionnalité pour afficher les régions avec la plus grande marge bénéficiaire.
J'ai écrit le code ci-dessus qui permet de déterminer les régions ayant la plus grande marge bénéficiaire.
Tout d'abord, je crée une liste "regions_best" qui va contenir les régions ayant la plus grande marge bénéficiaire, ainsi qu'une variable "marge_best" qui va stocker la valeur de la marge bénéficiaire maximale.

Ensuite, je parcours chaque région du dataframe et calcule la marge bénéficiaire associée. Pour chaque région, je récupère la marge bénéficiaire calculée précédemment et je la compare à la marge bénéficiaire maximale enregistrée jusqu'à présent. Si la marge bénéficiaire courante est supérieure à la marge maximale, je mets à jour la liste des régions ayant la plus grande marge bénéficiaire et la marge maximale. Si la marge bénéficiaire est égale à la marge maximale, j'ajoute simplement la région courante à la liste des régions ayant la plus grande marge bénéficiaire.

Enfin, j'ai  affiché les régions ayant la plus grande marge bénéficiaire en parcourant la liste "regions_best".
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

## Exemple de resultat 
```
::::::::::::les  regions :::::::::::
['Nord' 'Sud']


::::::::::::les produits :::::::::::
['A' 'B' 'C']


::::::::::::Ventes totales par région :::::::
Ventes totales pour la region Nord === 14000.0
Ventes totales pour la region Sud === 8500.0


::::::::::::benefices par région :::::::::::
benefices de la region Nord === 4700.0
benefices de la region Sud === 4600.0


::::::::::Pourcentage de ventes par produit :::::::::::
Pourcentage de ventes par le produit : A  est  6.666666666666667
Pourcentage de ventes par le produit : B  est  53.333333333333336
Pourcentage de ventes par le produit : C  est  40.0


::::::::::les dates avec les ventes les plus élevées pour chaque région :::::::::::
Pour la région  Nord  la date avec les ventes les plus élevées est le  2022-01-02
Pour la région  Sud  la date avec les ventes les plus élevées est le  2022-01-01




::::::::::les régions avec la plus grande marge bénéficiaire. :::::::::::
Les regions avec la plus grande marge beneficiaire sont :
Nord
```
