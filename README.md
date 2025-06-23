# Tutoriel détaillé pour maîtriser Ruby on Rails

Ce guide explique pas à pas comment installer, configurer et utiliser Ruby on Rails afin d'en maîtriser les fonctionnalités principales. Les étapes ci-dessous doivent être suivies dans l'ordre pour progresser efficacement.

## Étape 1 : Installation de Ruby et de Rails

1. Installez **Ruby** (>= 2.7) via `rbenv` ou `rvm`.
2. Mettez à jour `gem` : `gem update --system`.
3. Installez **Rails** avec `gem install rails`.
4. Vérifiez l'installation : `rails --version`.

## Étape 2 : Création d'une nouvelle application

Créez votre projet dans un dossier dédié :

```bash
rails new blog
cd blog
```

Ajoutez l'option `-d postgresql` pour utiliser PostgreSQL si nécessaire.

## Étape 3 : Structure du projet

- `app/` : modèles, vues et contrôleurs (MVC).
- `config/` : configuration et routes.
- `db/` : migrations et schéma de base.
- `test/` ou `spec/` : tests automatisés.
- `Gemfile` : dépendances Ruby.

Familiarisez-vous avec ces dossiers avant de continuer.

## Étape 4 : Génération d'une ressource

Créons un modèle `Article` et ses vues CRUD :

```bash
rails generate scaffold Article title:string body:text
rails db:migrate
```

Lancez ensuite le serveur :

```bash
rails server
```

Rendez-vous sur `http://localhost:3000/articles` pour tester l'interface.

## Étape 5 : Gestion des routes

Les routes sont définies dans `config/routes.rb`. Pour les lister :

```bash
rails routes
```

Définissez une page d'accueil :

```ruby
root "articles#index"
```

## Étape 6 : Modèles, validations et associations

Les modèles héritent d'`ApplicationRecord`. Exemple :

```ruby
class Article < ApplicationRecord
  validates :title, presence: true
  validates :body, length: { minimum: 10 }
end
```

Pour créer une relation avec un auteur :

```ruby
class Article < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
  has_many :articles
end
```

Exécutez `rails db:migrate` pour mettre à jour la base.

## Étape 7 : Contrôleurs et vues

Les contrôleurs gèrent la logique métier, les vues affichent les données. Les layouts se trouvent dans `app/views/layouts`.

## Étape 8 : Migrations et base de données

Pour ajouter un champ :

```bash
rails generate migration AddPublishedAtToArticles published_at:datetime
rails db:migrate
```

Revenez en arrière avec `rails db:rollback`.

## Étape 9 : Tests automatisés

Lancez les tests avec Minitest :

```bash
rails test
```

Vous pouvez installer RSpec via le `Gemfile` pour des tests plus avancés.

## Étape 10 : Fonctionnalités avancées

- **Action Mailer** : envoi d'e-mails.
- **Active Job** : tâches en arrière-plan.
- **Action Cable** : websockets.
- **Active Storage** : fichiers uploadés.

## Étape 11 : Déploiement

Exemple de déploiement sur Heroku :

```bash
heroku create
git push heroku main
heroku run rails db:migrate
```

Configurez vos variables d'environnement avant la mise en production.

## Étape 12 : Aller plus loin

Consultez la [documentation officielle](https://guides.rubyonrails.org/) pour approfondir : performances, sécurité, API, etc.

Ce tutoriel doit vous permettre d'acquérir une compréhension solide de Rails et de développer vos propres applications avec confiance.
