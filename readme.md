wp-bootstrap4-navwalker
======================

**A custom WordPress nav walker class to fully implement the Bootstrap 4.0.0b navigation style in a custom theme using the WordPress built in menu manager.**

This version adds the support for multi level dropdowns and HTML menu item titles and descriptions in the link as well as custom html elements inside the nav description.

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
		  'theme_location' => 'primary',
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
            'theme_location' => 'primary',
            'depth' => 0,
            'container' => '',
            'menu_class'  => 'nav navbar-nav',
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

Displaying the Titles and Descriptions 
-------------------
To display the menu item descriptions you need to click the tab **Screen Options** at the top of the screen and check "Title Attribute" and "Descriptions". The title attribute is part of the menu ite's link. the description text is not inside the menu item's link so you can add lists for additional links there.

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

You have the ability to use HTML in the description field of your menu item but be careful not to break the menu.

Creating a Mega-Menu type nav
-------------------
Add the class **.mega-parent** to the top level link

Sample CSS:


		/* -=-=-=-=-=- DROPDOWNS -=-=-=-=-=-=- */
		.dropdown-menu > li > a:focus, .dropdown-menu > li > a:hover {
		background-color: transparent;
		color: #fff;
		text-decoration: none;
		border-bottom-left-radius: 16px;
		}
		.dropdown-menu  li > a {
		padding: 8px 20px;
		color: #fff;
		}
		.mega-parent  li > a {
		padding: 8px 0px;
		color: #fff;
		}
		.mega-parent ul.dropdown-menu {
		padding-bottom: 12px;
		}
		.dropdown-menu  li a:hover {
		color: #000;
		}
		.dropdown-menu  li:first-child{
		border-bottom-left-radius: 16px;
		}
		.dropdown-menu  li:last-child {
		border-bottom-right-radius: 16px;
		}
		
		.dropdown-menu {
		background-clip: padding-box;
		background-color: rgba(0, 0, 0, 0.8);
		border: 1px solid rgba(0, 0, 0, 1);
		border-top: none;
		box-shadow: 0 6px 12px rgba(0, 0, 0, 0.176);
		float: left;
		font-size: 14px;
		list-style: outside none none;
		min-width: 160px;
		padding:  0;
		position: absolute;
		text-align: left;
		top: 100%;
		z-index: 1000;
		border-bottom-right-radius: 16px;
		border-bottom-left-radius: 16px;
		}
		
		/* Mega menu styles */
		.mega-parent .dropdown-menu .sub {
		white-space: normal ;
		color: #fff;
		}
		
		.navbar-nav, .navbar .collapse, .navbar-nav li {
		position: static;
		}
		.navbar .container {
		position: relative;
		}
		.navbar .dropdown-menu {
		left: auto;
		}
		.mega-parent, .mega-parent .dropdown-menu {
		left: 0;
		right: 0;
		}
		.navbar .container {
		max-width: 1170px;
		width: auto;
		}
		.sub > div {
		font-size: 12px;
		line-height: 19px;
		height: 100% !important;
		}
		.sublinks a {
		font-size: 12px !important;
		font-weight: normal  !important;
		padding: 0 !important;
		}
		.dropdown-menu  li .sub a:hover {
		color: #f50000;
		}
		.sublinks {
		list-style-type: none;
		margin: 0;
		padding: 0;
		}
		.sublinks > li {
		margin-bottom: 6px;
		}
		.sub img {
		margin: 0 18px 12px 0;
		}
		a.nolink {
		visibility: hidden !important;
		} 
		.tit {
		font-size: 18px;
		font-weight: bold;
		margin: 0;
		padding: 0;
		}
		
		.sub > form {
		background: #eee none repeat scroll 0 0;
		border-radius: 8px;
		color: #666;
		padding: 6px;
		}
		
		
		@media (min-width: 768px) {
		/** make dropdown active on hover and fade in*/
		ul.nav li.dropdown > ul.dropdown-menu{
		visibility:hidden;
		display:block;
		opacity:0;
		margin-top: 8px;
		transition: all 0.2s ease-in-out;
		}
		ul.nav li.dropdown:hover > ul.dropdown-menu{
		visibility:visible;
		opacity: 1;
		display: block;
		margin-top: 0;
		} 
		}
		
		@media (max-width: 767px) { 
		.navbar-nav {
		background: rgba(0, 0, 0, 0.6);
		}
		#menu-main-nav li.mega-parent ul li {
		float: none;
		}
		.dropdown-menu > li:first-child{
		border-bottom-left-radius: 0;
		}
		.dropdown-menu  li:last-child {
		border-bottom-right-radius: 0;
		}
		.navbar-nav {
		margin: 13px -15px 0;
		width: auto;
		z-index: 1000;
		}
		.navbar-nav > li > a {border-right: none; margin-left: 10px;}
		}

Hide a link's Navigation Label
----------------------------------
 To hide the link's text when necessary add "hidden" to the link's Link Relationship (XFN). This is usefull when you only want the HTML description to show and not the WordPress link's Navigation Label.




Original work by johnmegahan https://gist.github.com/1597994, Emanuele 'Tex' Tessore https://gist.github.com/3765640

