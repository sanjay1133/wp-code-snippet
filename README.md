<div align="center"><h1><a href="https://www.galaxyweblinks.com/" target="_blank">Galaxy Weblinks</a></h1></div>

# WordPress - Snippets

![Licence](https://img.shields.io/badge/Unlicense-red)

## Overview

This is a list of useful **WordPress** and **WooCommerce** code snippets and functions that I often reference to enhance or clean up my sites. 

**Note:** Please be careful and make backups!

## WordPress

- [Hide WordPress Update Nag To All But Admins](#hide-wordpress-update-nag-to-all-but-admins)
- [Utilize Proper WordPress Titles](#utilize-proper-wordpress-titles)
- [Create Custom WordPress Dashboard Widget](#create-custom-wordpress-dashboard-widget)
- [Remove All Dashboard Widgets](#remove-all-dashboard-widgets)
- [Include Navigation Menus](#include-navigation-menus)
- [Insert Custom Login Logo](#insert-custom-login-logo)
- [Modify Admin Footer Text](#modify-admin-footer-text)
- [Enqueue Styles And Scripts](#enqueue-styles-and-scripts)
- [Enqueue Google Fonts](#enqueue-google-fonts)
- [Modify Excerpt Length](#modify-excerpt-length)
- [Change Read More Link](#change-read-more-link)
- [Change More Excerpt](#change-more-excerpt)
- [Disable Emoji Mess](#disable-emoji-mess)
- [Remove Comments](#remove-comments)
- [Change Media Gallery URL](#change-media-gallery-url)
- [Create Custom Thumbnail Size](#create-custom-thumbnail-size)
- [Allow Additional File Formats For The Media Library](#allow-additional-file-formats-for-the-media-library)
- [Add Categories For Attachments](#add-categories-for-attachments)
- [Add Tags For Attachments](#add-tags-for-attachments)
- [Add Custom Excerpt To Pages](#add-custom-excerpt-to-pages)
- [Create A Global String](#create-a-global-string)
- [Support Featured Images](#support-featured-images)
- [Support Search Form](#support-search-form)
- [Excluding pages from search](#excluding-pages-from-search)
- [Disable XMLRPC](#disable-xmlrpcphp)
- [Escape HTML In Posts](#escape-html-in-posts)
- [Create Custom Global Settings](#create-custom-global-settings)
- [Remove WordPress Admin Bar](#remove-wordpress-admin-bar)
- [Add Open Graph Meta Tags](#add-open-graph-meta-tags)
- [Add Custom Post Type](#add-custom-post-type)
- [Implement Preconnect To Google Fonts In Themes](#implement-preconnect-to-google-fonts-in-themes)
- [Add Thumbnail Column To Post Listing](#add-thumbnail-column-to-post-listing)
- [Add Lead Class To First Paragraph](#add-lead-class-to-first-paragraph)
- [Exclude Custom Post Type From Search](#exclude-custom-post-type-from-search)
- [Remove Query String From Static Resources](#remove-query-string-from-static-resources)
- [Disable Website Field From Comment Form](#disable-website-field-from-comment-form)
- [Modify jQuery](#modify-jquery)
- [Disable JSON Rest API](#disable-json-rest-api)
- [Switch Post Type](#switch-post-type)
- [PHP Logger](#php-logger)
- [Always Show Second Bar In TinyMCE](#always-show-second-bar-in-tinymce)
- [Remove Admin Menu Items Depending On User Role](#remove-admin-menu-items-depending-on-user-role)
- [Remove Admin Menu Items Depending On Email Address (Domain)](#remove-admin-menu-items-depending-on-email-address-domain)
- [Reorder Admin Menu Items](#reorder-admin-menu-items)
- [Exclude A Category From WordPress Loops](#exclude-a-category-from-wordpress-loops)
- [Disable The JQMIGRATE Warning Message](#disable-the-jqmigrate-warning-message)
- [Disable Gutenberg From WordPress 5 Without A Plugin](#disable-gutenberg-from-wordpress-5-without-a-plugin)
- [Disable Automatic Updates In WordPress](#disable-automatic-updates-in-wordpress)
- [Disable Automatic WordPress Plugin Updates](#disable-automatic-wordpress-plugin-updates)
- [Disable Automatic WordPress Theme Updates](#disable-automatic-wordpress-theme-updates)
- [Cleanup Image Edits In WordPress](#cleanup-image-edits-in-wordpress)
- [Specify The Number Of Post Revisions](#specify-the-number-of-post-revisions)
- [Disable Post Revisions](#disable-post-revisions)
- [Show Popular Posts Without Plugins](#show-popular-posts-without-plugins)
- [Linking Images Within A Theme](#linking-images-within-a-theme)
- [Display Different Menus To Logged-In Users](#display-different-menus-to-logged-in-users)
- [Send Email To Admin If User Updated His Profile](#send-email-to-admin-if-user-updated-his-profile)
- [Create New Custom Widget](#create-new-custom-widget)

## Security

- [Disable Theme Or Plugin Editor In WP Admin](#disable-theme-or-plugin-editor-in-wp-admin)
- [Remove WordPress Version](#remove-wordpress-version)

## Other

- [Default robots.txt For WordPress](#default-robotstxt-for-wordpress)
- [Simple .htaccess For WordPress](#simple-htaccess-for-wordpress)

---

## WordPress

### Hide WordPress Update Nag To All But Admins

```php
/**
 * Hide WordPress update nag to all but admins
 */
function hide_update_notice_to_all_but_admin() {
    if ( !current_user_can( 'update_core' ) ) {
        remove_action( 'admin_notices', 'update_nag', 3 );
    }
}

add_action( 'admin_head', 'hide_update_notice_to_all_but_admin', 1 );
```

### Utilize Proper WordPress Titles

Make sure to remove the `<title>` tag from your header.

```php
/**
 * Utilize proper WordPress titles
 */
add_theme_support( 'title-tag' );
```

### Create Custom WordPress Dashboard Widget

```php
/**
 * Create custom WordPress dashboard widget
 */
function dashboard_widget_function() {
    echo '
        <h2>Custom Dashboard Widget</h2>
        <p>Custom content here</p>
    ';
}

function add_dashboard_widgets() {
    wp_add_dashboard_widget( 'custom_dashboard_widget', 'Custom Dashoard Widget', 'dashboard_widget_function' );
}

add_action( 'wp_dashboard_setup', 'add_dashboard_widgets' );
```

### Remove All Dashboard Widgets

```php
/**
 * Remove all dashboard widgets
 */
function remove_dashboard_widgets() {
    global $wp_meta_boxes;
    
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_quick_press'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_incoming_links'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_right_now'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_plugins'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_drafts'] );
    unset( $wp_meta_boxes['dashboard']['normal']['core']['dashboard_recent_comments'] );
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_primary'] );
    unset( $wp_meta_boxes['dashboard']['side']['core']['dashboard_secondary'] );

    remove_meta_box( 'dashboard_activity', 'dashboard', 'normal' );
}

add_action( 'wp_dashboard_setup', 'remove_dashboard_widgets' );
```

### Include Navigation Menus

```php
/** 
 * Include navigation menus
 */
function register_my_menu() {
    register_nav_menu( 'nav-menu', __( 'Navigation Menu' ) );
}

add_action( 'init', 'register_my_menu' );
```

Insert this where you want it to appear, and save the menu in **Appearance -> Menus**.

```php
wp_nav_menu( array( 'theme_location' => 'nav-menu' ) );
```

Here's the code for multiple menus:

```php
function register_my_menus() {
    register_nav_menus(
        array(
            'new-menu'      => __( 'New Menu' ),
            'another-menu'  => __( 'Another Menu' ),
            'an-extra-menu' => __( 'An Extra Menu' ),
        )
    );
}

add_action( 'init', 'register_my_menus' );
```

### Insert Custom Login Logo

```php
/**
 * Insert custom login logo
 */
function custom_login_logo() {
    echo '
        <style>
            .login h1 a { 
                background-image: url(image.jpg) !important; 
                background-size: 234px 67px; 
                width:234px; 
                height:67px; 
                display:block; 
            }
        </style>
    ';
}

add_action( 'login_head', 'custom_login_logo' );
```

### Modify Admin Footer Text

```php
/**
 * Modify admin footer text
 */
function modify_footer() {
    echo 'Created by <a href="mailto:you@example.com">you</a>.';
}

add_filter( 'admin_footer_text', 'modify_footer' );
```

### Enqueue Styles And Scripts

```php
/**
 * Enqueue styles and scripts
 */
function custom_scripts() {
    wp_enqueue_style( 'bootstrap', get_template_directory_uri() . '/css/bootstrap.min.css', array(), '3.3.6' );
    wp_enqueue_style( 'style', get_template_directory_uri() . '/css/style.css' );
    wp_enqueue_script( 'bootstrap', get_template_directory_uri() . '/js/bootstrap.min.js', array('jquery'), '3.3.6', true );
    wp_enqueue_script( 'script', get_template_directory_uri() . '/js/script.js' );
}

add_action( 'wp_enqueue_scripts', 'custom_scripts' );
```

### Enqueue Google Fonts

```php
/**
 * Enqueue Google Fonts
 */
function google_fonts() {
    wp_register_style( 'OpenSans', '//fonts.googleapis.com/css?family=Open+Sans:400,600,700,800' );
    wp_enqueue_style( 'OpenSans' );
}

add_action( 'wp_print_styles', 'google_fonts' );
```

### Modify Excerpt Length

```php
/**
 * Modify excerpt length
 */
function custom_excerpt_length( $length ) {
    return 25;
}

add_filter( 'excerpt_length', 'custom_excerpt_length', 999 );
```

### Change Read More Link

```php
/**
 * Change Read More link
 */
function custom_read_more_link() {
    return '<a href="' . get_permalink() . '">Read More</a>';
}

add_filter( 'the_content_more_link', 'custom_read_more_link' );
```

### Change More Excerpt

```php
/**
 * Change More excerpt
 */
function custom_more_excerpt( $more ) {
    return '...';
}

add_filter( 'excerpt_more', 'custom_more_excerpt' );
```

### Disable Emoji Mess

```php
/**
 * Disable Emoji mess
 */
function disable_wp_emojicons() {
    remove_action( 'admin_print_styles', 'print_emoji_styles' );
    remove_action( 'wp_head', 'print_emoji_detection_script', 7 );
    remove_action( 'admin_print_scripts', 'print_emoji_detection_script' );
    remove_action( 'wp_print_styles', 'print_emoji_styles' );
    remove_filter( 'wp_mail', 'wp_staticize_emoji_for_email' );
    remove_filter( 'the_content_feed', 'wp_staticize_emoji' );
    remove_filter( 'comment_text_rss', 'wp_staticize_emoji' );
    add_filter( 'tiny_mce_plugins', 'disable_emojicons_tinymce' );
    add_filter( 'emoji_svg_url', '__return_false' );
}

add_action( 'init', 'disable_wp_emojicons' );

function disable_emojicons_tinymce( $plugins ) {
    return is_array( $plugins ) ? array_diff( $plugins, array( 'wpemoji' ) ) : array();
}
```

### Remove Comments

```php
/**
 * Remove comments
 */
 
// Removes from admin menu
function my_remove_admin_menus() {
    remove_menu_page( 'edit-comments.php' );
}

add_action( 'admin_menu', 'my_remove_admin_menus' );

// Removes from post and pages
function remove_comment_support() {
    remove_post_type_support( 'post', 'comments' );
    remove_post_type_support( 'page', 'comments' );
}

add_action( 'init', 'remove_comment_support', 100 );

// Removes from admin bar
function mytheme_admin_bar_render() {
    global $wp_admin_bar;
    $wp_admin_bar->remove_menu( 'comments' );
}

add_action( 'wp_before_admin_bar_render', 'mytheme_admin_bar_render' );
```

### Change Media Gallery URL

```php
/**
 * Change Media Gallery URL
 */
if ( empty( get_option( 'upload_url_path' ) ) ) {
    update_option( 'upload_url_path', 'http://assets.website.com/wp-content/uploads' );
}
```

Also, you can filter the option value before it's retrieved from the database, which is slightly better:

```php
/**
 * Change Media Gallery URL
 */
add_filter( 'pre_option_upload_url_path', function() {
    return 'http://assets.website.com/wp-content/uploads';
});
```

### Create Custom Thumbnail Size

```php
/**
 * Create custom thumbnail size
 */
add_image_size( 'custom-thumbnail', 250, 250, true );
```

**Retrieve Thumbnail**

 ```php
 $thumb = wp_get_attachment_image_src( get_post_thumbnail_id($post->ID), 'custom-thumbnail' );

 echo $thumb[0]; 
 ```
 
Since WordPress 4.4.0, you can use:

```php
the_post_thumbnail_url( $size );
```

### Allow Additional File Formats For the Media Library

```php
/**
 * This code allows you to upload the file formats ZIP, MOBI, PDF, and EPUB
 */
function add_custom_mime_types( $mimes ) {
    $new_file_types = array (
        'zip'  => 'application/zip',
        'mobi' => 'application/x-mobipocket-ebook',
        'pdf'  => 'application/pdf',
        'epub' => 'application/epub+zip'
    );
    
    return array_merge( $mimes, $new_file_types );
}

add_filter( 'upload_mimes', 'add_custom_mime_types' );
```


### Add Categories For Attachments

```php
/**
 * Add categories for attachments
 */
function add_categories_for_attachments() {
    register_taxonomy_for_object_type( 'category', 'attachment' );
}

add_action( 'init' , 'add_categories_for_attachments' );
```

### Add Tags For Attachments

```php
/**
 * Add tags for attachments
 */
function add_tags_for_attachments() {
    register_taxonomy_for_object_type( 'post_tag', 'attachment' );
}

add_action( 'init' , 'add_tags_for_attachments' );
```

### Add Custom Excerpt To Pages

```php
/**
 * Add custom excerpt to pages
 */
function add_page_excerpt() {
    add_post_type_support( 'page', array( 'excerpt' ) );
}

add_action( 'init', 'add_page_excerpt' );
```

### Create A Global String

```php
/**
 * Create a global string
 */
function global_string() {
    return 'String';
}
```

**Retrieve Field**

```php
echo global_string();
```

### Support Featured Images

```php
/**
 * Support featured images
 */
add_theme_support( 'post-thumbnails' );
```

### Support Search Form

```php
/**
 * Support search form
 */
add_theme_support( 'html5', array( 'search-form' ) );
```

### Excluding Pages From Search

```php
/**
 * Excluding pages from search
 */
function exclude_pages_from_search() {
    global $wp_post_types;
    $wp_post_types['page']->exclude_from_search = true;
}

add_action( 'init', 'exclude_pages_from_search' );
```

### Disable xmlrpc.php

```php
/**
 * Disable xmlrpc.php
 */
 
add_filter( 'xmlrpc_enabled', '__return_false' );
remove_action( 'wp_head', 'rsd_link' );
remove_action( 'wp_head', 'wlwmanifest_link' );
```

### Escape HTML In Posts

```php
/**
 * Escape HTML in <code> or <pre><code> tags.
 */
function escapeHTML($arr) {
    if (version_compare(PHP_VERSION, '5.2.3') >= 0) {
        $output = htmlspecialchars($arr[2], ENT_NOQUOTES, get_bloginfo('charset'), false);
    } else {
        $specialChars = array(
            '&' => '&amp;',
            '<' => '&lt;',
            '>' => '&gt;'
        );

        // decode already converted data
        $data = htmlspecialchars_decode( $arr[2] );
        
	// escape all data inside <pre>
        $output = strtr( $data, $specialChars );
    }
    
    if (! empty($output)) {
        return  $arr[1] . $output . $arr[3];
    } else {
        return  $arr[1] . $arr[2] . $arr[3];
    }
}

function filterCode( $data ) { // Uncomment if you want to escape anything within a <pre> tag
    //$modifiedData = preg_replace_callback( '@(<pre.*>)(.*)(<\/pre>)@isU', 'escapeHTML', $data );
    $modifiedData = preg_replace_callback( '@(<code.*>)(.*)(<\/code>)@isU', 'escapeHTML', $data );
    $modifiedData = preg_replace_callback( '@(<tt.*>)(.*)(<\/tt>)@isU', 'escapeHTML', $modifiedData );

    return $modifiedData;
}

add_filter( 'content_save_pre', 'filterCode', 9 );
add_filter( 'excerpt_save_pre', 'filterCode', 9 );
```

Modified from [Escape HTML](https://wordpress.org/plugins/escape-html/).

### Create Custom Global Settings

```php
/**
 * Create custom global settings
 */
function custom_settings_page() { ?>
    <div class="wrap">
    <h1>Custom Settings</h1>
    <form method="post" action="options.php">
        <?php
            settings_fields( 'section' );
            do_settings_sections( 'theme-options' );
            submit_button();
        ?>
    </form>
    </div><?php 
}

function custom_settings_add_menu() {
    add_theme_page( 'Custom Settings', 'Custom Settings', 'manage_options', 'custom-settings', 'custom_settings_page', null, 99 );
}

add_action( 'admin_menu', 'custom_settings_add_menu' );

// Example setting
function setting_twitter() { ?>
    <input type="text" name="twitter" id="twitter" value="<?php echo get_option('twitter'); ?>" /><?php 
}

function custom_settings_page_setup() {
    add_settings_section( 'section', 'All Settings', null, 'theme-options' );
    add_settings_field( 'twitter', 'Twitter Username', 'setting_twitter', 'theme-options', 'section' );
    register_setting( 'section', 'twitter' );
}

add_action( 'admin_init', 'custom_settings_page_setup' );
```

**Retrieve Field**

```php
echo get_option( 'twitter' );
```

Modified from [Create a WordPress Theme Settings Page with the Settings API](http://www.sitepoint.com/create-a-wordpress-theme-settings-page-with-the-settings-api/).

### Remove WordPress Admin Bar

```php
/**
 * Remove WordPress admin bar
 */
function remove_admin_bar() {
    remove_action( 'wp_head', '_admin_bar_bump_cb' );
}

add_action( 'get_header', 'remove_admin_bar' );
```

### Add Open Graph Meta Tags

```php
/**
 * Add Open Graph Meta Tags
 */
function meta_og() {
    global $post;

    if ( is_single() ) {
        if( has_post_thumbnail( $post->ID ) ) {
            $img_src = wp_get_attachment_image_src( get_post_thumbnail_id( $post->ID ), 'thumbnail' );
        } 
        $excerpt = strip_tags( $post->post_content );
        $excerpt_more = '';
	
        if ( strlen($excerpt ) > 155) {
            $excerpt = substr( $excerpt,0,155 );
            $excerpt_more = ' ...';
        }
	
        $excerpt = str_replace( '"', '', $excerpt );
        $excerpt = str_replace( "'", '', $excerpt );
        $excerptwords = preg_split( '/[\n\r\t ]+/', $excerpt, -1, PREG_SPLIT_NO_EMPTY );
        array_pop( $excerptwords );
        $excerpt = implode( ' ', $excerptwords ) . $excerpt_more; ?>
	
        <meta name="author" content="Your Name">
        <meta name="description" content="<?php echo $excerpt; ?>">
        <meta property="og:title" content="<?php echo the_title(); ?>">
        <meta property="og:description" content="<?php echo $excerpt; ?>">
        <meta property="og:type" content="article">
        <meta property="og:url" content="<?php echo the_permalink(); ?>">
        <meta property="og:site_name" content="Your Site Name">
        <meta property="og:image" content="<?php echo $img_src[0]; ?>"><?php
    } else {
        return;
    }
}

add_action('wp_head', 'meta_og', 5);
```

### Add Custom Post Type

```php
/**
 * Add custom post type
 */
function create_custom_post() {
    register_post_type( 'custom-post', // slug for custom post type
        array(
        'labels' => array(
            'name' => __( 'Custom Post' ),
        ),
        'public'       => true,
        'hierarchical' => true, 
        'has_archive'  => true,
        'supports'     => array(
            'title',
            'editor',
            'excerpt',
            'thumbnail',
        ), 
        'can_export' => true,
        'taxonomies' => array(
             'post_tag',
              category',
        )
    ));
}

add_action( 'init', 'create_custom_post' );
```

**WordPress Codex**

[Function Reference/register post type](https://codex.wordpress.org/Function_Reference/register_post_type)

**WordPress Developer Resources**

[Dashicons](https://developer.wordpress.org/resource/dashicons/#menu)

### Implement Preconnect To Google Fonts In Themes

```php
/**
 * Implement preconnect to Google Fonts in themes
 */
function twentyfifteen_resource_hints( $urls, $relation_type ) {
    // Checks whether the subject is carrying the source of fonts google and the `$relation_type` equals preconnect.
    // Replace `enqueue_font_id` the `ID` used in loading the source.
    if ( wp_style_is( 'enqueue_font_id', 'queue' ) && 'preconnect' === $relation_type ) {
        // Checks whether the version of WordPress is greater than or equal to 4.7
        // to ensure compatibility with older versions
        // because the 4.7 has become necessary to return an array instead of string
        
	if ( version_compare( $GLOBALS['wp_version'], '4.7-alpha', '>=' ) ) {
            // Array with url google fonts and crossorigin
            $urls[] = array(
                'href' => 'https://fonts.gstatic.com',
                'crossorigin',
            );
        } else {
            // String with url google fonts
            $urls[] = 'https://fonts.gstatic.com';
        }
    }
    
    return $urls;
}

add_filter( 'wp_resource_hints', 'twentyfifteen_resource_hints', 10, 2 ); 
```

### Add Thumbnail Column To Post Listing

```php
/**
 * Add thumbnail column to post listing
 */
add_image_size( 'admin-list-thumb', 80, 80, false );

function wpcs_add_thumbnail_columns( $columns ) {
    if ( !is_array( $columns ) ) {
        $columns = array();
    }
    
    $new = array();

    foreach( $columns as $key => $title ) {
        if ( $key == 'title' ) { // Put the Thumbnail column before the Title column
            $new['featured_thumb'] = __( 'Image');
	}
	
        $new[$key] = $title;
    }
    
    return $new;
}

function wpcs_add_thumbnail_columns_data( $column, $post_id ) {
    switch ( $column ) {
    case 'featured_thumb':
        echo '<a href="' . $post_id . '">';
        echo the_post_thumbnail( 'admin-list-thumb' );
        echo '</a>';
        break;
    }
}

if ( function_exists( 'add_theme_support' ) ) {
    add_filter( 'manage_posts_columns' , 'wpcs_add_thumbnail_columns' );
    add_action( 'manage_posts_custom_column' , 'wpcs_add_thumbnail_columns_data', 10, 2 );
}
```
### Add Lead Class To First Paragraph

```php
/**
 * Add lead class to first paragraph
 */
function first_paragraph( $content ) {
    return preg_replace( '/<p([^>]+)?>/', '<p$1 class="lead">', $content, 1 );
}

add_filter( 'the_content', 'first_paragraph' );
```

Adds a `lead` class to the first paragraph in [the_content](https://developer.wordpress.org/reference/functions/the_content/).

### Exclude Custom Post Type From Search

```php
/**
 * Exclude custom post type from search
 */
function excludePages( $query ) {
   if ( $query->is_search ) {
      $query->set( 'post_type', 'post' );
   }
   return $query;
}

add_filter( 'pre_get_posts','excludePages' );
```

### Remove Query String From Static Resources

```php
/**
 * Remove query string from static resources 
 */
function remove_cssjs_ver( $src ) {
    if ( strpos( $src, '?ver=' ) ) {
        $src = remove_query_arg( 'ver', $src );
    }
	
    return $src;
}

add_filter( 'style_loader_src', 'remove_cssjs_ver', 10, 2 );
add_filter( 'script_loader_src', 'remove_cssjs_ver', 10, 2 );
```

### Modify jQuery

```php
/**
 * Modify jQuery
 */
function modify_jquery() {
    wp_deregister_script( 'jquery' );
    wp_register_script( 'jquery', 'https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js', false, '3.2.1' );
    wp_enqueue_script( 'jquery' );
}

if ( ! is_admin() ) add_action( 'wp_enqueue_scripts', 'modify_jquery' );
```

###  Disable Website Field From Comment Form

```php
/** 
 * Disable website field from comment form
 */
function disable_website_field( $field ) { 
    if( isset($field['url']) ) {
        unset( $field['url'] );
    }
    
    return $field;
}

add_filter('comment_form_default_fields', 'disable_website_field');
```

###  Disable JSON REST API

```php
/** 
 * Disable JSON REST API  
 */
add_filter( 'json_enabled', '__return_false' );
add_filter( 'json_jsonp_enabled', '__return_false' );
```

### Switch Post Type

```php
/**
 * Switch post type
 */
function switch_post_type ( $old_post_type, $new_post_type ) {
    global $wpdb;

    // Run the update query
    $wpdb->update(
        $wpdb->posts,
        // Set
        array( 'post_type' => $new_post_type),
        // Where
        array( 'post_type' => $old_post_type )
    );
}
```

### PHP Logger

```php
/**
 * PHP Logger
 */
function php_logger( $data ) {
    $output = $data;
    if ( is_array( $output ) )
        $output = implode( ',', $output );
        
    // print the result into the JavaScript console
    echo "<script>console.log( 'PHP LOG: " . $output . "' );</script>";
}
```

### Always Show Second Bar In TinyMCE

```php
/**
 * Always show second bar in TinyMCE
 */
function show_tinymce_toolbar( $in ) {
    $in['wordpress_adv_hidden'] = false;
    return $in;
}

add_filter( 'tiny_mce_before_init', 'show_tinymce_toolbar' );
```

### Remove Admin Menu Items Depending On User Role

```php
/**
 * Clone the administrator user role
 */
function clone_admin_role() {
    global $wp_roles;
    if ( ! isset( $wp_roles ) )
        $wp_roles = new WP_Roles();
    
    $adm = $wp_roles->get_role( 'administrator' );
    
    // Add new "Client" role with all admin capabilities
    $wp_roles->add_role( 'client', 'Client', $adm->capabilities );
}

add_action( 'init', 'clone_admin_role' );

/**
 * Specify which admin menu items are visible for users with role "Client"
 */
function remove_dashboard_menus() {
    if ( current_user_can( 'client' ) ) {
        // Hide Updates under Dashboard menu
        remove_submenu_page( 'index.php', 'update-core.php' );

        // Hide Comments
        remove_menu_page( 'edit-comments.php' );

        // Hide Plugins
        remove_menu_page( 'plugins.php' );

        // Hide Themes, Customizer and Widgets under Appearance menu
        remove_submenu_page( 'themes.php', 'themes.php' );
        remove_submenu_page( 'themes.php', 'customize.php?return=' . urlencode( $_SERVER['REQUEST_URI'] ) );
        remove_submenu_page( 'themes.php', 'widgets.php' );

        // Hide Tools
        remove_menu_page( 'tools.php' );

        // Hide General Settings
        remove_menu_page( 'options-general.php' );
    }
}

add_action( 'admin_menu', 'remove_dashboard_menus' );
```

### Remove Admin Menu Items Depending On Email Address (domain)

```php
/**
 * Specify which users can see admin menu items based on their email address
 */
function remove_dashboard_menus() {
    $user_data = get_userdata( get_current_user_id() );
    $user_email = isset( $user_data->user_email ) ? $user_data->user_email : '';

    if ( ! strpos( $user_email, '@yourcompany.com' ) ) {
        // Hide Updates under Dashboard menu
        remove_submenu_page( 'index.php', 'update-core.php' );

        // Hide Comments
        remove_menu_page( 'edit-comments.php' );

        // Hide Plugins
        remove_menu_page( 'plugins.php' );

        // Hide Themes, Customizer and Widgets under Appearance menu
        remove_submenu_page( 'themes.php', 'themes.php' );
        remove_submenu_page( 'themes.php', 'customize.php?return=' . urlencode( $_SERVER['REQUEST_URI'] ) );
        remove_submenu_page( 'themes.php', 'widgets.php' );

        // Hide Tools
        remove_menu_page( 'tools.php' );

        // Hide General Settings
        remove_menu_page( 'options-general.php' );
    }
}

add_action( 'admin_menu', 'remove_dashboard_menus' );
```

### Reorder Admin Menu Items

```php
/**
 * Reorder the Admin Menu
 */
function custom_menu_order( $menu_ord ) {
    if ( ! $menu_ord ) { return true; }
        return array(
            'index.php',
            'separator1',
            'edit.php?post_type=page', 
            'edit.php', 
            'edit.php?post_type=[your_post_type_slug]',
            'upload.php',
            'edit-comments.php',
            'separator2',
            'themes.php',
            'plugins.php',
            'users.php',
            'tools.php',
            'options-general.php'
        );
    }
}

add_filter( 'custom_menu_order', 'custom_menu_order' );
add_filter( 'menu_order', 'custom_menu_order' );
```

### Exclude A Category From WordPress Loops

```php
/**
 * Exclude a category from all WordPress loops
 */
add_action( 'pre_get_posts', function( $query ) { // anonymous callback
    
    global $wp_query; 

    // Hard coded category ID, but can be dynamic: esc_attr(get_option('your-cat-id')); 
    $excluded_cat_id = 25;

    // add category ID to existing, avoid overwriting it 
    $cat[] = $query->get( 'cat' );
    $cat[] = "-" . $excluded_cat_id;

    $query->set( 'cat', $cat );
    }
});
```

### Disable The JQMIGRATE Warning Message

```php
/**
 * Disable the message "JQMIGRATE: Migrate is installed version 1.4.1"
 */
add_action('wp_default_scripts', function ($scripts) {
    if (!empty($scripts->registered['jquery'])) {
        $scripts->registered['jquery']->deps = array_diff($scripts->registered['jquery']->deps, ['jquery-migrate']);
    }
});
```

### Disable Gutenberg From WordPress 5 Without A Plugin

```php
add_filter( 'use_block_editor_for_post', '__return_false' );
```

### Disable Automatic Updates In WordPress

```php
define( 'WP_AUTO_UPDATE_CORE', false );
```

**How to implement**

You can disable automatic updates in WordPress by adding this line of code in your **_wp-config.php_**.

### Disable Automatic WordPress Plugin Updates

```php
add_filter( 'auto_update_plugin', '__return_false' );
```

### Disable Automatic WordPress Theme Updates

```php
add_filter( 'auto_update_theme', '__return_false' );
```

### Cleanup Image Edits In WordPress

```php
define( 'IMAGE_EDIT_OVERWRITE', true );
```

By default, WordPress creates a new set of images every time an image is edited. When you restore the original image, the edits are stored on the server. Defining **_IMAGE_EDIT_OVERWRITE_** as **_true_** changes this behaviour. Only one set of image edits are ever created and when you restore the original, the edits are removed from the server.

**How to implement**

You can define cleanup image edits in WordPress by adding this line of code in your **_wp-config.php_**.

### Specify The Number Of Post Revisions

```php
define('WP_POST_REVISIONS', 3);
```

If you want to specify a maximum number of revisions, change false to an integer/number (e.g., 3 or 5).

**How to implement**

You can specify the number of post revisions in WordPress by adding this line of code in your **_wp-config.php_**.

### Disable Post Revisions

```php
define( 'WP_POST_REVISIONS', false );
```

By default, WordPress will save copies of each edit made to a post or page, allowing the possibility of reverting to a previous version of that post or page. The saving of revisions can be disabled, or a maximum number of revisions per post or page can be specified.

**How to implement**

You can disable post revisions in WordPress by adding this line of code in your **_wp-config.php_**.

### Show Popular Posts Without Plugins

```php
/**
 * Show Popular Posts Without Plugins
 */
function count_post_visits() {
    if( is_single() ) {
        global $post;
        $views = get_post_meta( $post->ID, 'my_post_viewed', true );
        if( $views == '' ) {
            update_post_meta( $post->ID, 'my_post_viewed', '1' );   
        } else {
            $views_no = intval( $views );
            update_post_meta( $post->ID, 'my_post_viewed', ++$views_no );
        }
    }
}

add_action( 'wp_head', 'count_post_visits' );
```

### Linking Images Within A Theme

```php
<img src="<?php bloginfo( 'stylesheet_directory' ); ?>/img/image.png" />
```

or

```php
<img src="<?php echo get_stylesheet_directory_uri(); ?>/images/image.jpg" />
```

### Display Different Menus To Logged-In Users

```php
function nav_menu_args( $args = '' ) {
    if ( is_user_logged_in() ) {
        $args['menu'] = 'Logged-In'; // we need to create this menu
    } else {
        $args['menu'] = 'Primary Menu'; // we need to create this menu
    }

    return $args;
}

add_filter( 'wp_nav_menu_args', 'nav_menu_args' );
```

### Send Email To Admin If User Updated His Profile

```php
function user_profile_update( $user_id ) {
    $user_info = get_userdata( $user_id );

    $to       = get_option( 'admin_email' );
    $subject  = 'User Profile Updated';
    $message  = "Hello Administrator,\n\nThe " . $user_info->user_nicename . " (" . $user_info->user_email . ") profile has been updated! \n\n";
    $message .= "Site URL: " . get_bloginfo( 'wpurl' ) . "\n\n";
    $message .= "Regards, \n" . get_option( 'blogname' );

    wp_mail( $to, $subject, $message );
}

add_action( 'profile_update', 'user_profile_update', 10, 2 );
```

### Create New Custom Widget

```php
class New_Widget extends WP_Widget {

	function __construct() {
		parent::__construct(
			'new_widget',
			esc_html__( 'Widget', 'textdomain' )
		);
	}

	private $widget_fields = array(
	);

	public function widget( $args, $instance ) {
		echo $args['before_widget'];

		if ( ! empty( $instance['title'] ) ) {
			echo $args['before_title'] . apply_filters( 'widget_title', $instance['title'] ) . $args['after_title'];
		}

		// Output generated fields
		
		echo $args['after_widget'];
	}

	public function field_generator( $instance ) {
		$output = '';
		foreach ( $this->widget_fields as $widget_field ) {
			$default = '';
			if ( isset($widget_field['default']) ) {
				$default = $widget_field['default'];
			}
			$widget_value = ! empty( $instance[$widget_field['id']] ) ? $instance[$widget_field['id']] : esc_html__( $default, 'textdomain' );
			switch ( $widget_field['type'] ) {
				default:
					$output .= '<p>';
					$output .= '<label for="'.esc_attr( $this->get_field_id( $widget_field['id'] ) ).'">'.esc_attr( $widget_field['label'], 'textdomain' ).':</label> ';
					$output .= '<input class="widefat" id="'.esc_attr( $this->get_field_id( $widget_field['id'] ) ).'" name="'.esc_attr( $this->get_field_name( $widget_field['id'] ) ).'" type="'.$widget_field['type'].'" value="'.esc_attr( $widget_value ).'">';
					$output .= '</p>';
			}
		}
		echo $output;
	}

	public function form( $instance ) {
		$title = ! empty( $instance['title'] ) ? $instance['title'] : esc_html__( '', 'textdomain' );
		?>
		<p>
			<label for="<?php echo esc_attr( $this->get_field_id( 'title' ) ); ?>"><?php esc_attr_e( 'Title:', 'textdomain' ); ?></label>
			<input class="widefat" id="<?php echo esc_attr( $this->get_field_id( 'title' ) ); ?>" name="<?php echo esc_attr( $this->get_field_name( 'title' ) ); ?>" type="text" value="<?php echo esc_attr( $title ); ?>">
		</p>
		<?php
		$this->field_generator( $instance );
	}

	public function update( $new_instance, $old_instance ) {
		$instance = array();
		$instance['title'] = ( ! empty( $new_instance['title'] ) ) ? strip_tags( $new_instance['title'] ) : '';
		foreach ( $this->widget_fields as $widget_field ) {
			switch ( $widget_field['type'] ) {
				default:
					$instance[$widget_field['id']] = ( ! empty( $new_instance[$widget_field['id']] ) ) ? strip_tags( $new_instance[$widget_field['id']] ) : '';
			}
		}
		return $instance;
	}
}

function register_new_widget() {
	register_widget( 'New_Widget' );
}
add_action( 'widgets_init', 'register_new_widget' );
```

## Security

### Disable Theme Or Plugin Editor In WP Admin

```php 
define( 'DISALLOW_FILE_EDIT', true ); 
```

**How to implement**

1. Locate the file **_wp-config.php_** in the base directory of your WordPress directory.
2. Open the file in a text editor.
3. Past the code above to the end of this file.
4. Save and close.

### Remove WordPress Version

```php 
/**
 * Remove WordPress version.
 * Better to hide it and keep hackers in the dark.
 */
function remove_wp_version() {
    return '';
}
add_filter( 'the_generator', 'remove_wp_version' );
```

## Other

### Default robots.txt For WordPress

```txt
User-Agent: *
Allow: /wp-content/uploads/
Disallow: /wp-content/plugins/
Disallow: /wp-content/themes/
Disallow: /readme.html
Disallow: /?s=
Disallow: /search/

Sitemap: http://www.sitename.com/post-sitemap.xml
Sitemap: http://www.sitename.com/page-sitemap.xml
```

**How to Implement:**

1. Create a file named **_robots.txt_**.
2. Copy and paste the snippet above to the file.
3. Switch the placeholder information with your unique address and save the file.
4. Drop the file in your root directory for your website.

### Simple .htaccess For WordPress

```txt
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

**How to Implement:**

1. Backup your current **_.htaccess_** file.
2. Edit a copy of the file and delete the current information.
3. Copy and paste the snippet above to the file.
4. Save the file.

---

## License

This repository is unlicense[d], so feel free to fork.
