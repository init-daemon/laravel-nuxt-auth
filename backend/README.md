# devenir_un_vrai_developpeur_laravel
## Routes
* Route::get('/posts/{id}', [Controller]->whereNumber('id')) préciser le type de variable d'une route en nombre. Ici, {id} doit être un nombre sinon 404
* ``Route::name(String)``: permet de nommer le route
```console 
$ php artisan route:list #permet de lister toute les routes de l'application
```
## Le controller et view
* ``view(String $1, compact(String...$2));``:  où $2 est juste une chaine, nom de la variable à passer
* ``view(String $1)->with(String $2, Any $3);``: $2 est la clé en chaine de caractère et $3 la valeur
### blades
* ``@yield(String $contentName)`` 
    * $contentName: nom du contenu à mettre à cette emplacement, comme slot dans vue
* ``@extends(String $cheminDansView)``; pour le chemin, on peut utiliser de point pour le repertoire vers le fichier en question
* `@section(String $contentName)...@endsection`
    * $contentName: la valeur de @yield précédent
* `@include(String $cheminDansView)`: permet d'inclure le partial dans un template blade
* `route(String)`: obtenir le chemin de route en utilisant son nom
* `asset($cheminDansPublic)`: permet de recupérer le chemin dans public
### Minification: Laravel Mix
* `mix.js(String $sourcePath, String $destinationPath);`
* `mix.postCss(String $sourcePath, String $destinationPath);`
* utilisation, apres avoir executer npm run dev, ils seront minifié dans un seul fichier correspondant 
```php
mix.js('resources/js/app.js', 'public/js')
    .postCss('resources/css/app.css', 'public/css');
```