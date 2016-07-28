# Multiple Domain

Multiple Domain allows you having more than one domain in a single WordPress installation. Differently than WordPress 
Multisite (MU), this plugin doesn't support a different theme or advanced customizations for each domain. It's only 
intended to enable constant navigation under a different domain than the one you set under WordPress options.

Without this plugin, if you have more than one domain set in your host, when a user access the website from a domain 
that is not the one defined in WordPress settings, all links will point to the default domain. After installing this 
plugin, you can set other domains and all links will be updated on the flow to use those domains.

This way, the user navigation will be end-to-end under the same domain.

You can also set an optional base URL. If you want to only a set of URL's to be accessed under a given domain, you can 
use this restriction.

## Installation

Follow the steps below to install the plugin:

1. Upload the plugin files to the `/wp-content/plugins/multiple-domain` directory, or install the plugin through the 
    WordPress plugins screen directly.
2. Activate the plugin through the 'Plugins' screen in WordPress
3. Use the Settings -> General screen to configure your additional domains

## Frequently Asked Questions

**Does this plugin set extra domains within my host?**

No. You have to set additional domains, DNS, and everything else to use this domain.

**Can I have a different theme/content/plugins for each domain?**

Nope. If you want a complex set up like this, you may be interested in WordPress Multisite. It's delivered with every 
WordPress installation since 3.0, you can find more info here: https://codex.wordpress.org/Create_A_Network.

**There is a way to add domain based logic to my themes?**

Absolutely. You can use the `MULTPLE_DOMAIN_DOMAIN` and `MULTPLE_DOMAIN_ORIGINAL_DOMAIN` constants to get the current 
and original domains. Just notice that since the value of the first one is checked against plugin settings, it may not 
reflect the actual domain in `HTTP_HOST` element from `$_SERVER` or user's browser. They also may include the host port 
when it's different than 80 (default HTTP port) or 443 (default HTTPS port).

**Can I create a custom access restriction logic for each domain?**

Yes. You can use the `multiple_domain_redirect` action to do that. For example:

    function my_custom_redirect($domain)
    {
        // Do nothing if the request is using the original domain.
        if ($domain === MULTPLE_DOMAIN_ORIGINAL_DOMAIN) {
            return;
        }
        // If the URI doesn't start with /cool/path, redirect the user to the original domain.
        if (!empty($_SERVER['REQUEST_URI']) && !preg_match('/^\/cool\/path/i', $_SERVER['REQUEST_URI'])) {
            wp_redirect('http://' . MULTPLE_DOMAIN_ORIGINAL_DOMAIN . '/');
            exit;
        }
    }
    
    add_action('multiple_domain_redirect', 'my_custom_redirect');

## Changelog

### 0.3

* Fixed bug when removing the port from current domain.
* Added `MULTPLE_DOMAIN_ORIGINAL_DOMAIN` constant to hold the original WP home domain.
* Allowing developers to create custom URL restriction logic through `multiple_domain_redirect` action.
* ...

### 0.2

* Improved port verification.
* Added `MULTPLE_DOMAIN_DOMAIN` constant for theme/plugin customization.
* And, last but not least, code refactoring.

### 0.1

This is the first release. It supports setting domains and an optional base URL for each one.

## Meta

Contributors: gustavostraube  
Tags: multiple, domains, redirect  
Requires at least: 4.0  
Tested up to: 4.5.3  
Stable tag: trunk  
License: GPLv2 or later  
License URI: http://www.gnu.org/licenses/gpl-2.0.html  
