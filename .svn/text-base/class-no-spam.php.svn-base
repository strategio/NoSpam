<?php
/**
 * No Spam
 *
 * @package   No_Spam
 * @author    Pierre SYLVESTRE <strategio@strategio.fr>
 * @license   GPL-2.0+
 * @link      http://www.strategio.fr
 * @copyright 2013 Strategio
 */

/**
 * Plugin class.
 *
 * @package No_Spam
 * @author  Pierre SYLVESTRE <strategio@strategio.fr>
 */
class No_Spam {

	/**
	 * Plugin version, used for cache-busting of style and script file references.
	 *
	 * @since   1.0.0
	 *
	 * @var     string
	 */
	protected $version = '1.0.0';

	/**
	 * Unique identifier for your plugin.
	 *
	 * Use this value (not the variable name) as the text domain when internationalizing strings of text. It should
	 * match the Text Domain file header in the main plugin file.
	 *
	 * @since    1.0.0
	 *
	 * @var      string
	 */
	protected static $plugin_slug = 'no_spam';

	/**
	 * Instance of this class.
	 *
	 * @since    1.0.0
	 *
	 * @var      object
	 */
	protected static $instance = null;

	/**
	 * Slug of the plugin screen.
	 *
	 * @since    1.0.0
	 *
	 * @var      string
	 */
	protected $plugin_screen_hook_suffix = null;

	/**
	 * Plugin options
	 *
	 * @since    1.0.0
	 *
	 * @var      string
	 */
	protected $options = array(
							'nospam_field_ID' => 'ns-nospam',
							'blahblah_field_ID' => 'ns-blahblah',
						);

	/**
	 * Initialize the plugin by setting localization, filters, and administration functions.
	 *
	 * @since     1.0.0
	 */
	private function __construct() {

		$saved_options = get_option(self::$plugin_slug.'_options');
		if(is_array($saved_options))
			$this->options = array_merge($this->options, $saved_options);

		// Load plugin text domain
		add_action( 'init', array( $this, 'load_plugin_textdomain' ) );

		// Add the options page and menu item.
		// add_action( 'admin_menu', array( $this, 'add_plugin_admin_menu' ) );

		// Load admin style sheet and JavaScript.
		//add_action( 'admin_enqueue_scripts', array( $this, 'enqueue_admin_styles' ) );
		//add_action( 'admin_enqueue_scripts', array( $this, 'enqueue_admin_scripts' ) );

		// Load public-facing style sheet and JavaScript.
		//add_action( 'wp_enqueue_scripts', array( $this, 'enqueue_styles' ) );
		//add_action( 'wp_enqueue_scripts', array( $this, 'enqueue_scripts' ) );

		// Hooking plugin functionnalities
		add_filter( 'comment_form_default_fields', array( $this, 'add_blahblah_input_field' ) );
		add_action( 'comment_form_after', array( $this, 'input_field_script' ) );
		add_filter( 'pre_comment_approved', array( $this, 'check_form_fields') , '99', 2 );
		add_filter( 'comment_form_before_fields', array( $this, 'add_comment_notice') );

	}

	/**
	 * Return an instance of this class.
	 *
	 * @since     1.0.0
	 *
	 * @return    object    A single instance of this class.
	 */
	public static function get_instance() {

		// If the single instance hasn't been set, set it now.
		if ( null == self::$instance ) {
			self::$instance = new self;
		}

		return self::$instance;
	}

	/**
	 * Fired when the plugin is activated.
	 *
	 * @since    1.0.0
	 *
	 * @param    boolean    $network_wide    True if WPMU superadmin uses "Network Activate" action, false if WPMU is disabled or plugin is activated on an individual blog.
	 */
	public static function activate( $network_wide ) {
			$options = array(
				'nospam_field_ID' => self::generate_field_ID(),
				'blahblah_field_ID' => self::generate_field_ID(),
			);

		update_option( self::$plugin_slug.'_options', $options);
	}

	/**
	 * Generate a random text-field like id
	 *
	 * @since    1.0.0
	 *
	 * @return string
	 */
	public static function generate_field_ID() {
		$length = mt_rand(6,12);
		$chars = 'abcdefghijklmnopqrstuvwxyz';
		$field_ID = '';

		for ($i=0; $i < $length; $i++) {
			$field_ID .= substr($chars, mt_rand(0,25), 1);
		}

		return $field_ID;
	}

	/**
	 * Fired when the plugin is deactivated.
	 *
	 * @since    1.0.0
	 *
	 * @param    boolean    $network_wide    True if WPMU superadmin uses "Network Deactivate" action, false if WPMU is disabled or plugin is deactivated on an individual blog.
	 */
	public static function deactivate( $network_wide ) {
		delete_option( self::$plugin_slug.'_options');
	}

