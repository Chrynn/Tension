### PAGE BASICS

třída pro aktuální stránku
```css
.page-active
```
třída pro aktuální článek
```css
.current-post-parent
```
úprava vyhledávání (sidebar)
> app/public/wp-includes/theme-compat/sidebar.php

získání hlavičky stránky
> soubor se musí jmenovat `header.php` a musí být v rootu šablony (template)
```php
get_header()
```

napojení na WP pomocí hlavičky `<head>`
```php
wp_head()
```
zobrazení WP menu
> - musí se definovat přímo ve WP
```php
wp_nav_menu(
    array(
        'menu' => 'primary',
        'container' => '',
        'theme_location' => 'primary',
    )
);
```
získání PHP obsahu
```php
get_template_part('template-parts/contact/contact', 'index')
```
získání patičky stránky
> soubor se musí jmenovat `footer.php` a musí být v rootu šablony (template)
```php
get_footer()
```

### PAGE EXTENSIONS
zobrazí se pokud se nacházím na hlavní stránce
```php
if (is_home()) {

}
```
zobrazí se pokud se nacházím v kategorii
```php
if (is_category()) {

}
```
zobrazí se pokud se nacházím na dané stránce
```php
if (is_page('kontakt', 'domu')) {

}
```
zobrazení shortoce podle jednotlivých obsahů
> - k obahům se dostávám pomocí indexace `$content[]`
```php
$var = get_the_content();
$content = explode('<!-- wp:shortcode /-->', $var);
echo do_shortcode($content[0]);
```
zobrazení tagů
```php
$post_tags = get_the_tags();
if ($post_tags) {
    echo $post_tags[0]->name, $post_tags[1]->name;
}
```
titulek článkuu
```php
the_title()
```
popis článku
```php
get_content()
```
výňatek článku
```php
the_excerpt()
```
obrázek článku
```php
the_post_thumbnail()
```
název kategorie
```php
single_cat_title()
```
název kategorie s odkazem `<a href="">`
```php
the_category()
```
popis kategorie
> kategorie nemá výňatek (excerpt), ale popis (description)
```php
echo category_description()
```
odkaz na upravení článku
> vidí pouze přihlášený
```php
edit_post_link('upravit')
```
čas a autor  článku
```php
the_time('F jS, Y'); ?> by <?php the_author_posts_link()
```
obsah vidící pouze admin
```php
if (current_user_can('administrator')) {

}
```
titulek (title) stránky
```php
bloginfo('name')
```
Tagline
```php
bloginfo('description')
```

### WP smyčka
```php
<?php	
	// category loop
	<?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?>

	<?php endwhile; else : ?>
	         <p><?php esc_html_e( 'Sorry, no posts matched your criteria.' ); ?></p>
        <?php endif; ?>
```
### `Functions.php`
```php
<?php
	// CSS import
    	function tension_scripts() {
      	wp_enqueue_script( 'scripts', get_template_directory_uri() . '/assets/js/script.js', array(), '1.0.0' );
      	wp_enqueue_style( 'stylesheets', get_template_directory_uri() . '/assets/css/style.css', array(), '1.0.0' );
    	}
    	add_action( 'wp_enqueue_scripts', 'tension_scripts' );
    
    	// allow SVG pics
    	add_filter( 'wp_check_filetype_and_ext', function($data, $file, $filename, $mimes) {
      	global $wp_version;
      	if ( $wp_version !== '4.7.1' ) {
        	return $data;
      	}
      	$filetype = wp_check_filetype( $filename, $mimes );
      	return [
          'ext'             => $filetype['ext'],
          'type'            => $filetype['type'],
          'proper_filename' => $data['proper_filename']
      	];
    	}, 10, 4 );
    	function cc_mime_types( $mimes ){
      	$mimes['svg'] = 'image/svg+xml';
      	return $mimes;
    	}
    	add_filter( 'upload_mimes', 'cc_mime_types' );
    	
	// remove admin bar
    	add_filter('show_admin_bar', '__return_false');
    	
	// add menu types (to show in wp menu)
    	function snytbrno_menu(){
      	$locations = array(
        	'primary' => 'Top Menu',
        	'secondary' => 'Side Menu',
        	'footer' => 'Footer Menu'
      	);
      	register_nav_menus($locations);
?>
```
### Struktura šablony

> složky
- template-files (page, frontpage, category)
- inc
- assets (css, js, img)

> soubory
- `front-page.php`
- `page.php`
- `funtions.php`
- `category.php`
- `single.php`
- `search.php`
- `404.php`
- `style.css`

### Komentáře pro šablonu
`front-page.php`
```
/**
* The main template file
*
* Template file for czech restaurant snyt-brno
* 
* @link http://snytbrno.local/wp-content/themes/snyt-brno
*
* @package Wordpress
* @subpackage snyt-brno
* @since snyt-brno 1.0
*/
```

`style.css`
```css
/*
Theme Name: Wordpress Talk
Theme URI: wordpresstalk.com
Author: wordpress talk company
Author URI: http://wordpresstalkcompany.com
Description: This is a place we can talk about wordpress stuff.
Version: 1.0
License: GNU General Public License v2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html
Tags: black, brown, orange, tan, white
Text Domain: wordpress
This theme, like WordPress, is licensed under the GPL.
Use it to make something cool, have fun, and share what you've learned with others.
*/
```