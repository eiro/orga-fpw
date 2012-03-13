# Un guide pour éditer le site web

## 2 sites : test et prod

### Le site de test

Le site de test est une série de fichiers qui se trouve sur le serveur spectre.
On les modifie en se connectant sur le serveur ssh avec authentification par clés.
L'accès est à demander à Maddingue (il est sur #perlfr de irc.perl.org)

Les mises à jour sont visibles dès la sauvegarde du fichier concerné (et un refresh du browser :-) ) sur http://test.mongueurs.net/fpw2012

### Le site de prod

Le site de prod est accessible via un dépôt Subversion. Le commit bit est à demander à Maddingue.

Pour initialiser sa zône de travail :

 - cd /ma/zone/de/travail
 - svn checkout svn://svn.mongueurs.net/fpw2012

Pour éditer des fichiers :

 - vi, emacs (ou ses variantes), kwrite, kate, eclipse... ce que vous voulez

Pour remonter ses modif au server :

 - svn commit

Pour mettre à jour sa zône de travail : 

 - cd /ma/zone/de/travail/fpw2012
 - svn update

Le site est republié toutes les 5 minutes.

### Interactions tests / prod

Au commencent, seul était le site de test.

Puis, quand on lui demande, Maddingue passe le site de test en prod. Je n'ai pas
les détails, mais en gros :

 - le site de test reste accessible
 - le dépôt fpw2012 est créé et est disponible
 - http://journeesperl.fr/2012 est visible, et les alias sont mis à jour
 - seuls les fichiers sont transmis, pas la DB : pas le wiki, pas les news, pas les inscriptions, pas les talks soumis

Ensuite, le site de test et le site de prod vivent leurs vies de leurs côtés.
Il n'y a pas de synchro autre qu'en recopiant les fichiers de l'un vers l'autre
à la main.

On peut imaginer avoir dans un premier temps un site de prod minimal (date, lieu,
inscriptions ouvertes, soumissions ouvertes, design avec une marge d'amélioration
évidente) et d'affiner le look sur le site de test pour basculer un peu plus tard
sur un design abouti.


## Le principe du site

Le site est géré par Act (Another Conference Tool ou un truc comme ça).
Act s'appuie sur une DB (je sais pas laquelle, je sais pas comment) pour fournir une
gestion des inscriptions à la conférence, une gestion des présentations (soumission, 
approbation, planification...), un wiki, un système de news, des rôles 
(administrateur du site, des users, du wioki...) et des pages plus 
statiques, avec une gestion multilingue.
Pour la gestion des pages, Act utilise Template::Toolkit (ou Template::Toolkit2) :
ça permet d'avoir les menus dans un fichier, le corps des pages dans un autre fichier
et à la publication, le truc assemble les 2 pour faire les pages consultables par
tout un chacun. Cela permet aussi de créer des templates, et certainement des tas
d'autres trucs qu'on n'a pas utilisé pour les JP depuis des lustres.

Les fichiers du sites sont dispatchés dans 2 répertoires : actdocs et wwwdocs.

Dans wwwdocs, on met les fichiers qui seront utilisés tels quels à la publication,
typiquement les CSS, les images.

Dans actdocs, on trouver le fichier de config du site (ne pas toucher "juste pour voir"),
les templates, les pages statiques (avec du TT dedans)


## 2 fichiers "points d'entrée"

### actdocs/conf/act.ini

C'est le fichier de conf.

C'est là qu'on dit on peut inscrire, si on peut soumettre, combien il y a de places,
combien de salles, combien de langues pour le site web et lesquelles, combien ça coûte...

C'est du :

 [section]
 clé = valeur.

### actdocs/templates/ui

C'est le point d'entrée HTML, enfin, tel qu'on utilise le site aujourd'hui.

C'est lui qui décrit (en TT) comment les pages (de actdocs seulement) se présentent
(en haut, y a le logo de la conférence, puis tels menus, puis le corps, à droite y a
d'autres menus, en bas les logos des sponsors... par exemple).

## Les pages du site

### Les pages 'statiques'

Elles ne sont pas éditables depuis le site web lui-même. Il faut passer par ssh 
(site de test) ou svn (site de prod).

Dans actdocs/static se trouvent une série de .html qui sont le corps des pages statiques
du site.

Ces pages commencent par appeler le template qui va être utilisé (ui) :

 [% WRAPPER ui title = global.conference.name %]

Ensuite, c'est du HTML.

Et on finit par :

 [% END %]


Les pages publiées sont construites en appliquant ce corps dans le template précisé (ui chez nous).

La gestion multilingue se fait avec les balises <t>.
Disons que dans act.ini, on ait défini 2 langues : fr et en.
On aura dans les .html et les templates des trucs du genre :

 <t>
  <fr>Mon texte en français</fr>
  <en>The same text in English</en>
 </t>


On n'utilise pas wwwdocs/ pour y mettre des pages (je ne dis ni que ce n'est pas faisable,
ni que ce n'est pas à faire, juste qu'on ne l'a pas fait).


### Les pages d'inscription, de création de login

Le login est réutilisable sur toutes les conf Act hébergées par Act (et il y en a
pas mal).

On n'a pas la main sur la page. Elle est entièrement gérée par Act.

Ce qui est modifiable sont les infos de l'utilisateur, que lui-même met à jour
en ligne.


### La partie Utilisateurs, Recherche

Même topo : gérées par Act.

### Présentations

Là encore, c'est géré par Act.

Pour soumettre, il faut être inscrit à la conférence et que les soumissions 
aient été autorisées (via un parms de act.ini)

### News

Gérées par Act.

Un user avec 'admin news' peut créer une news (qui est publiée ou à publier), publier
une news, corriger une news publiée ou pas publiée...

### Wiki

Géré par Act.

Un user avec 'admin wiki' pourra créer des pages.

Un user inscrit pourrait modifier les pages existantes.



Allez, je m'arrête là (las).


