# drupal9-movies-import
Challenge in Drupal 9 to get information and create local content from themoviedb (https://www.themoviedb.org/terms-of-use)

Note: The original version was written around at the end of 2019, it is just an ugrade to make this code work in D9
## Requirementes
https://docs.docker.com/get-docker/

https://docs.docker.com/compose/install/

## Local dev environment setup

To build and up the required services please run:
````bash
$ docker-compose build
$ docker-compose up -d
````

Once the containers are running:
````bash
$ docker exec -it php_movies_importer bash
````

Inside the container run composer install:
````bash
$ composer install
````

Inside the container move to the web folder and Install Druapal:
````bash
$ cd web
$ pwd && echo "pwd must prompt /var/www/html/web"
$ drush site-install standard \
--db-url='mysql://apupiales:apupiales123@database-app:3306/movies_importer' \
--account-name=admin --account-pass=admin123 \
--site-name=Movies \
--site-mail=dumy@test.com
````

Enable the custom modules
````bash
$ drush en dc_vocabularies
$ drush en dc_content_entities
$ drush en dc_content_import
$ drush en dc_pages
````

## API data import
The module with this logic is dc_movies_content_import. There is a form to config the connection setting where you can set an API key. This form is accessible in the WEB SERVICES section in admin/config or directly in /admin/config/services/api-connection-settings.
To get the API key, login or register in https://www.themoviedb.org/settings/api 
To import the content you just need to execute the next commands in this exactly order (be patient, there is a lot of content to import):

````bash
$ drush import:genres
$ drush import:movies upcoming 15
$ drush import:movies popular 500
$ drush import:actor 500
````

##Custom Pages
After the import you will be able to see the content renderer in 3 custom pages (pretty basic render)

'/home'

'/movies'

'/actors'
