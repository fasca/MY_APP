# Tutoriel complet Ruby on Rails

Ce guide explique pas à pas comment installer Ruby on Rails et créer une application basique.

## 1. Installation de Ruby et Rails

1. Installez Ruby (>= 2.7) via `rbenv` ou `rvm`.
2. Installez Rails avec `gem install rails`.
3. Vérifiez la version : `rails --version`.

## 2. Création d'une application

```bash
rails new blog
cd blog
```

Cette commande génère l'arborescence de l'application.

## 3. Structure d'un projet Rails

- `app/` : contient les modèles, vues et contrôleurs (MVC).
- `config/` : fichiers de configuration, routes, base de données.
- `db/` : migrations et schéma de base de données.

## 4. Génération de ressources

Créez un modèle Article avec contrôleur et vues :

```bash
rails generate scaffold Article title:string body:text
rails db:migrate
```

Lancez le serveur :

```bash
rails server
```

Ouvrez `http://localhost:3000/articles` pour voir l'interface CRUD.

## 5. Migrations et base de données

Les migrations décrivent les changements de schéma :

```bash
rails generate migration AddAuthorToArticles author:string
rails db:migrate
```

## 6. Tests

Rails propose différents frameworks de test (Minitest, RSpec). Exemple avec Minitest :

```bash
rails test
```

## 7. Déploiement

Pour déployer sur Heroku :

```bash
heroku create
git push heroku main
rails db:migrate
```

Ce tutoriel fournit les bases pour bien démarrer avec Ruby on Rails. Consultez la [documentation officielle](https://guides.rubyonrails.org/) pour aller plus loin.
