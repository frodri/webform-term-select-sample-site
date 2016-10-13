Webform Term Select
===================

A Drupal 7 module that creates pre-built option lists for flagged taxonomy terms.

## Module Requirements
* Taxonomy module (included in core)
* The [Webform](https://www.drupal.org/project/webform) contrib module

## Installation Instructions

* Download the latest release from the Releases section.
* Unzip its contents on the *sites/all/modules* (or *sites/\<your site\>/modules*) directory.
* Activate the module in the Modules admin page.

## Usage Instructions

* By default, Webform Term Select will only expose flagged taxonomy terms to the Webform module. To flag these terms, head to the term list for your vocabulary (*admin/structure/taxonomy/\<your vocabulary machine name\>*) and edit each term you want to expose. Checking the ***Show this term on the Webform pre-built lists*** checkbox and saving will successfully expose a term to Webform.
* Once the terms you want displayed on Webform have been flagged, go to a webform node, and create (or edit) a ***Select options*** component. Its' options should now have an expanded ***Load a pre-built option list*** containing a *Flagged terms* entry for each vocabulary on your site. Select the flagged terms you want to display, verify the options on the ***Options*** text area, and save the changes by clicking the on the ***Save component*** button. Your select option components are now ready to use.
