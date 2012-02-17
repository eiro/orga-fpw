# installer git 

sur les debian derivative: aptitude install git-core

sur arch, il me semble que c'etait pacman -Syu git

sur les autres je sais pas :)


# la premiere fois


Avoir une version locale des documents

git clone git@github.com:eiro/jp12orga.git



Avoir une version locale des documents

git clone git@github.com:eiro/jp12orga.git

Entrer dans le repertoire de travail

cd jp12orga

Editer un fichier 

vim README.md
git add README.md

Expliquer en quoi la modification a consisté

git commit -m "J'ai mis des choses dedans"

Partager une nouvelle version de travail avec les autres membres

git push

# les fois suivantes

c'est la meme sauf que la premiere ligne, celle avec git clone, devient 

git push
