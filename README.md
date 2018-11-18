# Code Review - What to check for a good code review ?

Ce document reprend les différents points qui sont pris en compte lors des audits de code.
Sert de checklists des points à vérifier lors des audits.
Il n'est évidemment pas exhaustif.

Il s'applique pour les projets SpringBoot et peut être adaptable à d'autres typologies de projets.


# Backend SpringBoot Java
## Organisation des sources

 * Y a t'il pas des sources superflues (fichiers temporaires, fichiers de constructions, binaires finaux, fichiers de logs)
 * y'a t'il des fichiers spécifiques aux développeurs ? (fichiers d'IDE par exemple - `.settings`, `.projects`)
 * Y'a t'il des fichiers "historiques" (genre les fichiers nommés `-old.xx` ou `-sauv` ou `-backup`)

## Construction et modularisation du projet

 * Est ce que le projet est constructible facilement ? via convention (Maven par exemple ?)
 * Est ce que le projet est correctement découpé en sous-module ?
 * Est ce qu'il n'est pas sur-découpé ?
 * Est ce que les scope Maven sont correctement utilisés (scope `test` sur les librairies utilisés que pour les test, scope `runtime` pour Lombok).
 * Est ce qu'il y'a une adhérence de librairies / code technique au CDS qui réalise le développement ?
 * Est ce qu'il n'y a pas de duplication de projet ?

## Découpage des sources par projet

 * Est ce que les projets suivent la convention de nommage de package interne : `com.mycompany.application.module.` ?
 * Est ce que l'application est d'abord découpée fonctionnellement, puis techniquement ?
 * Est ce que les modules fonctionnels sont correctement nommés ? explicitement et parlant ?
 * Est ce que les couches techniques `repository`, `domain`, `utils`, `common`, `business` sont présentes ?.

## Convention de nommage

 * Nom des sources et des fonctions/paramètres ? franglais ?
 * Est ce que les nommages sont cohérents ? (un `readXX` est idempotent par exemple)
 * Est ce que les nommages sont auto-documentés ?

## Frameworks et librairies

 * Est ce que les librairies utilisés suivent les préconisations de la stack d'Entreprise ?
 * Est ce que les versions sont à jours (par rapport à la Stack Logicielle Entreprise)?
 * Est ce que les versions sont assurées de ne pas avoir de failles de sécurité connues ?
 * Utilisation de code "utilitaire" - type Lombok - pour simplifier les développements ?
 * utilisatoin de BootDev pour faciliter les développements ?
 * Y'a t'il des créations "maison" de brique techniques qui pourraient être déléguées à des librairies existantes ?

## Conception : Généralité

## DTO / Objets métiers

  * Est ce que les objets métiers utilisent les `HashCode`/`Equals` ?
  * Sont ils implémentés correctement ?  
  * Est ce qu'ils ne contiennent pas de code métier ?

## Bean Métier (Service)

 * Est ce que les Service sont bien stateless ?
 * Est ce que les Service ne se limitent bien qu'a du code métier ?
 * Est ce qu'il n'existe des variables de classes ? (alors qu'il ne devrait pas?)
 * (voir performance) - asynchronisme des traitements
 * Quid du nommage des service métier ?
 * Est ce que l'on privilégie bien la composistion plutot que l'héritage ?
 * Est ce que les parametres d'entrées sont vérifiés avant traitement (nullité et validité). Que ce passe t'il si les valeurs sont hors-bornes ?
 * Comment sont gérées les transactions?
 * Est ce que le code contient des valeurs magiques ? (des valeurs de variables en dur dans le code, sans explication...)  

## Bean d'accès aux données (Repository)

 * Est ce que les Repository sont bien stateless ?
 * Est ce que les Repository se limitent bien à de la récupération d'objtes métiers ?
 * Est ce que les Repository s'appuient sur des librairies facilitant les développements ? Spring Data JPA?
 * Est ce que les getXXX mettent en place un système de pagination ?
 * Comment sont gérées les transactions ?

## Bean Web (Controller) 

  * Quel est le niveau de maturité des API REST ?
  * Est ce que les URL correctement constituées et les verbes correctement implémentés ?
  * Est ce que les objets de retour de Controller sont standards ? Ou est ce qu'il existe des objets "fait maison" pour encapsuler les réponses ? 
  * Est ce qu'un mécanisme de pagination est mis en place ?
  * Est ce qu'un mécanisme de réponse partielle est mis en place ?
  * Est ce que le GZIP est activé par défaut ?

## Spring

  * Est ce que l'injection de composant est correctement faite avec Spring ?
  * Est ce que la définitiion de Bean est faite par annotation plutot que XML ?
  * Est ce que l'injection est faite prioritairement par constructeur ?
  * Est ce que les création de composants sont faites en cohérence avec JavaConfig ?
  * Est ce que les beans sont correctement annotés (@Component, @Service, @Repository, etc.) ?
  * Est ce qu'il n'ya pas d'ambiguité sur les injections de beans ? (nommages correct ?)
  * Vérifier qu'il n'ya pas de surdécoupage des beans (Interface/Implémentation) 

## Erreurs

 * Est ce qu'un système de log est mis en place ?

# FRONTEND

## Construction
  * Y'a t'il un système de construction IHM ?
  * Y'a t'il un pre-processeur CSS ?
  * Est ce que les sources sont minifiées avant d'être servies ?
  * Est ce que les sources sont concaténées avant d'être servies ?

# Programmation réactive  

 * Est ce que le projet met en place des concepts de programmation réactive ?

# Performances

  * Est ce que des systèmes de gestion asynchrones sont mis en place pour assurer la paralléliastion des traitements ?

# Externalisation / Paramétrage

 * Est ce que l'externalisation est correctement faite ?

# Tests unitaires
 * Est ce que des tests unitaires sont mis en place sur le projet ?
 * Sur quelles couches les tests unitaires sont mis en place ?
 * Quel est le niveau de couverture du projet par rapport à l'acceptable ?
 * Quel est le pourcentage de succès des tests unitaires ?

# Tests fonctionnels IHM
 * Est ce que les tests fonctionnels sont mis en place sur le projet ?
 * Quel est le niveau de couverture du projet par rapport à l'acceptable ?
 * Quel est le pourcentage de succès des tests ?
 * Avec quels navigateurs sont ils mis en place ?

# Qualité
En complément de l'analyse Sonar.

 * Taille des fichiers de code ?
 * documentation ?
 * duplication de code ?
 * code mort dans le code ?
 * Est ce que les commentaires sont explicites ? Est ce qu'ils ont de la valeur ajoutée ?

# Evolution par rapport aux Audit précédents ?

 * Est ce que les points remontés lors des audits précédents ont été pris en compte ?
