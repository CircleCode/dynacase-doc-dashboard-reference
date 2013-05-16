# Paramétrage initial {#dashboard-ref:7ac54e01-5120-45e1-b640-686a44bdc741}

Le dashboard est paramétré au moyen de documents dynacase. Il se base sur les famille suivantes :

Dashboard Widget
:   cette famille permet de définir un widget.  
    Pour définir, un widget vous devez indiquer :
    
    *   son nom,
    *   son action de consultation,
    *   son action de paramétrage. Si aucune action de paramétrage est indiquée alors le widget ne sera pas paramétrable.

Dashboard catégorie
:   cette famille permet de définir une catégorie de widgets.  
    Vous pouvez créer une catégorie en créant un nouveau document et ajouter des widgets dans cette catégorie en allant dans le menu *autres > ajouter un document*.

Dashboard Tab Template
:   cette famille permet de définir une typologie de dashboard (contenu, nom, ordre d'affichage, accessibilité).  
    Les utilisateurs ou groupes ayant accès verront ce dashboard s'ajouter aux leurs lors de leur prochaine consultation du dashboard. Vous pouvez ensuite indiquer les éléments suivants :
    
    *   l'ensemble des widgets qui sont mis à disposition par défaut et si les widgets sont supprimables ou pas,
    *   l'importance d'affichage des tab (les widgets ayant l'importance la plus haute seront placés en premier),
    *   le nombre de colonnes dans le tab,
    *   les catégrories associées à ce tab (si aucune catégorie n'est présente, il n'est pas possible d'ajouter de nouveau widget).
    
    Il est à noter que si vous spécifiez pour un widget un numéro de colonne plus élévé que le nombre de colonnes de la tab, alors la tab est choisie en utilisant le reste de la division entière du numéro de colonne par le nombre de tab.

Dashboard Tab User
:   cette famille représente les tab des utilisateurs.  
    Elle est synchronisée avec les tab template (une modification du template entraînera la mise à jour des tab user associées).

Si vous supprimez un widget d'un tab template, il n'est pas supprimé des tab user où il est déjà présent.

Si vous supprimez un tab template, les tab user déjà initié associé à ce tab template ne sont pas supprimés.

Pour paramétrer le dashboard, vous devez donc :

1. créer les widgets que vous souhaitez présenter,
2. créer les catégories que vous souhaitez présenter et associer les widgets aux catégories,
3. créer les template de Dashboard pour les différentes catégories d'utilisateurs ayant accès au dashboard.

Si vous ne spécifiez de tab template pour une catégorie d'utilisateur, ceux-ci verront un message d'erreur. Il est donc conseillé de faire un template de dashboard par défaut.

Vous pouvez aussi définir des assets (js et css) qui seront ajoutés dans la page du dashboard. Pour ce faire, il suffit de compléter le paramètres applicatif *Asset (js et css) ajouté à la page dashboard UI* (ASSETS) de l'application *DASHBOARD_UI*.
