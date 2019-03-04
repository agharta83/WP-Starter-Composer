# Installation custom WordPress avec Composer

1. Création du répertoire (mkdir "nom_du_répertoire") et installation de WordPress
En ligne de commande :
* "composer init" en ligne de commande
* Ouvrir le répertoire avec l'éditeur, il doit y avoir "composer.json"
Dans le fichier "composer.json" :
* Ajouter (modifier la version si besoin)
`{
  "require": {
    "php": ">=5.4",
    "johnpbloch/wordpress": "4.*"
  },
  "extra": {
    "wordpress-install-dir": "wp"
  }
}`
* "composer update" en ligne de commande
* Copier / coller les fichiers suivants à la racine du projet : wp-config-sample.php, index.php

2. Configuration manuelle de WordPress
* Créer le fichier .htaccess à la racine du projet, et coller les informations suivantes :
`RewriteEngine on
RewriteCond %{REQUEST_URI} !^sousdossier/
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1`
* Modifier le DIRNAME dans le fichier index.php pour qu'il pointe vers le bon dossier de wp ('/wp/wp-blog-header.php')
* Copier / coller "wp-config-sample.php" et renommer le nouveau fichier en "wp-config.php"

3. Configuration de la base de donnée (PHPMyAdmin et MySQL)
* Ouvrir PHPMYAdmin, saisir le login et mdp.
* Ajouter un nouvel utilisateur en local et cliquer sur "Executez"
* Dans le fichier "wp-config.php" modifier les variables selon la base de données créée
* Générer les clés de salage sur le site de wordpress et copier / coller dans le dossier wp-config.php

4. Ajout de thémes et plugins
* Créer le répertoire "content" à la racine du projet
* Dans le fichier "wp-config", modifier le CONTENT_URL et le CONTENT_DIR pour qu'il pointe vers l'URL du site en local (dessous $table_prefix)
`define( 'WP_CONTENT_URL', 'http://monurl.local/content' );
define( 'WP_CONTENT_DIR', dirname( ABSPATH ) . '/content' );`
* Dans le fichier "composer.json", ajouter :
`{
  "repositories": [
    {
      "type": "composer",
      "url": "https://wpackagist.org"
    }
  ],
  "require": {
    "php": ">=5.4",
    "johnpbloch/wordpress": "4.*",
    "wpackagist-plugin/advanced-custom-fields": "*",
    "wpackagist-plugin/contact-form-7": "*",
    "wpackagist-theme/hueman":"*",
    "wpackagist-theme/twentyseventeen":"*",
    "wpackagist-theme/bento":"*"
  },
  "extra": {
    "wordpress-install-dir": "wp",
    "installer-paths": {
        "content/plugins/{$name}/": ["type:wordpress-plugin"],
        "content/themes/{$name}/": ["type:wordpress-theme"]
    }
  }
}`
* "composer update" en ligne de commande

5. Activation des droits si besoin
* Activer les droits si besoin (apparement sur windows pas necessaire), en ligne de commande à la racine du projet
`sudo chown -R <user>:www-data .
sudo find . -type f -exec chmod 664 {} +
sudo find . -type d -exec chmod 775 {} +`

6. Vérification que tout fonctionne correctement
* Se rendre dans le navigateur "localhost/dossier-du-projet/wp-admin/install.php"
* Dans le tableau de bord => Settings => General => Site Adress (URL) => delete "/wp" à la fin du chemin => Save
* Permaliens => Settings => Permalinks => Post name => Save

7. Activer le mode DEBUG
* Dans le dossier .htaccess, supprimer les rewrite
* Copier / coller les lignes de codes du mode DEBUG  du fichier wp-config-sample.php dans le fichier wp-config.php et mettre le mode DEBUG actif (true)

8. Configurer GIT
* Créer le fichier ".gitignore" à la racine du projet et ajouter les dossiers suivants :
`/wp
/vendor
/content/plugins/akismet
/content/plugins/contact-form-7
/content/themes/twentyseventeen
/wp-config.php
/content/languages`
* Dans le tableau de bord de WordPress, vérifier que tous fonctionne et dans l'onglet "Settings", mettre WordPress en français.
* En ligne de commande : "git init", "git status", "git add .", "git commit" => "git push" (si besoins)

# WORDPRESS EST INSTALL ET CONFIGURE EN MODE DEVELOPPEUR