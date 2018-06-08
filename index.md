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

**&#9888; Je ne suis pas un expert en sécurité &#9888;**

<img src="https://media.giphy.com/media/KQm5O05y9rzQA/giphy.gif" />

---

# Petit récapitulatif

## Dat "package.json"

Dans tous les projets node :

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
	"yolo": "^1.2.3",
	"add": "~3.2.4",

    // le reste de l'internet
  }
}
```

---

## Les scripts

```json
{
  "name": "my-project",
  "scripts": {
    "say-hello": "echo hello",
    "say-goodbye": "echo goodbye"
  }
}
```

Pour faire tourner un script :

```bash
npm run say-hello
```

---

## Les lifecycle scripts

- à l'installation : `[pre|post]install`
- à la publication : `[pre|post]publish`

- l'execution est automatique par défaut :)

---

## "[pre|post]install"

### Installation d'une dépendance npm `yolo` :

- `npm install yolo`

- npm télécharge la dernière version de `yolo`

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

## Idée 1 (bis)

```json
{
	"name": "erase-npm",
	"bin": {
		"npm": "./not-so-nice-npm.js"
	}
}
```

--
<img src="https://media.giphy.com/media/Ll2fajzk9DgaY/giphy.gif" />

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

NOTE :

- mitigé par le `package-lock.json`

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

&#9888; NE PAS FAIRE D'INSTALLATION EN ROOT &#9888;

_( aka `sudo npm install ...` )_

<br />
<br />
<br />

--
**Pourquoi ?**

Les scripts sont exécutés avec les permissions de l'utilisateur courant.

```json
{
	"scripts": {
		"postinstall": "rm -rf /"
	}
}
```

---

## Précaution 3

Auditer TOUTES les dépendances du projet à la main (il suffit qu'un seul module soit impacté).

--

**Inconvénient :**

- nombre de dépendances transitives conséquent

_Auditer toutes les dépendances du projet automatiquement ?_

- JavaScript est très permissif

- analyse de code difficile

- beaucoup de faux négatifs

---

## Précaution 4

Depuis la version 6 :

`npm audit`

--

**Inconvénient :**

- potentiel délai avant qu'une faille soit déclarée

- délai encore plus long pour que ça se propage dans toutes vos dépendances

---

## Précaution 5

Dans le doute : `npm logout`

<img src="https://media.giphy.com/media/HteV6g0QTNxp6/giphy-downsized-large.gif" />

---

## Autre piste potentielle

Signature des modules ?

- https://github.com/npm/npm/issues/8489

- https://github.com/node-forward/discussions/issues/29

---

## Conclusion

- "Les utilisateurs installent les packages les plus populaires" (ou pas)

- une typo arrive vite (`npm install bable`)

- Le problème n'est pas récent (2016) :

https://www.kb.cert.org/CERT_WEB/services/vul-notes.nsf/6eacfaeab94596f5852569290066a50b/018dbb99def6980185257f820013f175/$FILE/npmwormdisclosure.pdf

- Le problème n'est pas trivial à résoudre

- Les exploits sont assez fun à écrire :)

---

class: center fretlink

## <img src="img/fretlink.svg" /> recrute

_( React, NodeJS, PureScript, Haskell, ...)_

<img src="https://media.giphy.com/media/3d78lX84bkU6T4zNOg/giphy.gif" />

<a href="https://www.fretlink.com/join-us">https://www.fretlink.com/join-us</a>

---

class: center

## Des questions ?

<img src="https://media.giphy.com/media/l3vQWH3WGT1xEWKwU/giphy.gif" />
