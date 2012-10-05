# Widget de recherche

Le dashboard est fourni par défaut avec un widget de recherche. Celui-ci permet de présenter une collection de documents (recherche ou dossier) dans le dashboard.

Pour le mettre à disposition et le paramétrer vous devez :

* Créer un nouveau document de la famille **Dashboard Widget** et indiquer dans le paramétrage :

	* pour la consultation :

		* comme application : **DASHBOARD_UI**
		* comme action : **WIDGET_SEARCH_CONSULT**
		* vous pouvez de plus indiquer dans les paramétres par défaut :

			* *collectionId* : avec l'id ou le nom logique d'une collection (c'est la collection par défaut du widget),
			* *fetchApp* : le nom d'une application de recherche,
			* *fetchAction* : le nom d'une action de recherche,
			* *slice* : un entier représentant le nombre d'élément à afficher par page dans le widget,
			* *start* : le rang du premier élément à afficher
			* *title* : le titre du widget

	* pour le paramétrage : 

		* comme application : **DASHBOARD_UI**
		* comme action : **WIDGET_SEARCH_PARAM**

		* vous pouvez de plus indiquer dans les paramètres par défaut :

			* *baseCollectionSearch* : un id ou nom logique de collection par défaut dans lequel seront recherchées les collections utilisables comme *collectionId* (par défaut toutes les collections, recherche et répertoire visibles, par l'utilisateur sont affichés),
			* *canModifyCollection* : TRUE ou FALSE si à FALSE la collection par défaut ne pourra pas être modifiée (TRUE par défaut),
			* *canModifySlice* : TRUE ou FALSE si à FALSE le slice par défaut ne pourra pas être modifié.

*note* : Si vous voulez créer un widget non paramétrable ne remplissez pas la partie paramétrage, il serait de plus dommage de faire remplir l'action de paramétrage avec les deux canModify à FALSE car cela ferait une IHM avec juste un bouton de validation,
*note* : En consultation, vous devez fournir au moins un collectionId ou un fetchApp,
*note* : fetchApp et fetchAction permettent de changer l'action par défaut qui est appelée pour compléter le widget.

## Paramétrage avancé du widget de recherche

Si vous souhaitez créer votre propre application/action de recherche, vous pouvez vous inspirer de l'action widget_search_content_json dans l'application DASHBOARD_UI, qui a la forme suivante :

	<?php

	require_once "DASHBOARD_UI/widget_search_content.php";

	/**
	 * Return the json content for a docGrid
	 *
	 * @param Action $action
	 */
	function widget_search_content_json(Action &$action)
	{
	    $return = array(
	        "success" => true
	    );

	    try {
	        $return["data"] = widget_search_content($action);
	    } catch (Exception $e) {
	        $message = sprintf("(%s from %s : %s) %s", get_class($e), $e->getFile(), $e->getLine(), $e->getMessage());
	        error_log(__METHOD__ . ' ' . $message);
	        $return["success"] = false;
	        $return["error"] = $message;
	        unset($return["data"]);
	    }

	    $action->lay->template = json_encode($return);
	    $action->lay->noparse = true;


	    header('Content-type: application/json');
	}

Cette action appelle l'action par défaut pour compléter le widget et retourne le contenu en JSON. Il est à noter que vous pouvez passer en deuxième paramètre à **widget_search_content** un objet **SearchDoc** il sera alors utilisé en lieu et place de la collection définie par collectionId.

## Création d'un nouveau widget

Pour créer un nouveau type de widget, il faut définir une ou des actions fournissant deux types d'éléments :

* pour la consultation : des assets (js, css) et du HTML à intégrer ,

* pour le paramétrage : une page HTML qui présente et enregistre les paramètres de l'action.

### Exemple simple d'action de consultation :

	<?php

	require_once 'FDL/freedom_util.php';

	function hello_world(Action &$action)
	{

		$usage = new \ActionUsage($action);

		$elementId = $usage->addNeeded("elementId", "elementId");
		$dashboardUUID = $usage->addNeeded("dashboardUUID", "dashboardUUID");
		$widgetUUID = $usage->addNeeded("widgetUUID", "widgetUUID");
		$message = $usage->addOption("message", "message", array(), "coucou");
		$color = $usage->addOption("color", "message", array(), "");

		$userName = \new_Doc('', $action->user->fid)->getTitle();

		$return = array(
			"success" => true,
			"data" => array(
			"assets" => array(
			"js" => array("DASHBOARD_WIDGETS/Layout/hello_world.js"),
			"css" => "DASHBOARD_WIDGETS/Layout/hello_world.css"
			),
			"html" => '<script type="text/javascript">
			window.hello_world.hello("#' . $elementId . '", "' . $message . ' ' . $userName . '", "' . $color . '");
			</script> <div class="helloPeople">Hello World !!!!!</div>',
			"title" => 'hello world'
			)
		);

		$action->lay->template = json_encode($return);
		$action->lay->noparse = true;
		header('Content-type: application/json');

	}

En entrée de l'action de consultation, l'application DASHBOARD_UI envoie les éléments suivants :

* elementId : l'id au sens DOM de l'élément dans le quel est intégré le widget (le HMTL lui même est intégré dans un élement DIV contenu dans cette élément ayant la classe "dcpui-dashboardwidget-content" accessible de la manière suivant en jQuery $("#elementId").find(".dcpui-dashboardwidget-content")),

* dashboardUUID : l'identifiant unique du dashboard en cours,

* widgetUUID : l'identifiant unique du widget en cours.

* l'ensemble des paramètre de configuration du widget en cours (valeurs par défaut et valeurs enregistrées dans le dashboard).

L'action de consultation doit fournir un retour en JSON semblable à celui ci-dessus :

* "success" : indique le succès de la construction du widget (booléen),

* "data" : contient le HTML et les assets :

	* assets : les fichiers js et css qui seront intégrés dans la page :

		* js : string ou array de string contenant les url des fichiers JS à intégrer, ceux-ci sont intégrés en série,
		* css : string ou array de string contenant les url des fichiers css à intégrer

	* html : fragement html intégré dans la page web

Vous avez à votre disposition une classe permettant de consulter et enregistrer les paramètres de configuration du widget, celle-ci est présente sous le namespace **dcp\dashboard_ui** et s'utilise de la manière suivante :

* intialisation : $dashboardManager = new dui\DashboardManager();

* récupérer la configuration : $currentConf = $dashboardManager->getWidgetConf($dashboardUUID, $widgetUUID);. Celle ci est renvoyée sous la forme d'un array associatif clef/valeur. Les clefs sont identique à celle passée en POST lors de l'appel en consultation de l'action

* sauvegarder la configuration : $currentConf = $dashboardManager->setWidgetConf($dashboardUUID, $widgetUUID, $conf);. Ou $conf est un array associatif clef/valeur qui seront stockés en bases pour être ensuite envoyées lors de l'appel en consultation.

### Exemple simple d'action de paramétrage : 

	<?php

	use dcp\dashboard_ui as dui;

	require_once 'FDL/freedom_util.php';

	function hello_world(Action &$action)
	{

		$usage = new \ActionUsage($action);

		$dashboardUUID = $usage->addNeeded("dashboardUUID", "dashboardUUID");
		$widgetUUID = $usage->addNeeded("widgetUUID", "widgetUUID");
		$modeSave = $usage->addOption("modeSave", "modeSave", array(), false);
		$widgetConf = $usage->addOption("widgetConf", "widgetConf", array(), false);

	if ($modeSave !== false) {
			$dashboardManager = new dui\DashboardManager();
			$dashboardManager->setWidgetConf($dashboardUUID, $widgetUUID, json_decode($widgetConf, true));
			Redirect($action, "DASHBOARD_UI", "RELOAD_WIDGET", '?dashboardUUID=' . urlencode($dashboardUUID) . '&widgetUUID=' . urlencode($widgetUUID) . '&');
	} 
	else {
		$dashboardManager = new dui\DashboardManager();
		$currentConf = $dashboardManager->getWidgetConf($dashboardUUID, $widgetUUID);

		$action->lay->set("message", $currentConf["message"]);
		$action->lay->set("color", $currentConf["color"]);
		$action->lay->set("dashboardUUID", $dashboardUUID);
		$action->lay->set("widgetUUID", $widgetUUID);
	}
	}

Dans l'exemple ci-dessous l'action en modeSave à false utilise le moteur de Layout pour produire un formulaire qui appelle de nouveau l'action en modeSave à true qui elle sauvegarde la configurationet redirige vers une page spécifique qui fermera l'overlay d'affichage de l'action de paramétrage et rechargera le widget.

Une action de paramétrage reçoit en entrée les éléments suivants :

* dashboardUUID : l'identifiant du dashboard en cours,

* widgetUUID : l'identifiant du widget en cours

et elle doit en fin de paramétrage renvoyer vers l'action **RELOAD_WIDGET** de l'application **DASHBOARD_UI** avec comme paramètres le **dashboardUUID** et le **widgetUUID** (attention ceux-ci doivent être fournis sous la forme d'une string url_encodée terminant par un **&**).

### Divers

Recharger un widget : vous pouvez recharger un widget en utilisant l'api du widget et en faisant un appel de ce genre :

$("#elementId").dashboardwidget("refresh", config)

L'objet config est optionnel et est un objet javascript contenant les paramètres attendu par l'action de consultation du widget.
