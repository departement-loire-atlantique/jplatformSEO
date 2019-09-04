# jPlatformSEO

Ce module améliore les fonctionnalités SEO pour JPlatform10.

Liste des fonctionnalités en bref :

* Plan du site personnalisable
* Fichier robots.txt personnalisé
* Optimisation / réécriture des URLS
* Outil d'aide à la contribution SEO
* Balises META pour les réseaux sociaux
* Pages d'erreur 404 et 403 personnalisées
* Page site en maintenance
* Personnalisation du libellé "jcms" pour l'URL de JCMS
* Personnalisation du titre de la page ?
* Marqueur UniversalAnalytics avec variables personnalisées

## Plan du site personnalisable
### Plan du site pour les internaute

Fichier JSP plugins/SEOPlugin/jsp/sitemap.jsp

Réalise automatiquement un plan du site à 3 niveaux basé sur une liste de catégories en ignorant certaines catégories.

Configuration :

* La propriété *fr.cg44.plugin.seo.sitemap.cats* permet de lister les catégories racines (Liste d'ID de catégories)

MAJ : on utilisera finalement une porlet Navigation avancée qui permettra de saisir les différentes catégories racine.
Cette portlet permettra aussi de paramétrer le nombre de niveaux.
* La propriété *fr.cg44.plugin.seo.sitemap.stopcats* permet de lister les catégories filles à ignorer (Liste d'ID de catégories)

MAJ : vu avec Jérémie, plus utile. On affiche tout.

### Plan du site pour les robots

Fichier JSP sitemap.jsp à mettre à la racine du site.

Génération automatique d'un fichier sitemap.xml à déclarer dans google search console pour le site.

Le module sitemap respectera les droits sur les catégories et les contenus (Module CategoryRight et natif JCMS).

Configuration :

* La propriété *fr.cg44.plugin.seo.sitemapxml.type.blacklist* permet d'ignorer des types de contenu (Liste de chaine de caractères)
* La propriété *fr.cg44.plugin.seo.sitemapxml.mime.blacklist* permet d'ignorer certains documents relativement à leur types MIMES


## Fil d'ariane personnalisé et configurable

Le fil d'ariane se base sur les catégories du contenu actuellement affiché sur le site.
Une propriété permettra de préciser la branche de catégorie utilisée pour la navigation.
    
et un gabarit de **Portlet navigation** :

```
plugins/SEOPlugin/types/PortletNavigate/breadcrumb.jsp
```

Configuration :

* La propriété *fr.cg44.plugin.seo.breadcrumb.navcat* permet de choisir la branche de navigation pour la navigation

## Fichier robots.txt personnalisé

La configuration de ce fichier permet de concentrer l'indexation des moteurs de recherche sur les contenus HTML du site.

Voir fichier : /robots.txt

## Optimisation / réécriture des URLS

Afin d'optimiser le référencement des contenus, le titre de la page dans l'URL ainsi que la méta donnée **description** seront configurables.

Cette fonctionnalité repose sur deux extra pour toutes les catégories, les portlets et les publications :

* Titre (URL page et META)
* Description (META)


Et une extra data supplémentaire dédié aux catégories (Valeur par défaut si non renseigné : non) :

- Affichée oui/non dans l'URL

Il faut aussi pouvoir renseigner une balise "meta robots" manuellement sur chaque page afin de maitriser parfaitement l'indexation  : 
* ```<meta name="robots" content="noindex,nofollow" />```

Les pages du sites seront soit une catégorie, soit une publication.

Nous utiliserons des extradatas pour insérer ces champs. Afin de ne pas surcharger le store, nous mettrons ces extradata sur certains types de contenus uniquement via une propriété indiquant les types de contenus concernés (ex : inutile sur adresses collèges).


Donner également la possibilité de renseigner une balise "rel canonical" sur chaque page de type éviterait également la duplication : 
* ```<link rel="canonical" href="http://www.monsite.com/url-canonique.html"/>``` 

 
Une personnalisation automatique des propriétés JCMS relativement aux URLs des pages : descriptive-urls.*

Format URL contenu : /departement44/première_catégorie_autorisée_trouvée/titre/id

Format URL membre : /departement44/première_catégorie_autorisée_trouvée/titre/id

Format URL category : /departement44/première_catégorie_autorisée_trouvée/titre/id

Format URL autre : /departement44/première_catégorie_autorisée_trouvée/titre/id

**IMPORTANT : si un contenu est rattaché à plusieurs thématiques, la première catégorie autorisée trouvée est aléatoire et dépend d'un algorithme propre à jPlatform.**

Un PortalPolicyFilter permettant :

* de calculer la syntaxe de l'URL à afficher.
* d'ajouter automatiquement dans la page les META titre et description adaptée
* d'ajouter automatiquement dans la page la META last-modified

## Outil d'aide à la contribution SEO

Tableau de bord en back office permettant de voir les valeurs des propriétés clés du module ainsi que la liste des catégories pour lesquelles une personnalisation extradata a été configurée.
       
## Balises META pour les réseaux sociaux

- A préciser : qu'est ce qui dynamique, qu'est qui est lié à une propriété - 

Exemple :
```
<meta property="og:site_name" content="Loire-atlantique.fr" />
<meta property="og:url" content="https://www.loire-atlantique.fr/jcms/services-fr-c_5026" />
<meta property="og:type" content="website" />
<meta property="og:title" content="Informations pratiques et services en ligne" />
<meta property="og:description" content="loire-atlantique.fr : services, contacts, aides et informations pratiques ciblés pour l&apos;enfance, la famille, la jeunesse, les personnes âgées, les personnes en situation de handicap, l&apos;éducation, le RSA, l&apos;insertion, l&apos;emploi, l&apos;économie, l&apos;environnement, la culture, le sport, le logement, le tourisme, les aides aux territoires, les subventions départementales.
Retrouvez aussi toutes les informations pratiques pour votre quotidien  : infotrafic, horaires des transports LILA , agenda des loisirs et des événements incontournables en Loire-Atlantique. " />
<meta property="og:image" content="https://www.loire-atlantique.fr/upload/docs/image/png/2015-04/depla_site_favicon_196x196.png" />
<meta name="twitter:card" content="summary" />
<meta name="twitter:site" content="loireatlantique" />
```
 
## Pages d'erreur 404 et 403 personnalisées

Deux gabarits de pages statiques et un PortalPolicyFilter permettent d'afficher des pages personnalisés en cas d'URL introuvable dans JCMS. Lors de l'accès à une ressource protégée, une page personnaliser apparait permettant de revenir à l'accueil ou de s'authentifier (code d'erreur 403 pour éviter l'indexation par Google).

Ces pages sont également configurée dans apache (conf à préciser) afin de fonctionner aussi pour des URLs non prises en charges dans JCMS.

Exemple (hors jcms) : https://www.loire-atlantique.fr/nexitepas
Exemple (jcms) : https://www.loire-atlantique.fr/jcms/nexitepas


## Marqueur UniversalAnalytics avec variables personnalisées

Si la propriété *fr.cg44.plugin.seo.analytics* est renseignée avec le code du marqueur analytics du site, alors le marqueur UniversalAnalytics est automatiquement intégré dans toutes les pages du site.

Si la propriété *fr.cg44.plugin.seo.google-site-verification* est renseignée, alors la META suivante est automatiquement intégré dans toutes les pages du site :

```
<meta name="google-site-verification" content="fNMrbG81fIcmTXv-vSbSPmaJYs3Jt1cux6YSx7uNoUQ" />
```

Listes des variables personnalisées :

- ariane catégorie : id de la catégorie / id de la catégorie / ...
- ariane nommée : nom de la catégorie / nom de la catégorie / ...
- espace de travail : nom

Configuration :

* La propriété *fr.cg44.plugin.seo.analyticscats* permet de choisir les branches de navigation pour le calcul des niveaux des variables personnalisées (la première catégorie qui correspond à une catégorie du contenu est utilisée)


## Installation de ce module

Attention : conséquence sur les URL, penser à mettre en place un plan de redirection des URLs