title: Les scripts NPM et la sécurité
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->


---

class: impact

# {{title}}

---

class: center

# DISCLAIMER

/!\ Je ne suis pas un expert en sécurité /!\

---

# Petit récapitulatif

## Dat "package.json"

Dans tous les projets node :

```
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    // vous dépendez de l'internet
  }
}
```

---

## Les scripts

```
{
  "name": "my-project",
  "scripts": {
    "say-hello": "echo hello",
    "say-goodbye": "echo goodbye"
  }
}
```

Pour faire tourner un script :

```
npm run say-hello
```

---

## Les lifecycle scripts

- à l'installation : `[pre|post]install`
- à la publication : `[pre|post]publish`

- l'execution est automatique par défaut :)

---

## "[pre|post]install"

### Installation d'une dépendance npm :

- npm télécharge la dernière version de yolo
- execute le script `preinstall` si il y en a un
- installe les dépendances suivant toutes les étapes de cette liste
- execute le script `install` ou `postinstall` si il y en a

---

class: center

## What could go wrong

<img src="https://media.giphy.com/media/rVbAzUUSUC6dO/giphy.gif" />

---

## Idée 1 - Modifier curl

- créer un alias `curl` vers notre script

```json
{
	"bin": {
		"curl": "./my-custom-curl.js"
	}
}
```

- faire tout ce qu'on veut

- appeler vraiment `curl`


---

## Idée 2 - Remote terminal via socket

Le script de `postinstall` va :

- exécuter un client socket vers un serveur qu'on contrôle
- pour des messages spécifiques, le client executera la commande envoyée et remontera les résultats
- c'est tout :)

---

## Idée 3 - Self-replicating script

- `npm whoami`
- scraping de la page de l'utilisateur sur le site de npm
- pour tous les modules :
  - `npm install`
  - `cd my-module/`
  - `npm version patch`
  - ajouter le script dans `package.json`
  - copier le script malicieux
  - `npm publish`
- supprimer toutes traces du script malicieux

---

class: center

## Give me solutions

<img src="https://media.giphy.com/media/LaYIKCZE7aC8o/giphy.gif" />

---

## Précaution 1

Désactiver l'exécution automatique des scripts :

`npm install --ignore-scripts <l'internet>`

ou `npm config set ignore-scripts true`

Inconvénient :
- on casse le bon fonctionnement de certains packages (PhantomJS ou autre).

---

## Précaution 2

NE PAS FAIRE D'INSTALLATION EN ROOT, aka `sudo npm install ...`.

Pourquoi ?

Les scripts sont exécutés avec les permissions de l'utilisateur courant.

---

## Précaution 3

Auditer TOUTES les dépendances du projet à la main (il suffit qu'un seul module soit impacté).

Inconvénient :
- nombre de dépendances transitives conséquent

Auditer toutes les dépendances du projet automatiquement ?
- JavaScript est très permissif
- analyse de code difficile
- beaucoup de faux négatifs

---

## Précaution 4

Depuis la version 6 :

`npm audit`

Inconvénient :
- potentiel délai avant qu'une faille soit déclarée

---

## Précaution 5

Dans le doute : `npm logout`

---

- signature des modules ?
  - https://github.com/npm/npm/issues/8489
  - https://github.com/node-forward/discussions/issues/29

---

## Conclusion

- une typo arrive vite (`npm install bable`)

- Le problème n'est pas récent (2016) :

https://www.kb.cert.org/CERT_WEB/services/vul-notes.nsf/6eacfaeab94596f5852569290066a50b/018dbb99def6980185257f820013f175/$FILE/npmwormdisclosure.pdf

- Le problème n'est pas trivial à résoudre

- Les exploits sont assez fun à écrire :)

---

class: center

## Questions

<img src="https://media.giphy.com/media/l3vQWH3WGT1xEWKwU/giphy.gif" />
