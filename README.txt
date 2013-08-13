=== No Spam ===
Donate link: http://nospam.strategio.fr/
Tags: comments, spam, anti-spam, spam-bots
Requires at least: 3.5.1
Tested up to: 3.6
Stable tag: 1.0.0
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

A simple and efficient anti-spam plugin

== Description ==

No Spam is a simple, lightweight and efficient anti-spam plugin.

It relies on differences between humans and robots when they visit (or crawl) a page.

1. Most of human visitors clients are javascript enabled and most of spam bot are not. Then the No Spam plugin adds an input field in the comment form using javascript. After submission, the plugin check if this field exists.

2. As spam bots usually don't kown which fields are required and which are not, they use to fill all the fields. The plugin adds a extra field (with empty value) in the comment form and hide it with CSS styling. After submission, the plugin check if the field is still empty.

== Installation ==


1. Upload `nospam` to the `/wp-content/plugins/` directory
1. Activate the plugin through the 'Plugins' menu in WordPress

== Frequently Asked Questions ==

= A question that someone might have =

An answer to that question.

= What about foo bar? =

Answer to foo bar dilemma.

== Screenshots ==


== Changelog ==

= 1.0 =
* First stable version

== Upgrade Notice ==

= 1.0 =
* Get the first No Spam plugin release