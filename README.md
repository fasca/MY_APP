# Apprendre Ruby on Rails pas à pas

Ce document accompagne un débutant total pour comprendre **pourquoi** et **comment** créer une application Ruby on Rails. Chaque étape est expliquée en détail afin d'offrir le contexte et la logique derrière chaque commande.

## 1. Qu'est‑ce que Ruby on Rails ?

Ruby on Rails (souvent appelé *Rails*) est un framework web écrit en Ruby. Il propose une structure qui simplifie la création d'applications en suivant le modèle **MVC** (Modèle–Vue–Contrôleur). Rails s'occupe de nombreuses tâches répétitives afin que vous puissiez vous concentrer sur la logique de votre application.

## 2. Préparer son environnement

1. Installez **Ruby** (version 2.7 ou plus) à l'aide de `rbenv` ou `rvm`. Ruby est le langage dans lequel Rails est écrit.
2. Mettez à jour l'outil `gem` :
   ```bash
   gem update --system
   ```
3. Installez **Rails** :
   ```bash
   gem install rails
   ```
4. Vérifiez que tout fonctionne :
   ```bash
   rails --version
   ```

## 3. Créer un nouveau projet

Dans un dossier de travail, lancez :

```bash
rails new blog
cd blog
```

L'option `-d postgresql` permet d'utiliser PostgreSQL comme base de données (par défaut, c'est SQLite). Choisissez celle qui vous convient le mieux.

## 4. Explorer la structure du projet

Quelques dossiers importants :

- `app/` : contient les **modèles**, **vues** et **contrôleurs**. C'est le cœur de l'application.
- `config/` : paramètres et fichier `routes.rb` pour définir l'URL de chaque page.
- `db/` : migrations et schéma de la base de données.
- `Gemfile` : liste des bibliothèques Ruby utilisées par le projet.

Prenez le temps de parcourir ces dossiers. Comprendre leur rôle facilite la suite.

## 5. Comprendre le modèle MVC

Rails suit l'architecture **Modèle–Vue–Contrôleur** :

- **Modèle** : représente les données (ex. un article). Il correspond à une table dans la base. Le modèle contient aussi la logique pour valider et manipuler ces données.
- **Vue** : gère l'affichage (HTML, CSS) que voit l'utilisateur.
- **Contrôleur** : reçoit les requêtes web, demande au modèle les informations nécessaires, puis choisit la vue à afficher.

Cette séparation permet de mieux organiser le code. On commence souvent par définir un modèle car les données sont la base de toute application.

## 6. Créer sa première ressource

Imaginons que l'on souhaite gérer des articles de blog. On génère une ressource complète (*scaffold*) pour obtenir un exemple fonctionnel :

```bash
rails generate scaffold Article title:string body:text
rails db:migrate
```

- `rails generate scaffold` crée un **modèle**, un **contrôleur** et des **vues** pour l'objet `Article`.
- `rails db:migrate` applique la migration générée afin de créer la table `articles` en base de données.

### Pourquoi commencer par le modèle ?

Le modèle décrit la structure des données (titres, contenus, etc.). En partant de cette structure, Rails sait ensuite générer le reste (contrôleurs, vues) de façon cohérente. Les données étant au cœur du projet, il est logique de les définir en premier.

Démarrez ensuite le serveur pour voir le résultat :

```bash
rails server
```

Rendez‑vous sur `http://localhost:3000/articles` pour accéder à l'interface créée automatiquement.

## 7. Les routes

Les routes font le lien entre les URL et les actions de vos contrôleurs. Pour les lister :

```bash
rails routes
```

Définissons une page d'accueil qui affiche la liste des articles :

```ruby
# config/routes.rb
root "articles#index"
```

## 8. Valider et associer les modèles

Dans `app/models/article.rb`, on peut ajouter des règles pour s'assurer que chaque article possède un titre et un contenu suffisamment long :

```ruby
class Article < ApplicationRecord
  validates :title, presence: true
  validates :body, length: { minimum: 10 }
end
```

Pour lier un article à un auteur :

```ruby
class Article < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
  has_many :articles
end
```

Après avoir créé les migrations nécessaires, exécutez :

```bash
rails db:migrate
```

## 9. Contrôleurs et vues

Les contrôleurs contiennent la logique qui prépare les données avant de les afficher. Les vues se trouvent dans `app/views`. Rails vous fournit déjà des vues générées par le scaffold, que vous pouvez personnaliser.

Les layouts communs (en‑tête, pied de page) sont stockés dans `app/views/layouts`.

## 10. Migrations et gestion de la base

Si vous souhaitez ajouter une colonne `published_at` à vos articles :

```bash
rails generate migration AddPublishedAtToArticles published_at:datetime
rails db:migrate
```

Vous pouvez revenir en arrière avec `rails db:rollback` si nécessaire.

## 11. Tester son application

Rails intègre Minitest. Pour lancer tous les tests :

```bash
rails test
```

Pour des tests plus avancés, vous pouvez installer RSpec en l'ajoutant dans le `Gemfile`.

## 12. Fonctionnalités avancées

- **Action Mailer** : envoie des e-mails (confirmation d'inscription, etc.).
- **Active Job** : exécute des tâches en arrière‑plan.
- **Action Cable** : gère les websockets pour des mises à jour en temps réel.
- **Active Storage** : permet d'attacher des fichiers (images, documents) à vos modèles.

## 13. Déployer son application

Voici un exemple de déploiement sur Heroku :

```bash
heroku create
git push heroku main
heroku run rails db:migrate
```

N'oubliez pas de configurer vos variables d'environnement (clefs secrètes, base de données) avant la mise en production.

## 14. Aller plus loin

La [documentation officielle](https://guides.rubyonrails.org/) regorge de guides pour approfondir tous les sujets : sécurité, performances, API, tests, etc.

Ce tutoriel doit vous donner une vue d'ensemble claire de Rails, en expliquant le rôle de chaque élément. En maîtrisant ces bases, vous pourrez créer vos propres applications avec assurance.
