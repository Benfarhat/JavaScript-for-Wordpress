# Wordpress en javascript

Le principe de ce dépot est de voir comment faire du développement Wordpress avec du javascript.

Wordpress est un gestionnaire de contenu (C.M.S.) hautement personalisable et extensible via:

- les thèmes qui modifient le visuel
- les plugins (ou extensions) qui rajoutent des fonctionalités

Le coeur de Wordpress est à la base écrit en PHP, mais depuis quelques année, face à l'engouement des développeurs pour javascript et surtout aux possibilités offertes par ce dernier depuis sa version ES6, de nombreuses initiatives JS sur wordpress voient le jour dont notamment Calypso (l'interface d'adinistration sur worpress.com) et l'éditeur SPA "Gutenberg".

Il est par défaut possible d'insérer du code javascript dans votre thème ou dans la zone d'administration via les fonctions php suivantes:

- wp_enqueue_script() (on y renseigne le nom du handle, le path vers le fichier, les dépendances ou encore les fichiers après lesqueles il doit être chargé, la version qui peut être changée pour reformuler le cache et un boolen qui dit si l'appel se fait de la footer/true ou dans le header/false de votre page)
- wp_localize_script() qui permet notamment de mettre à disposition des variables aux scripts

A propos du booléen final il s'agit de l'équivalent de wp_head() et wp_footer()

```
function hook_javascript() {
    ?>
        <script>
            alert('Page is loading...');
        </script>
    <?php
}
add_action('wp_head', 'hook_javascript');
```

Au niveau PHP, pour mettre a disposition d'un script les variables "nom" et "age", il faut faire ceci:

```
<?php

// Register the script
wp_register_script( 'monScript', 'path/to/myscript.js' );

// Localize the script with new data
$translation_array = array(
	'nom' => __( 'Benfarhay', 'plugin-domain' ),
	'age' => '40'
);
wp_localize_script( 'monScript', 'object_name', $translation_array );

// Enqueued script with localized data.
wp_enqueue_script( get_stylesheet_directory_uri().'monScript' );
```
qui seront récupérés comme suit:

```
<script>
alert( "Bonjour je m'appelle " + object_name.nom + " et j'ai " + parseInt( object_name.age, 10 ) + " ans");
</script> 

```