	/**
	 * Load the plugin text domain for translation.
	 *
	 * @since    1.0.0
	 */
	public function load_plugin_textdomain() {

		$domain = self::$plugin_slug;
		$locale = apply_filters( 'plugin_locale', get_locale(), $domain );

		load_textdomain( $domain, WP_LANG_DIR . '/' . $domain . '/' . $domain . '-' . $locale . '.mo' );
		load_plugin_textdomain( $domain, FALSE, dirname( plugin_basename( __FILE__ ) ) . '/lang/' );
	}

	/**
	 * Register and enqueue admin-specific style sheet.
	 *
	 * @since     1.0.0
	 *
	 * @return    null    Return early if no settings page is registered.
	 */
	public function enqueue_admin_styles() {

		if ( ! isset( $this->plugin_screen_hook_suffix ) ) {
			return;
		}

		$screen = get_current_screen();
		if ( $screen->id == $this->plugin_screen_hook_suffix ) {
			wp_enqueue_style( self::$plugin_slug .'-admin-styles', plugins_url( 'css/admin.css', __FILE__ ), array(), $this->version );
		}

	}

	/**
	 * Register and enqueue admin-specific JavaScript.
	 *
	 * @since     1.0.0
	 *
	 * @return    null    Return early if no settings page is registered.
	 */
	public function enqueue_admin_scripts() {

		if ( ! isset( $this->plugin_screen_hook_suffix ) ) {
			return;
		}

		$screen = get_current_screen();
		if ( $screen->id == $this->plugin_screen_hook_suffix ) {
			wp_enqueue_script( self::$plugin_slug . '-admin-script', plugins_url( 'js/admin.js', __FILE__ ), array( 'jquery' ), $this->version );
		}

	}

	/**
	 * Register and enqueue public-facing style sheet.
	 *
	 * @since    1.0.0
	 */
	public function enqueue_styles() {
		wp_enqueue_style( self::$plugin_slug . '-plugin-styles', plugins_url( 'css/public.css', __FILE__ ), array(), $this->version );
	}

	/**
	 * Register and enqueues public-facing JavaScript files.
	 *
	 * @since    1.0.0
	 */
	public function enqueue_scripts() {
		wp_enqueue_script( self::$plugin_slug . '-plugin-script', plugins_url( 'js/public.js', __FILE__ ), array( 'jquery' ), $this->version );
	}

	/**
	 * Register the administration menu for this plugin into the WordPress Dashboard menu.
	 *
	 * @since    1.0.0
	 */
	public function add_plugin_admin_menu() {

		$this->plugin_screen_hook_suffix = add_plugins_page(
			__( 'No Spam', self::$plugin_slug ),
			__( 'No Spam', self::$plugin_slug ),
			'read',
			self::$plugin_slug,
			array( $this, 'display_plugin_admin_page' )
		);

	}

	/**
	 * Render the settings page for this plugin.
	 *
	 * @since    1.0.0
	 */
	public function display_plugin_admin_page() {
		include_once( 'views/admin.php' );
	}

	/**
	 * Add a nospam input using javascript
	 *
	 * @since    1.0.0
	 */
	public function input_field_script() {
		$nospamID = $this->options['nospam_field_ID'];

		echo '<script type="text/javascript">
					(function(window){
						var form = document.getElementById("commentform");
						var input = document.createElement("input");
		                input.type = "hidden";
		                input.name = "'.$nospamID.'";
		                form.appendChild(input);
		                var notice = document.getElementById("nospam-notice");
		                if(notice) {
							notice.parentNode.removeChild(notice);
		                }
					})(window);
				</script>';
	}

	/**
	 * Add a blahblah extra field to comment form
	 *
	 * @since    1.0.0
	 */
	public function add_blahblah_input_field($fields) {
		$blahblahID = $this->options['blahblah_field_ID'];

		$newfields = array('blahblah' => '<p class="comment-form-'.$blahblahID.'" style="display:none!important;"><label for="'.$blahblahID.'">' . ucfirst($blahblahID) . ' <span class="required">*</span></label>
        			<input id="'.$blahblahID.'" name="'.$blahblahID.'" type="text" value="" size="30" /></p>');

    	return array_merge($newfields, $fields);
	}
 
	/**
	 * Check if the blahblah field is still empty
	 * Check if the nospam field is set
	 *
	 * @since    1.0.0
	 *
	 * @param mixed 0|1|spam
	 * @param array
	 */
	public function check_form_fields( $approved , $commentdata ){
		$blahblahID = $this->options['blahblah_field_ID'];
		$nospamID = $this->options['nospam_field_ID'];

		// Check if the blahblah field
		if(isset($_POST[$blahblahID]) && $_POST[$blahblahID] != ''){
			$approved = 'spam';
		}

		// Check if the nospam field
		if(!isset($_POST[$nospamID])){
			$approved = 'spam';
		}

		return $approved;
	}
	
	/**
	 * Add a notice for visitors in case javascript is not activated
	 *
	 * @since    1.0.0
	 *
	 * @param string
	 */
	public function add_comment_notice($notes){
		echo '<p id="nospam-notice">'.__('Please enable javascript to post a comment !').'</p>';
	}
}