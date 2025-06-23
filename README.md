# Apprendre Ruby on Rails pas à pas

Ce guide est destiné aux grands débutants. Nous allons voir **pourquoi** et **comment** chaque étape fonctionne, afin que vous puissiez suivre sans connaissances préalables.

## 1. Qu'est‑ce que Ruby on Rails ?

Ruby on Rails (ou simplement *Rails*) est un framework pour construire des sites web en Ruby. Pensez-y comme à un kit de construction : il fournit une structure et des outils pour aller plus vite. Rails s'appuie sur le modèle **MVC** (Modèle–Vue–Contrôleur) pour organiser le code.

## 2. Préparer son environnement

1. **Installer Ruby**
   - Sur macOS ou Linux : installez `rbenv` ou `rvm`, puis choisissez une version récente, par exemple :
     ```bash
     rbenv install 3.1.2
     rbenv global 3.1.2
     ```
   - Sur Windows : utilisez [RubyInstaller](https://rubyinstaller.org/).
2. **Mettre à jour `gem`** (le gestionnaire de bibliothèques Ruby) :
   ```bash
   gem update --system
   ```
3. **Installer Rails** :
   ```bash
   gem install rails
   ```
4. **Vérifier vos versions** :
   ```bash
   ruby --version
   rails --version
   ```
5. **Base de données** : SQLite est fourni par défaut, mais vous pouvez installer PostgreSQL ou MySQL si vous préférez.
6. **Node.js et Yarn** pour le JavaScript moderne :
   ```bash
   # Exemple sous Ubuntu
   sudo apt-get install nodejs yarn
   ```

## 3. Créer un nouveau projet

Dans votre terminal, placez-vous dans le dossier souhaité et lancez :

```bash
rails new mon_blog
cd mon_blog
```

Cette commande génère toute l'ossature de l'application. C'est comme déballer un kit prêt à l'emploi.

> Astuce : ajoutez `-d postgresql` pour utiliser PostgreSQL plutôt que SQLite.

## 4. Explorer la structure du projet

Les dossiers clés créés sont :

- **`app/`** : tout votre code (modèles, vues, contrôleurs).
- **`config/`** : réglages de l'application et fichier `routes.rb`.
- **`db/`** : migrations et schéma de la base de données.
- **`Gemfile`** : liste des bibliothèques utilisées.

Prenez le temps de parcourir ces dossiers pour vous familiariser avec leur rôle.

## 5. Comprendre le modèle MVC

Rails sépare votre application en trois parties :

- **Modèle (Model)** : représente les données et gère la logique d'accès à la base.
- **Vue (View)** : produit les pages HTML vues par l'utilisateur.
- **Contrôleur (Controller)** : fait le lien entre les deux. Il reçoit la requête, demande au modèle, puis rend la vue.

Cette séparation évite de mélanger les responsabilités. Les données (modèle) sont souvent définies en premier car tout en dépend.

## 6. Créer sa première ressource

Pour gérer des articles de blog, générons une structure complète (*scaffold*) :

```bash
rails generate scaffold Article title:string body:text
rails db:migrate
```

- La première commande crée le modèle, le contrôleur et les vues pour `Article`.
- La seconde applique la migration et crée la table `articles` en base.

### Pourquoi commencer par le modèle ?

Le modèle définit la forme de vos données (titre, corps). En partant de là, Rails peut générer automatiquement les autres fichiers cohérents.

Démarrez ensuite le serveur :

```bash
rails server
```

Rendez-vous sur `http://localhost:3000/articles` pour voir le résultat.

## 7. Les routes

Les routes associent une URL à une action de contrôleur. Pour les lister :

```bash
rails routes
```

Pour définir une page d'accueil qui affiche vos articles :

```ruby
# config/routes.rb
root "articles#index"
```

Rails saura ainsi que visiter `/` déclenche l'action `index` du contrôleur `Articles`.

## 8. Valider et associer les modèles

Dans `app/models/article.rb`, ajoutez des règles :

```ruby
class Article < ApplicationRecord
  validates :title, presence: true
  validates :body, length: { minimum: 10 }
end
```

Rails refusera désormais d'enregistrer un article vide ou trop court.

Pour lier un article à un auteur :

```ruby
class Article < ApplicationRecord
  belongs_to :author
end

class Author < ApplicationRecord
  has_many :articles
end
```

Créez le modèle `Author` et lancez `rails db:migrate` pour mettre à jour la base.

## 9. Contrôleurs pas à pas

Un contrôleur regroupe plusieurs actions. Pour en créer un simplement :

```bash
rails generate controller Welcome index
```

Cette commande crée `welcome_controller.rb` et la vue associée `index.html.erb`. Une action typique :

```ruby
class WelcomeController < ApplicationController
  def index
    @articles = Article.all
  end
end
```

La variable `@articles` est transmise à la vue pour pouvoir afficher la liste.

## 10. Vues et mise en page

Les vues résident dans `app/views`. Chaque action possède généralement un fichier `.html.erb`.

Exemple de contenu pour `app/views/welcome/index.html.erb` :

```erb
<h1>Articles</h1>
<ul>
  <% @articles.each do |article| %>
    <li><%= article.title %></li>
  <% end %>
</ul>
```

Entre `<% %>` se glisse du code Ruby grâce au moteur ERB. Les éléments communs (menu, pied de page) se placent dans `app/views/layouts/application.html.erb`.

## 11. Migrations et gestion de la base

Une migration décrit une modification du schéma. Pour ajouter une colonne `published_at` :

```bash
rails generate migration AddPublishedAtToArticles published_at:datetime
rails db:migrate
```

En cas de souci, retour en arrière :

```bash
rails db:rollback
```

## 12. Tester son application

Rails fournit **Minitest** pour automatiser les tests. Pour tout lancer :

```bash
rails test
```

C'est un moyen sûr d'éviter les régressions quand on ajoute des fonctionnalités. Certains préfèrent **RSpec**, à installer via le `Gemfile`.

## 13. Fonctionnalités avancées

- **Action Mailer** : envoyer des e-mails.
- **Active Job** : exécuter des tâches en arrière‑plan.
- **Action Cable** : websockets et temps réel.
- **Active Storage** : joindre des fichiers à vos modèles.

## 14. Déployer son application

Pour mettre votre site en ligne avec Heroku :

```bash
heroku create
git push heroku main
heroku run rails db:migrate
```

N'oubliez pas de configurer les variables d'environnement (clés, bases de données) avant la mise en production.

## 15. Aller plus loin

La [documentation officielle de Rails](https://guides.rubyonrails.org/) explore en profondeur chaque sujet : sécurité, performance, API, tests… Prenez l'habitude de la consulter.

En suivant pas à pas ce guide, même le roi des noobs pourra comprendre comment fonctionne Rails et construire ses propres applications web.
