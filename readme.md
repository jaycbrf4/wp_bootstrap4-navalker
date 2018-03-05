wp-bootstrap4-navwalker
======================

**A custom WordPress nav walker class to fully implement the Bootstrap 4.0.0 navigation style in a custom theme using the WordPress built in menu manager.**

This version adds the support for multi level dropdowns.

NOTE
----
This is a utility class that is intended to format your WordPress theme menu with the correct syntax and classes to utilize the Bootstrap dropdown navigation, and does not include the required Bootstrap 4 JS files. You will have to include them manually. 

Installation
------------
Place **wp_bootstrap4_navwalker.php** in your WordPress theme folder `/wp-content/your-theme/`

Open your WordPress themes **functions.php** file  `/wp-content/your-theme/functions.php` and add the following code:

```php
// Register Custom Navigation Walker
require_once('wp_bootstrap4_navwalker.php');
```

Usage
------------
Update your `wp_nav_menu()` function in `header.php` to use the new walker by adding a "walker" item to the wp_nav_menu array.

```php
 <?php
          $args = array(
		  'theme_location' => 'mega_menu',
		  'depth' => 0,
		  'container' => '',
		  'menu_class'  => 'nav navbar-nav',
		  'walker'  => new BootstrapNavMenuWalker()
          );
    wp_nav_menu($args);
        ?>
```

Your menu will now be formatted with the correct syntax and classes to implement Bootstrap dropdown navigation. 

You will also want to declare your new menu in your `functions.php` file.

```php
register_nav_menus( array(
	'primary' => __( 'Primary Menu', 'THEMENAME' ),
) );
```

Typically the menu is wrapped with additional markup, here is an example of a ` navbar-fixed-top` menu that collapse for responsive navigation.

```php
    <nav class="navbar navbar-expand-md bg-dark fixed-top">
      <div class="container-fluid">
      <!-- custom calls to options stored in Admin section "Theme Options" to display the logo or not -->
      <a class="navbar-brand" id="logo" href="<?php echo site_url(); ?>"><img src="<?php header_image(); ?>" alt="Our Logo" class="img-responsive logo"/></a>
      <!-- custom calls to options stored in Admin section "Theme Options" to display the logo or not -->
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#jd-bootstrap-nav-collapse" aria-controls="jd-bootstrap-nav-collapse" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
      </button>
      <!-- Collect the nav links from WordPress -->
        <div class="collapse navbar-collapse" id="jd-bootstrap-nav-collapse">         
          <?php $args = array(
            'theme_location' => 'mega_menu',
            'depth' => 0,
            'container' => '',
            'menu_class'  => 'navbar-nav mr-auto',
            'walker'  => new BootstrapNavMenuWalker()
            );
            wp_nav_menu($args);
          ?>

        </nav>
```

To change your menu style add Bootstrap nav class names to the `menu_class` declaration.

Review options in the Bootstrap docs for more information on nav classes for responsiveness, theme and color
http://getbootstrap.com/docs/4.0/components/navbar/

Displaying the Menu 
-------------------
To display the menu you must associate your menu with your theme location. You can do this by selecting your theme location in the *Theme Locations* list wile editing a menu in the WordPress menu manager.


To get the submenus to open on hover
--------------------
Since Bootstrap does not support the click open of submenus add the following CSS to your theme's stylesheet
		
		```  @media (min-width: 767px) { /* change to appropriate breakpoint based on your menu class */
			  .divider::after {
			    content: "|";
			    padding: 0 4px;
			  }
			  /** make dropdown active on hover and fade in**/
			  ul.nav li.dropdown > ul.dropdown-menu{
			    visibility:hidden;
			    display:block;
			    opacity:0;
			    margin-top: 20px; /*  adds animated slide up */
			    transition: all 0.5s ease-in-out;
			  }
			  ul.nav li.dropdown:hover > ul.dropdown-menu{
			    visibility:visible;
			    opacity: 1;
			    display: block;
			    margin-top: 7px; /*  adds animated slide up */
			    background: #fff;
			    border-bottom-right-radius: 16px;
			    border-bottom-left-radius: 16px;
			  }
			}
The following jQuery will be used to add functionality to popper.js to open and close submenus when collapsed. Add to your theme's JS file

			(function($){   //Declare jQuery NameSpace

			//add show class on dropdpwns when clicked
			$(document).ready(function()
			{
			    $('a.dropdown-toggle[data-toggle="dropdown"]').on('click', function(event) 
			    {
			      var currentTarget = $(this),
				  parent        = currentTarget.parent(),
				  siblings      = parent.siblings();

			      event.preventDefault(); 
			      event.stopPropagation(); 

			      siblings.removeClass('show');
			      siblings.find('ul').removeClass('show');
			      parent.toggleClass('show');
			      parent.children('ul').toggleClass('show');
			    });
				});
			})(jQuery); //Close jQuery

Or you can omit the JS and not toggle the show class-
			
			


Original work by johnmegahan https://gist.github.com/1597994, Emanuele 'Tex' Tessore https://gist.github.com/3765640

