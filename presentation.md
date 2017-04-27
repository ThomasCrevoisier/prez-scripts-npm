title: Les scripts NPM ...--yolo
class: animation-fade
layout: true

<!-- This slide will serve as the base layout for all your slides -->


---

class: impact

# {{title}}

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

TL;DR : installation d'une dépendance npm

- npm télécharge la dernière version de yolo
- execute le script `preinstall` si il y en a un
- installe les dépendances suivant toutes les étapes de cette liste
- execute le script `install` ou `postinstall` si il y en a

---

## Time to break things

<img src="https://media.giphy.com/media/rVbAzUUSUC6dO/giphy.gif" />

---

## Démo 1 - Modifier curl

TL;DR : Notre module malicieux va executer un script après son installation qui va :

- créer un alias `curl` vers notre script
- do whatever you want
- appeler vraiment `curl`

---

## Démo 2 - Remote terminal via socket

Notre script de `postinstall` va :

- exécuter un client socket
- pour des messages spécifiques, le client executera la commande envoyée
- c'est tout :)

---

## Démo 3 - Self-replicating script

- `npm whoami`
- scraping de la page de l'utilisateur sur le site de npm
- pour tous les modules :
  - `npm install`
  - `cd my-module/`
  - `npm version patch`
  - ajouter le script dans `package.json`
  - copier le script malicieux
  - `npm publish`

---

## Give me solutions

<img src="https://media.giphy.com/media/LaYIKCZE7aC8o/giphy.gif" />

---

## Give me solutions

- `npm install --ignore-scripts <l'internet>`
- `npm config set ignore-scripts true`
- signature des modules ?
  - https://github.com/npm/npm/issues/8489
  - https://github.com/node-forward/discussions/issues/29
- DONT EXECUTE ARBITRARY CODE BY DEFAULT ANYONE ?

Just saying : 
- une typo arrive vite
- https://www.kb.cert.org/CERT_WEB/services/vul-notes.nsf/6eacfaeab94596f5852569290066a50b/018dbb99def6980185257f820013f175/$FILE/npmwormdisclosure.pdf

---

## Questions

<img src="https://media.giphy.com/media/l3vQWH3WGT1xEWKwU/giphy.gif" />
