<?php
/**
 * The WordPress Plugin Boilerplate.
 *
 * A foundation off of which to build well-documented WordPress plugins that also follow
 * WordPress coding standards and PHP best practices.
 *
 * @package   No_Spam
 * @author    Pierre SYLVESTRE <strategio@strategio.fr>
 * @license   GPL-2.0+
 * @link      http://www.strategio.fr
 * @copyright 2013 Strategio
 *
 * @wordpress-plugin
 * Plugin Name: No Spam
 * Plugin URI:  http://nospam.strategio.fr/
 * Description: A simple and efficient anti-spam plugin (based on dummy input and javascript enabled client)
 * Version:     1.0.2
 * Author:      Pierre SYLVESTRE
 * Author URI:  http://nospam.strategio.fr/
 * Text Domain: no_spam
 * License:     GPL-2.0+
 * License URI: http://www.gnu.org/licenses/gpl-2.0.txt
 * Domain Path: /lang
 */

// If this file is called directly, abort.
if ( ! defined( 'WPINC' ) ) {
	die;
}

require_once( plugin_dir_path( __FILE__ ) . 'class-no-spam.php' );

// Register hooks that are fired when the plugin is activated, deactivated, and uninstalled, respectively.
register_activation_hook( __FILE__, array('No_Spam', 'activate') );
register_deactivation_hook( __FILE__, array('No_Spam', 'deactivate') );

No_Spam::get_instance();