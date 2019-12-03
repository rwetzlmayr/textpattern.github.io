---
layout: document
category: Themes
published: true
title: Themes: A complete walk-through
description: A complete journey through creating, managing, using, exporting, importing, and deleting Textpattern themes. Join the expedition.
---

# Themes: A complete walk-through

This document is a complete journey through creating, managing, using, exporting, importing, and deleting Textpattern themes, all in context of default installation conditions, but kindly extrapolated to situations that aren’t. It just happens to be that default can go a long way.

**On this page**:

* Table of contents
{:toc}

## The big picture

Textpattern users, and theme designers in general, new and old, will find Textpattern theming exciting. Being that Textpattern is unlike any other content management system, so is the theming functionality and workflow it provides. Before diving in with how to do things, which you’ll have mastered by the end of this document, consider it all from higher up the ladder.

### A theme development ‘studio’

The addition of the [Themes](https://docs.textpattern.com/administration/themes-panel) panel has essentially transformed how to think about building website architectures with Textpattern’s semantic building blocks. Textpattern designers will likely find the Themes panel is now the centre stage of the administration side. Indeed, the back-end is like a theme development studio, where you can work on many themes simultaneously, for employing on your website or sharing with the community. But don’t worry, writers and editors, Textpattern is still the same system to do what its creator envisioned. ‘Just write.’

Theoretically speaking, you could hack a theme package together, from scratch and by hand, directly on your web server. We know you masochistic types are out there. And, sure, it can be a learning experience to sit on the cold garage floor with grease and gears.

Most of this document, however, describes the sensible approach of using the new theming functionality in the back-end. Be at ease knowing that creating a new theme has no impact on a website’s operation or presentation if the pages and styles of the theme are not yet associated to the website’s functioning sections (easy to tell at a glance, as you will see). If not, you can freely modify any [theme package assets](#theme-package) without worry.

### Default system references

In addition to the document you are reading, and your own installation of the software, the following resources make for handy references to default system conditions in case you go astray, or in the event you do not have an install of your own yet, in fact. Make note of these:

* Front-end demo of default theme:
  * [Current stable release of theme](https://release-demo.textpattern.co/)
  * [Upcoming release of theme](https://dev-demo.textpattern.co/)  (in development)
* [Back-end demo of fresh software install](https://www.textpattern.co/demo) (first copy the username/password detail, then proceed to the ‘Admin’ version of choice)
* [Package assets: pages, forms, styles, metadata](https://github.com/textpattern/textpattern-default-theme) (go to *dist/{name-of-them}*)

The front-end and back-end demos are routinely updated. The back-end demo offers a choice between the current version release and the next one under development.

### Author prefix and registration

If you intend to create themes to share with the community, you must [register and use an author prefix](https://docs.textpattern.com/brand/author-prefixes-and-registration). Your chosen prefix must not already be taken, as indicated by the prefix lists. Having and using a personal prefix ensures no two themes will ever be created with the same *name*, thus tripping Textpattern up.

While prefixes are the same whether you create themes or develop plugins, the naming convention is different in each case to make it clear at a glance what product a name is representing. Basically, theme names use hyphens, and plugin names use underscores. If your prefix is *abc*, for example, your theme names would look like this: 

* {:.directory} **abc-theme-name**
{:.list--files}

And your plugin names, if you developed any, would look like this, as always:

* {:.directory} **abc_plugin_name**
{:.list--files}

### Theme package

A Textpattern theme is a collection of *page* templates, *form* markup, stylesheets, and a manifest file with a little metadata; all packaged together in a single directory bearing the theme’s name. The single directory can be thought of as a ‘package’ and its contents can be thought of as ‘assets’ organized in subdirectories by type.

To ensure a theme imported into your installation does not break your front-end, Textpattern augments imported themes with any missing assets needed by the default system, and provides empty placeholders where non-essential assets are missing. This is useful, say, when the grease-monkeys build from scratch and the integrity of a given theme’s structure is questionable.

Following is the expanded tree of a default package structure:

* {:.directory--open} abc-theme-name
  * {:.directory--open} pages
    * {:.document} archive.txp
    * {:.document} default.txp
    * {:.document} error_default.txp
  * {:.directory} forms
    * {:.directory--open} article
      * {:.document} article_listing.txp
      * {:.document} default.txp
      * {:.document} search_results.txp
    * {:.directory} category (empty)
    * {:.directory--open} comment
      * {:.document} comments.txp
      * {:.document} comments_display.txp
      * {:.document} comment_form.txp
    * {:.directory--open} file
      * {:.document} files.txp
    * {:.directory--open} link
      * {:.document} plainlinks.txp
    * {:.directory} misc (empty)
    * {:.directory} section (empty) 
  * {:.directory--open} styles
    * {:.document} default.css
  * {:.document} manifest.json
{:.list--files}

Of the entire package, only two page templates and the metadata file are ‘essential’:

* {:.directory--open} abc-theme-name
  * . . .
    * . . .
    * {:.document} default.txp
    * {:.document} error_default.txp
  * . . .
  * {:.document} manifest.json
{:.list--files}

Theme designers would doubtfully ever share such a worthless theme, but if one did, and you actually wanted to use it, here’s how Textpattern would treat it at time of [importing](#importing-themes):

1. **Pages**. If the essential page templates were missing, they would be created as empty pages. If the entire *pages* directory was missing, it would be created, and the two essential pages included with their default contents.
2. **Forms**. If non-essential form files are missing, they are added in place as empty files. If any form subdirectories are missing, or the entire *forms* directory, the full *forms* directory from the tree above will be created, and the eight form files will contain default Textpattern markup.
3. **Styles**. If the file is missing, it is added as an empty file. If the *styles* directory is missing, the directory plus a blank *default.css* file will be created.
4. **Metadata**. The metadata file is handled more simply; the focus is on any missing metadata values. See [Metadata](#metadataa).

When [updating](#updating-themes) such an augmented theme, such as when another theme author releases a new version of a theme you’ve already imported, and the theme’s file composition has not changed from the original imported version, Textpattern will ignore the missing files because it has already added them in your database. If, however, the theme author *does* add new files to their theme that were missing originally, they will replace the placeholders Textpattern originally added, and the theme is one step better to being a real functioning theme.

If, after importing or updating a sparse theme as described, you later delete any of the augmented files, and that's your prerogative, and exported it that way for sharing, those files will remain missing in the exported theme. In other words, you would be sharing a worthless theme just like what was shared with you. But that’s fine, because it would not break anything when someone else imported it; their install of Textpattern would augment the theme with defaults and placeholders again just like described.

#### Page assets

[Pages](https://docs.textpattern.com/administration/pages-panel) are your web page templates, primarily built with HTML and [Textpattern tags](https://docs.textpattern.com/tags). Pages may be constructed solely with embedded markup, or by interchanging markup with form ‘includes’ (using [output_form](https://docs.textpattern.com/tags/output_form) tags), or as a combination of embedded markup and form includes. Pages are assigned to one or more of your website’s sections. A one-to-many relationship between pages and sections, achieved with forms and conditional tag logic, can result in a site architecture that’s easier to manage and maintain. *Definitely* learn Textpattern tags!

#### Form assets 

[Forms](https://docs.textpattern.com/administration/forms-panel) are the named containers of markup components (conceptually the same as partials, snippets, or includes) for *inclusion* into page templates, or for nesting into other forms. Such ‘Russian doll’ construction of your website’s architecture using pages and forms enables building sophisticated website structures that are, again, easier to manage and maintain. See [Form templates explained](https://docs.textpattern.com/themes/form-templates-explained.md) if you need a deeper dive into forms.

All form files must have unique names — across core forms and any custom forms created — and core form names will never be changed from their defaults. Compound form names use underscores between words (e.g. <i>**custom_form_name**</i>) and the resulting template file name should match (i.e. <i>custom_form_name.txp</i>).

#### Style assets

Of course you know what stylesheets are; they need no further explanation except to emphasize that, like pages, they must be associated to one or more sections of your website in order to rule the presentation of them.

#### Metadata

The essential metadata rides with the package as a <i>manifest.json</i> file, in which the following six metadata items are found:

* title
* version
* description
* author
* author_uri
* txp-type

The first five items reflect data pull from the [New themes](#new-themes) functionality in the Themes panel. The `title` is always required so a value will always be present in a theme package. The `txp-type` and its associated value is a constant; it must always exist and never change. The remaining four items reflect optional metadata fields, which nevertheless should be filled in by theme authors at time of creation when the data is known. If skipped, however, Textpattern will provide default values for the `version` and `author` items at time of theme creation; so these values will pass with an imported theme if not changed by the author, and the `description` and `author_uri` (i.e. the Website field in the creation form) items are treated as `none` values in imported themes.

Altogether, a minimum-filled set of metadata for a theme would look like follows, where ‘username’ would be the theme author’s log in username for their Textpattern installation:   

```json
{
"title": "My New Theme",
  "version": "0.0.1",
  "description": "none",
  "author": "username",
  "author_uri": "none",
  "txp-type": "textpattern-theme"
  }
```

### Scope of possibilities

Textpattern has always allowed you to create a different *presentation* for your website by merely editing the default stylesheet, or using one or more static files on your web server. But with the inclusion of themes functionality, you can make and collect many theme packages that can be:

* Swapped in and out on your entire website
* Used to create different presentations; one for each site section
* Used to create different site structures, to a certain degree
* Shared with the community, so others can benefit from your savvy design sense.

Now we descend the ladder and get closer to the action! Everything from here on is framed in context of a fresh (out-of-the-box) install of the software, but the principles are the same whether your installation is brand new or an old workhorse that’s up to date. As long as you’re using the [latest release](https://textpattern.com/start) of the software, at least, it will make sense. Any exceptions to this rule are made clear in context of instructions.

## New themes

Multiple options are available for creating a new theme via the [Themes](https://docs.textpattern.com/administration/themes-panel) panel. You can:

* Create a new theme directly via the **New theme** form
* Duplicate a theme via the **With selected** controls
* Duplicate a theme via the **Edit theme** form.

All the approaches accomplish the same thing, which is establish a new theme to start working on. The approaches also use the same functionality, but just in different order and contexts (e.g. the **New theme** form and **Edit theme** form are the same, but accessed differently). 

Let’s look at each one in default system context.

### Create via the ‘New theme’ form

Creating a theme via the **New theme** form is the most called-out way to get started by evidence of the prominent button bearing the same label right at top of the Themes panel.

![Click New theme button](https://docs.textpattern.com/img/click-new-theme-button.png)

The **New theme** button opens the associated form. The form is composed of six metadata fields, the first two of which are required:

* Theme name (required) 
* Theme title (required)
* Theme version
* Theme description
* Theme author
* Theme website 

The six fields you see in the editing form correspond to four columns in the theme’s table on the main panel (Name, Title, Version, Author), and five of the metadata values in [the <i>manifest.json</i> file](#the-manifest-json-file).

The theme’s name and title are not the same. If you registered an [author prefix](#author-prefixes-and-registration), you should use that as the first part of the theme’s name (e.g. ‘abc-my-new-theme’). The title, on the other hand, like for a book, is the proper, ‘human-readable’ name and should be entered in capital case with no hyphens (i.e. ‘Title Like This’). You do not have to use your prefix in the title, only for the name; so you could have this pairing, for example:

* Theme name: abc-my-new-theme
* Theme title: My New Theme

If you decide to skip the optional fields for whatever reason, Textpattern will automatically fill the version and author fields with the following default values:

* Theme version: 0.0.1
* Theme author: {username} (i.e. the name you log in with)

The remaining description and website fields will stay empty if not used. The website field is for adding a URL to where your theme can be downloaded online.

In any case, you should fill out the form as completely as possible, without relying on Textpattern’s defaults. The more complete the form, the more informative the manifest file will be. Any fields you can’t fill immediately, such as the website URL, can always be filled in later.

When the **New theme** form is saved, the resulting theme appears in the themes table.

![Active theme indicator](https://docs.textpattern.com/img/active-theme-indicator.png)

The two other approaches that get to this point, both by way of theme duplication, are described next. Or jump to the [Active versus non-active themes](#active-versus-non-active-themes) section to continue on the current journey.   

### Duplicating themes

Using an existing theme package as a guide for your efforts is a good way to get started with making themes. There’s no better package to use in this case than the default theme that comes with Textpattern, already shown and functioning in the Themes panel upon installing the software.

This does not mean start working directly on the default theme, or you will be doing development on an active (live) theme and changing its nature. Instead, duplicate the default theme and turn it into a new one.

While these instructions describe using the default theme (the only theme available in an out-of-the-box context of the software), you could duplicate any existing theme in the Themes panel; such as one that might have the kind of website structure you want to work closer with.

The following two approaches to duplication are the same regardless of what theme you duplicate.

#### Duplicate via the ‘With selected’ control

One way to duplicate the default theme is via the **With selected** controls that work in combination with the check boxes in the themes table.

Proceed by checking the box next to the default theme in the themes table, then selecting the ‘Duplicate’ option from the **With 1 selected**… menu just below the table, and, finally, clicking ‘OK’ when asked if you are sure. 

![Duplicate via with selected controls](https://docs.textpattern.com/img/duplicate-via-with-selected-controls.png)

The duplicated theme is added to the themes table and the name appears with ‘-copy’ on it. You can now uncheck the default theme’s box in the table.

Now is also a good time to change the name, title, and other metadata fields on the duplicated theme. Do this by clicking the theme’s name in the Name column of the themes table to open the **Edit theme** form (same as the **New theme** form but in different context).

![Click default theme name](https://docs.textpattern.com/img/click-default-theme-name.png)

Change the metadata in the six fields as thoroughly as possible. See [Create via the ‘New theme’ form](#create-via-the-new-theme-form) for explanation of the fields. If you don’t have a value to give for an optional field, clear the field’s value so it’s entirely blank. You can always fill it in later. When finished with changing all the metadata on your duplicated theme, save and close the form.

![Save edit theme form changes](https://docs.textpattern.com/img/save-edit-theme-form-changes.png)

(The ‘Duplicate’ link you see in the image above is a hint to the next section.)

The duplicated theme will be added to the themes table in the main panel view. Once the default theme is duplicated and the metadata changed, the duplicated theme is, for all practical purposes, a *new* theme.

#### Duplicate via the ‘Edit theme’ form

The last way to create a theme is the most direct (fewer clicks in the interface) yet least obvious way to *duplicate* a theme.

As before, click the name of the default theme under the Name column of the table.

![Click default theme name](https://docs.textpattern.com/img/click-default-theme-name.png)

As you now know, the **Edit theme** form will open up and show the default theme’s metadata so you can change it accordingly. See the process described in the previous section.

When done editing the form, click the ‘Duplicate’ link under the form, at right of the Cancel button.

![Click duplicate link](https://docs.textpattern.com/img/click-duplicate-link.png)

There is no confirmation dialogue asking if you are sure. You are simply taken back to the default panel view where your new (revised) theme is added to the themes table.

![New theme created](https://docs.textpattern.com/img/new-theme-created.png)

## Contexts and associations

The administration panels provide features and cues that make it easy to see what theme context you are in from any relevant panel. This includes: whether a theme is active or not, what assets are associated to which themes, what themes are assigned to sections, and contextual navigation between it all. 

### Active / Non-active

You can tell from a glance in the Themes panel if a given theme is active or not. Look under the Sections column of the themes table. Any theme with ‘0’ section associations is not active (i.e. not live on the front-end).

![Sections column](https://docs.textpattern.com/img/sections-column.png)

When a theme is not active, you can work on the associated assets without concern for it impacting website visitors or any other problems.

In version 4.8, soon, a new feature is introduced. Under the Name column of the themes table, at right of the name, will be an ‘Active’ link. When the link is blue, the theme is not active. When the link is green, the theme is active.

![Active theme indicator](https://docs.textpattern.com/img/active-theme-indicator.png)

More about using this feature and what is happening behind the scenes is presented in [Same structure; different theme](#same-structure-different-theme).

### Assets association

Under the Pages, Forms, and Styles columns of the themes table are linked numbers indicating how many assets of each type are associated to a given theme.

![Theme-assets-links](https://docs.textpattern.com/img/assets-columns.png)

When you duplicate the default theme (as the next image depicts), or any other theme, the existing assets in the source theme are cloned as well, logically, thus why the duplicated themes have the same number.

![Sections and assets columns](https://docs.textpattern.com/img/sections-and-assets-columns.png)

Click any of these links and you are taken to the respective panels with the indicated number of assets in context. For example, clicking a ‘3’ under the Pages column for the duplicated theme lands you on the Pages panel with the associated three pages listed:

![Pages assets](https://docs.textpattern.com/img/pages-assets.png)

You can tell which theme the panel assets are in context with by the appearance of the Theme selection menu above the assets list, showing the title of the relevant theme.[^themebox]

![Theme menu context](https://docs.textpattern.com/img/theme-menu-context.png)

You can use the Theme selection menu to change theme context from any assets panel.

![Theme menu selection](https://docs.textpattern.com/img/theme-menu-selection.png)

You will remain in that different theme context as you browse between the assets panels. You can switch theme context again at any time from any assets panel by using the Theme menu again, or by returning to the Themes panel and clicking a number link for a given theme’s assets, as described above.

[^themebox]: The box does not appear in assets panels when the Themes panel only has one theme.

### Sections preview

(Forthcoming)

## Applying themes

When you are ready to apply your theme and see how it looks on the front-side, you need to assign it to one or more [sections](https://docs.textpattern.com/administration/sections-panel) that are setup and ready to go. It’s assumed here that your sections *are* ready to go, so focus is on the assigning part. How you do this might depend on what your theme objectives are.

### Switching themes

If you are using version 4.8 of the software, and you simply want to swap one theme for another across your website as it’s currently designed, return to the Themes panel and click the blue ‘Active’ link found at right of the desired theme’s name.

![Click blue active link](https://docs.textpattern.com/img/click-blue-active-link.png)

Confirm ‘OK’ when asked if you are sure. The theme’s ‘Active’ link then becomes green and the previously active theme’s link turns blue.

In situations where you have created the themes and know how your website is constructed (i.e. assets between the different themes are paired will and using similar names), this switching process should work fairly smoothly. If switching between themes that you have imported, however, where the variability of theme package contents is potentially greater between themes, Textpattern will switch those parts that are compatible (similarly named, etc.) and ignore the others. 

Obviously you do not want to use this feature if your own new theme is not ready, or not being developed locally or on a staging server. And you might need to fiddle a bit with themes imported from somewhere else. But the ‘Active’ link still makes it easier to switch from one theme to another across your entire site.

### Assigning multiple themes

In this case you might like keeping the same general structure of your website but want to have a different layout and presentation for each section. The blue ‘Activate’ links described previously will not work in this case because that only switches one theme to the whole website — an easy and quick theme switcher.

If you want a 1:1 assignment of a different theme to every website section, you will have to assign each theme and its associated page and style assets to the section individually. There are two approaches to this process, which is a one-section-at-a-time process no matter what since you want all sections looking different, but one approach is a little more direct than the other. 

Use the **With selected** controls introduced earlier in the duplicating themes sections, but this time from the context of the [Sections](https://docs.textpattern.com/administration/sections-panel) panel, where assigning assets to templates is done, and where the select options are different.

So, hop into the Sections panel, check the box by the name of a section to assign a theme too, and select ‘Change theme/page/style’ from the **With 1 selected…** menu.  

![Select ‘Change theme…’](https://docs.textpattern.com/img/select-change-theme.png)

Yes, you can select more than one section at a time and thereby assign a given theme to multiple sections at once. This would be the ideal middle road to take when the ‘Activate’ theme switcher was too much (all or nothing) and a 1:1 pairing is too tedious. Alas, you’re on a tedious mission here because each website section must look like an Alphonse Mucha season, so push forth with one section assignment at a time.

The selection will reveal an associated and uncharacteristically rich set of controls for completely defining the assignment action.

![Select ‘Change theme…’ option](https://docs.textpattern.com/img/select-change-theme-option.png)

First is not one but _two_ checkboxes: ‘Development’ and ‘Live’. The labels are clear, undoubtedly. ‘Development will keep the theme assignment offline until you say different another day.[^sect] ‘Live’ will make it public immediately. The former and safer option is checked by default, or you might be brave and switch to ‘Live’. Still, you could check both at once and see what happens. You won’t know until you try!

The check boxes are accompanied by three select menus for Theme, Page, and Style. The current theme assignment will show by default. When you change that menu first, the Page and Style menu options adjust in relation. This ensures you only choose from page and style assets in the theme package; no mistaking it.

Repeat the above process for each theme-to-section assignment. When done with all sections, return to the Themes panel to find that the blue ‘Activate’ links on the assigned themes have magically turned to shamrock green and beam ’**Active**{:.success}’. Luck of the Irish!  

[^sect]: A column now exists in the `txp_section` table for the development theme assets.

## ‘Database’ themes versus ‘disk’ themes

In the remaining sections, the exporting, importing, updating, and deleting processes are explained. Before getting there, a concept needs laid out for understanding. Some may snort at the obviousness of it. Others might appreciate it being made clear.

Textpattern interface strings, pophelp, and this documentation in relation — and probably even developers themselves, and you, eventually, when exchanging in the community forum — will occasionally use terms that refer to themes as either ‘database’ or ‘disk’. These distinguishing terms are in reference to themes at specific locations and contexts in the [importing](#importing-themes) and [deleting](#deleting-themes) scenarios, particularly.

As far as the location terminology goes: 

* **Themes panel = database theme**. When a theme is in the themes table of the Themes panel, it is in the database and thus can be referred to as a ‘database theme’.
* **<i>themes</i> directory = disk theme**. When the theme package is sitting in the <i>themes</i> directory, it is on the web server’s disk and thus referred to as a ‘disk theme’, ‘disk-based’ theme, or what have you.

When a theme is in both locations, as may often be the case in your workflows, remember to distinguish a given theme between the two locations, which is also useful when seeking help.

It all centres around the <i>themes</i> directory located in your software’s root installation location, which is best thought of as a checkpoint for incoming (imported) and outgoing (exported) themes:

* {:.directory--open} {root}
  * {:.directory} files
  * {:.directory} . . .
  * {:.directory} **themes** 	
  * {:.document} . . .
  * {:.document} index.php
  * {:.document} . . .
{:.list--files}

## Exporting themes

You have learned how to create and manage themes at this point, so take it to the next logical step and export one for other people to use. Fame and fortune awaits!

When your theme is tweaked to perfection and ready to be shared, go to the Themes panel, check the theme’s box in the table, and use the **With selected** controls to select ‘Export to disk’ (i.e. to the <i>themes</i> directory).

![Select export](https://docs.textpattern.com/img/select-export.png)

The selection will trigger another option to respond to by way of a checkbox: <q><i>Delete unused templates from disk on export</i></q>.

![Select export option](https://docs.textpattern.com/img/select-export-option.png)

If you read the help tip for that option, it says:

> If checked, each template file in the selected theme(s) that does not have a corresponding item in the database will be permanently deleted from the disk-based theme. The result after completion of the action will be that your file system templates will reflect exactly what is in the database.

In other words, Textpattern is making a comparison between the theme package you want to export from the Themes panel, presumably the package designed as it should be, and any similarly named theme that might be sitting in the <i>themes</i> directory but not so well put together. So it’s asking, if there is such a theme there in the directory, and any asset files in it do not have similarly named files in the panel version (the one you are exporting), do you want those files deleted as part of the exportation action?

This is generally a good idea, and the check box is ticked by default. This ensures that after exportation, there is a direct match between the database version (in the Themes panel) and the version on disk (in the <i>themes</i> directory).

Click the ‘Go’ button and confirm when asked if you are sure.

Now you are almost famous, but not quite yet. Still one more thing to do if you want your theme added to the new Themes website repository, coming soon. *Zip it up!* Textpattern will only host one file per theme package, and it must be compressed. So transfer the exported theme from the <i>themes</i> directory to an external location and make it a single, compressed <i>.gzip</i> or <i>.zip</i> file.

How you store the theme file anywhere else for sharing is up to you. But, no matter what location, Themes website or your own, make sure the expected URL has been entered in the Website field of the theme’s metadata. Add that by clicking the name of the theme in the themes table and using the **Edit theme** form.

## Importing themes

Unlike with exporting themes that you create, where the theme is already tabled in the Themes panel, importing requires first getting an externally sourced theme into the database — <i>themes</i> directory to the rescue!

First, use your (S)FTP software of choice to transfer the external theme (presumably downloaded from a shard location) to your <i>themes</i> directory on the server. A theme import control will appear in the Themes panel, above the themes table.

![Import theme menu](https://docs.textpattern.com/img/import-theme-menu.png)

If you didn’t see the menu before, it is because it works in context with the <i>themes</i> directory, as follows:

1. **One or more disk themes; no database equivalents.** When one or more themes are sitting in the <i>themes</i> directory that are not also existing in the themes table, they will cause the selection menu to appear at top of the themes table with those themes appearing as an options for importation (i.e. to become themes in the database). The menu is not visible if this condition is not true.
2. **Disk themes equal database themes.** When themes are equally in the database and on disk, the contextual menu is hidden, since there is nothing to potentially import.
3. **Database theme deleted; back to condition 1**. If/when you delete a theme from the database but not the disk-based version (assuming the previous condition), the theme import menu will appear again because the first condition is true again.

You can then use the import menu to select the theme and initially import it into the database, to appear in the themes table.

![Import theme menu select](https://docs.textpattern.com/img/import-theme-menu-select.png)

The theme is now in your themes table to use as desired, and if the themes in the table match the themes in your directory, the import control will disappear again from the Themes panel.  

## Updating themes

If you are more into importing than exporting, you will eventually come to the point where one or more of the themes you have imported needs updated. This is the case when a theme designer has revised the source theme and distributed a new and improved version. Not wanting to miss out on the cutting edge or potential security improvements, you dutifully update your version with the latest release.

When you update a theme from disk, Textpattern creates an *exact* copy in the database from the disk version you are updating from, merging new elements and overwriting existing ones having the same file name.

As with initially importing a theme, you begin by downloading and transferring the new version of the sourced theme to your <i>themes</i> directory. Then it is on to the Themes panel, where, like [described earlier](#duplicate-via-the-with-selected-control) you do the checkbox-and-select dance, but this time opting for ‘Update from disk’.

![Select update from disk](https://docs.textpattern.com/img/select-update-from-disk.png)

When selected, another checkbox option is presented: <q><i>Delete unused templates from database on import?</i></q>[^update]

![Select update from disk option](https://docs.textpattern.com/img/select-update-from-disk-option.png)

This second option is asking if you want to make your theme in the themes table (database version) the same as the one in the <i>themes</i> directory (disk version) by deleting files that might be different (in terms of existing) in the database version. This is generally a good idea, to keep versions consistent, and the checkbox is ticked by default. But you might have your reasons for wanting variability, like wanting to customize the sourced theme. Hard to say.

Either way, click the ‘Go’ button and confirm when asked if you are sure.

[^update]: Should read ‘. . . on update?’ to be consistent with the initial selection action.

## Deleting themes

Themes can be deleted from the Themes panel, and the associated version in the <i>themes</i> directory as well, all in one go. Or you can opt to keep the disk version while just deleting the one from the database.

Deletion requires the **With selected** menu again, which you surely know by heart at this point. Check the box by the theme to be deleted from the themes table, then select the ‘Delete’ option from the menu.

![Select delete](https://docs.textpattern.com/img/select-delete.png)

The selection triggers another option to respond to: <q><i>Delete theme templates from disk too</i></q>.[^delete]

![Select delete option](https://docs.textpattern.com/img/select-delete-option.png)

Again, ‘disk’ is referring to the <i>themes</i> directory, and you are confirming to delete the package that may be sitting there. The box for this option is checked by default. Proceed with ‘Go’ if you want to clean house entirely, and confirm ‘OK’ when asked if you are sure.

As a safety precaution, if theme packages on disk contain standard theme directories having non-standard subdirectories (e.g. <i>styles/sass</i>), they will not be deleted by Textpattern; rather, the package container will remain, along with the non-standard subdirectories and files inside them. This shell of a theme and its non-standard elements will need deleted manually since the software does not recognize such elements.

If you choose to delete the theme from the database but not from disk (i.e. unchecking the option box), the theme import control discussed in [Importing themes](#importing-themes) appears again at top of the themes table with the theme package still sitting on disk showing in the selection menu. You can then re-import the theme at any time (but then why delete it to begin with), or use the appearance of the theme in the import control as a reminder that the package in the <i>themes</i> directory also needs deleted.

[^delete]: The check-box option text is not phrased well, and missing punctuation. Should be: ‘And delete theme assets from disk?’