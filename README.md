# wp-theme-woocomerce
Basis for Wordpress theme extension, adds __Woocommerce__ plugin support to any Wordpress theme.

__Notice!__
<br><em>This package is not "ready from the box". Thus you will need to perform some edits, before it starts to work properly.<br>I recommend to apply all changes to child-theme.</em>

## Edits to apply:

+ In __functions.php__ include base wrapper class.
	```php
    locate_template( 'extensions/woocommerce/main.php', true );
    ```

+ In __extensions/woocommerce/main.php__ update __Woocommerce__ template wrappers:
	* __Wrapper start__
		```php
		/**
		* Hooked to woocommerce_before_main_content, 10
		*/
		public function output_content_wrapper(){
			?>
			<!-- YOUR THEME's CONTENT WRAPPERS BEFORE MAIN CONTENT -->
			<?php
		}
		```
	* __Wrapper end__
		```php
		/**
		* Hooked to woocommerce_after_main_content, 10
		*/
		public function output_content_wrapper_end(){
			?>
			<!-- YOUR THEME's CONTENT WRAPPERS AFTER CONTENT -->
			<?php
		}
		```
	* __After Sidebar wrapper close__
		```php
		/**
		* Hooked to woocommerce_sidebar, 11
		*/
		public function after_sidebar_wrapper_close(){
			?>
			<!-- YOUR THEME's SIDEBAR WRAPPER END -->
			<?php	
		}
		```

+ Duplicate __header.php__ and __footer.php__ from original theme and rename them to __header-shop.php__ and __footer-shop.php__. You already have __sidebar-shop.php__ included.
	You may omit this step if you don't need the shop header and footer to differ from original site header and footer. Although you will still need copies of header and footer to integrate 'Shop menu'(see further).

+ Open your __page.php__(static page template), from original theme. Copy "page content code"(typically all between __get_header()__ and __get_footer()__). Replace code inside __extensions/woocommerce/templates/template-shop.php__ with copied "page content code".
	
    If __get_sidebar()__ is present inside "page content code" - replace it with __get_sidebar('shop')__.
	Alternatively you can replace __template-shop.php__ with renamed __page.php__. In this case, replace all occurrences of __get_header__, __get_footer__ and __get_sidebar__ calls whith their "shop" versions.
	
	This will provide you with static page template named 'Shop Page Template'. Available for selection in admin section of the site. On 'Edit Page' page, inside 'Page Attributes' meta-box.
	Main purpose of this template - output 'Shop Sidebar' on desired, non related to shop, pages.

+ Considering all performed well. So inside __Admin area__, you should see notice asking to perform requiered plugins instalation. Do install and activate plugins.

+ Install Woocommerce 'dummy data'(wp-content/plugins/woocommerce/dummy-data/dummy-data.xml or wp-content/plugins/woocommerce/dummy-data/dummy-data.csv) if needed.

+ You will need to create static pages __Compare__ and __Wishlist__ for __TM WooCommerce Compare & Wishlist__, if they
 aren't created already. And select them on __Woocommerce__ settings page.

+ Inside Admin area -> Appearance->Menus section, there will be added 'Shop menu' location. Create and attach new menu to it.

+ Now you can include __shop-nav-menu.php__-template
	```php
	get_template_part( 'extensions/woocommerce/templates/shop-nav-menu' );
	```

+ Add minicart navbar
	```php
	get_template_part( 'extensions/woocommerce/templates/shopping-cart-link' );
	```

+ Inside __extensions/woocommerce/assets/scss/__ you will find __styles.scss__ - adjust colors and rules to to suffice your needs. You may compile scss with __extensions/woocommerce/sass-watch.bat__ locally, if you have node.js and node-sass installed. Optionaly you can edit __extensions/woocommerce/assets/css/styles.css__.

## Check for bugs and gliches

+ If you have Cherry3-theme - use [this repository](#)
+ If you have Cherry5-theme - use [this repository](#)

## Files description(respectively to <em>extensions/woocommerce/</em>)

* __main.php__ - main "wrapper" class.
* __assets\css\styles.css__ - styles.
* __assets\css\styles.css.map__ - map-file for scss - styles generated by node-sass via __sass-watch.bat__.
* __sass-watch.bat__ - batch-file, starts watch task over __assets\scss\styles.scss__ file.
* __assets\js\scripts.js__ - contains package __Java Script__ modules.
* __inc\class-tgm-plugin-activation.php__ - [TGM-Plugin-Activation](http://tgmpluginactivation.com/) third party __PHP__- script used for automated _WordPress_ plugins instalation/activation.
* __templates\multilingual-menu.php__ - Template part. In case of [__WPML__](https://wpml.org/) plugin usage. Should render currency switcher and language switcher.
* __templates\shop-nav-menu.php__ - Renders 'Shop menu', if one attached to theme location.
	<br>_Notice!: __Renders only 1'st level of menu!___
* __templates\shopping-cart-link.php__ - Sopping Cart Navbar.
* __templates\template-shop.php__ - Static page template.
* __languages\en.pot__ - Translation asset (_untested_).
* __assets\scss\styles.scss__ - style.css source.
