# Paramétrage initial

Initialisation du dashboard. Le dashboard utilise pour fonctionner les familles suivantes :

* **Dashboard Widget** : cette famille permet de définir un widget. Pour définir, un widget vous devez indiquer son nom, son action de consultation et son action de paramétrage. Si aucune action de paramétrage est indiquée alors le widget ne sera pas paramétrable.

* **Dashboard catégorie** : cette famille permet de définir une catégorie de widgets. Vous pouvez créer une catégorie en créant un nouveau document et ajouter des widgets dans cette catégorie en allant dans le menu *ajouter un document* dans autre.

* **Dashboard Tab Template** : cette famille permet de définir une typologie de dashboard (contenu, nom, ordre d'affichage, accessibilité). Les utilisateurs ou groupes ayant accès verront ce dashboard s'ajouter aux leurs lors de leur prochaine consultation du dashboard. Vous pouvez ensuite indiquer les éléments suivants :

	* l'ensemble des widgets qui sont mis à disposition par défaut et si les widgets sont supprimables ou pas,
	* l'ordre d'affichage des tab (plus l'ordre est élévé plus le widget apparaît en premier),
	* le nombre de colonnes dans la tab,
	* les catégrories associées à cette tab (si aucune catégorie n'est présente, il n'est pas possible d'ajouter de nouveau widget).

NB : il est à noter que si vous spécifiez pour un widget un numéro de colonne plus élévé que le nombre de colonnes de la tab, alors la tab est choisie l'est en utilisant le reste de la division entière du numéro de colonne par le nombre de tab.

* **Dashboard Tab User** : cette famille représente les tab des utilisateurs. Elle est synchronisée avec les tab template (une modification du template entraînera la mise à jour des tab user associées).

NB : si vous supprimez un widget d'un tab template, il n'est pas supprimé des tab user où il est déjà présent.
NB : si vous supprimez un tab template, les tab user déjà initié associé à ce tab template ne sont pas supprimés.

Pour paramétrer le dashboard, vous devez donc :

* créer les widgets que vous souhaitez présenter,

* créer les catégories que vous souhaitez présenter et associer les widgets aux catégories,

* créer les template de Dashboard pour les différentes catégories d'utilisateurs ayant accès au dashboard.

NB : il est à noter que si vous ne spécifiez de tab template pour une catégorie d'utilisateur, ceux-ci verront un message d'erreur. Il est donc conseillé de faire un template de dashboard par défaut.

NB : vous pouvez aussi définir des assets (js et css) qui seront ajoutés dans la page du dashboard. Pour ce faire, il suffit de compléter le paramètres applicatif *Asset (js et css) ajouté à la page dashboard UI* (ASSETS) de l'application DASHBOARD_UI.
