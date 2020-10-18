# CDA_API

## API pour la selection CDA

Cette API a été codée en PHP / requêtes SQL pour (et testée sur) un serveur et une base de données en local (via Apache + MySQL). Les ressources correspondent au diagramme "selection-uml.jpg" également présent dans le repo. Afin de répondre à la convention d'une API REST, chaque endpoint renverra en format JSON :
- sucess (true or false)
- message (erreur spécifiée ou execution correcte)
- data (quand cela est attendu)

Au niveau hierarchique, chaque fichier.php comprennant les endpoints appelle en premier lieu le fichier "header.php" qui comprend les informations de connexion à la BDD via la couche intermédiaire PDO.

## Messages d'erreur

Afin de faciliter l'utilisation de l'API, les cas d'usage ont été pris en compte, avec notamment des messages d'erreur explicites pour les clients. De plus, le code intègre directement la prise en compte des erreurs de typage, limitant la déclaration de paramètres non attendus par la base de données (chaînes de caractères, entiers, date).

## Liste des actions disponibles 

### Créer un Topic

    **POST** http://localhost/API/CDA/ajouter_topic.php

**Paramètres**

**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Title	| Oui 		| String	| Aucune 				| Préciser ici le titre en body de la requête POST pour l'ajout du topic 												| Non Applicable

**Informations complémentaires**

La table topics comporte 2 colonnes : id_topic + title. L'ajout d'un nouveau titre ne nécessite pas la déclaration de l'id_topic, qui s'incrémente automatiquement (numero du dernier id_topic + 1) au moment de la requête. Chaque ressource possédera donc bien un attribut dans chaque colonne.

**Format de la réponse**

	{
	"sucess ": true, 
	"message": "Le topic a bien été ajouté.
	}

**exemple**

    **POST** http://localhost/API/CDA/ajouter_topic.php Key = title Keyvalue = "Ceci est un exemple"

Cette requête ajoutera un topic à la table, avec comme titre "ceci est un exemple", et un id_topic unique automatiquement attribué.

### Créer un Post

    **POST** http://localhost/API/CDA/ajouter_post.php

**Paramètres**

**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
id_topic	| Oui 		| Int	| Aucune 				| Préciser ici l'id du topic unique associé à ce post 												| Non Applicable
content	| Oui 		| String	| Aucune 				| Préciser ici le contenu du post 												| Non Applicable
author	| Oui 		| String	| Aucune 				| Préciser ici l'auteur du post 												| Non Applicable
date	| Oui 		| DateTime	| Aucune 				| Préciser ici la date du post au format yyyy-mm-dd H:m:s 												| Non Applicable

**Informations complémentaires**

La table posts comporte également un id_post. Comme pour la création d'un topic, l'ajout d'un nouveau post ne nécessite pas la déclaration de l'id_post, qui s'incrémente automatiquement (numero du dernier id_topic + 1) au moment de la requête. Chaque ressource possédera donc bien cet attribut.

Conformément au diagramme des ressources, un post appartient obligatoirement à un Topic unique. Pour cela, la table comprends la colonne id_topic qui précisera la connexion avec la table topics. Cet élément est donc à déclarer au moment de la création du post.

**Format de la réponse**

	{
	"sucess ": true, 
	"message": "Le post a bien été ajouté.
	}

**exemple**

    **POST** http://localhost/API/CDA/ajouter_post.php Key = id_topic Keyvalue = "1" Key = content Keyvalue = "Voici le premier post lié au topic 1." Key = author Keyvalue = "Cyril" Key = date Keyvalue = "2020-10-18 12:00:00"

Cette requête ajoutera un post à la table avec son id_post et l'association via son id_topic unique à l'autre table.


### Afficher un Topic


**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>


### Afficher un Post


**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>

### Modifier un Topic

**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>


### Modifier un Post


**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>

### Suprrimer un Topic

**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>


### Supprimer un Post


**Nom**			| **Requis**| **Type** 	| **Valeur par défaut**	| **Description**																| **Valeur possible**
----------------|-----------|-----------|-----------------------|-------------------------------------------------------------------------------|----------------------
Authorization	| Oui 		| String	| Aucune 				| Jeton d'authentification utilisé 												| bearer <valeur de jeton>
