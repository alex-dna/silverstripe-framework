# Translation Process #

This page covers a few advanced topics related to SilverStripe's translation system.

To find out about how to assist with translating SilverStripe, see the [Contributing Translations page](contributing/translation).

## Set up your own module for localization

### Collecting translatable text

As a first step, you can automatically collect
all translatable text in your module through the `i18nTextCollector` task.
See [i18n](/topics/i18n#collecting-text) for more details.

### Import master files

If you don't have an account on transifex.com yet, [create a free account now](http://www.transifex.com/signup).
After creating a new project, you have to upload the `en.yml` master file as a new "Resource".
While you can do this through the web interface, there's a convenient 
[commandline client](http://support.transifex.com/customer/portal/topics/440187-transifex-client/articles)
for this purpose. In order to use it, set up a new `.tx/config` file in your module folder:

	[main]
	host = https://www.transifex.com

	[my-project.master]
	file_filter = lang/<lang>.yml
	source_file = lang/en.yml
	source_lang = en
	type = YML

If you don't have existing translations,
your project is ready to go - simply point translators
to the URL, have them sign up, and they can create languages and translations as required.

### Import existing translations

In case you have existing translations in YML format,
there's a "New language" option in the web interface.
Alternatively, use the [commandline client](http://support.transifex.com/customer/portal/topics/440187-transifex-client/articles).

### Export existing translations

You can download new translations in YML format through the web interface,
but that can get quite tedious for more than a handful of translations.
Again, the [commandline client](http://support.transifex.com/customer/portal/topics/440187-transifex-client/articles)
provides a more convenient interface here with the `tx pull` command, downloading all translations as a batch.

### Merge back existing translations

If you want to backport translations onto release branches, simply run the `tx pull` command on multiple branches.
This assumes you're adhereing to the following guidelines:

 - For significantly changed content of an entity, create a new entity key
 - For added/removed placeholders, create a new entity
 - Run the `i18nTextCollectorTask` with the `merge=true` option to avoid deleting unused entities
   (which might still be relevant in older release branches)

### Converting your language files from 2.4 PHP format

The conversion from PHP format to YML is taken care of by a module
called [i18n_yml_converter](https://github.com/chillu/i18n_yml_converter).

## Download Translations from Transifex.com

We are managing our translations through a tool called [transifex.com](http://transifex.com).
Most modules are handled under the "silverstripe" user,
see [list of translatable modules](https://www.transifex.com/accounts/profile/silverstripe/).

Translations need to be reviewed before being committed,
which is a process that happens roughly once per month.
We're merging back translations into all supported release branches as well as the `master` branch.
The following script should be applied to the oldest release branch, and then merged forward into newer branches:
	
	tx pull
	# Manually review changes through git diff
	git add lang/*
	git commit -m "Updated translations"

You can download your work right from Transifex in order to speed up the process for your desired language.

# Related

 * [i18n](/topics/i18n): Developer-level documentation of Silverstripe's i18n capabilities
 * [contributing/translation](contributing/translation): Information for translators looking to contribute translations of the SilverStripe UI.
 * [translatable](https://github.com/silverstripe/silverstripe-translatable): DataObject-interface powering the website-content translations
 * ["Translatable ModelAdmin" module](http://silverstripe.org/translatablemodeladmin-module/): An extension which allows translations of DataObjects inside ModelAdmin