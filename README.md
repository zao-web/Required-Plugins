Required Plugins
=========

A library that allows a theme or plugin to filter the list of required plugins so that:
* The deactivate links are removed.
* Plugins are automatically activated (if they are in the plugins directory)
* Plugin is listed with a 'required' label.

Library also allows black-listing plugins, useful to ensure development plugins are not activated on production.

To use, place this library in your mu-plugins/ directory (if you don't have one, create one in wp-content/), then use the example below:

#### Example Usage:
```php
<?php

require WPMU_PLUGIN_DIR . '/Required-Plugins/wp-auto-required-plugins.php';

/**
 * Add required plugins to Required_Plugins
 *
 * @param  array $required Array of required plugins in `plugin_dir/plugin_file.php` form
 *
 * @return array           Modified array of required plugins
 */
function wds_required_plugins_add( $required ) {

	$required = array_merge( $required, array(
		'jetpack/jetpack.php',
		'sample-plugin/sample-plugin.php',
	) );

	return $required;
}
add_filter( 'wds_required_plugins', 'wds_required_plugins_add' );
// Or network-activate/require them:
// add_filter( 'wds_network_required_plugins', 'wds_required_plugins_add' );
```

#### Modification:
To change the label from 'Required Plugin', use the following filter/code.

```php

/**
 * Modify the required-plugin label
 *
 * @param  string  $label Label markup
 *
 * @return string         (modified) label markup
 */
function change_wds_required_plugins_text( $label ) {

	$label_text = __( 'Required Plugin for ACME', 'acme-prefix' );
	$label = sprintf( '<span style="color: #888">%s</span>', $label_text );

	return $label;
}
add_filter( 'wds_required_plugins_text', 'change_wds_required_plugins_text' );
```

##### Hide from the Plugin list
To hide your required plugins from the plugins list, use the following filter/code.

```php
add_filter( 'required_plugin_remove_from_list', '__return_true' );
```

#### Changelog
* 0.1.6
	* Add ability to remove plugins from the plugin list, if desired.
* 0.1.5
	* New filters for blacklisting plugins, `'blacklisted_plugins'` and `'network_blacklisted_plugins'`.
* 0.1.4
	* Will now log if/when a required plugin is not found.
	* New filters:
		* `'required_plugin_auto_activate'` - By default required plugins are auto-activated. This filter can disable that.
		* `'required_plugin_log_if_not_found'` - By default, missing required plugins will trigger an error in your log. This filter can disable that.
		* `'required_plugins_error_log_text'` - Filters the text format for the log entry.
* 0.1.3
	* Network activation filter
* 0.1.2
	* i10n
* 0.1.1
	* Automatically activate required plugins (if they are available).
* 0.1.0
	* Hello World.
