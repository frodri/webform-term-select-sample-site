Webform Term Select
===================

A Drupal 7 module that creates pre-built option lists for flagged taxonomy terms.

## Module Requirements
* Taxonomy module (included in core)
* The [Webform](https://www.drupal.org/project/webform) contrib module

## Installation Instructions

* Download the latest release from the [Releases](https://github.com/frodri/webform-term-select/releases) section.
* Unzip its contents on the *sites/all/modules* (or *sites/\<your site\>/modules*) directory.
* Activate the module in the Modules admin page.

## Usage Instructions

* By default, Webform Term Select will only expose flagged taxonomy terms to the Webform module. To flag these terms, head to the term list for your vocabulary (*admin/structure/taxonomy/\<your vocabulary machine name\>*) and edit each term you want to expose. Checking the ***Show this term on the Webform pre-built lists*** checkbox and saving will successfully expose a term to Webform.
* Once the terms you want displayed on Webform have been flagged, go to a webform node, and create (or edit) a ***Select options*** component. Its' options should now have an expanded ***Load a pre-built option list*** containing a *Flagged terms* entry for each vocabulary on your site. Select the flagged terms you want to display, verify the options on the ***Options*** text area, and save the changes by clicking the on the ***Save component*** button. Your select options component is now ready to use.

## Considerations During Development

### Visibility flag storage

We considered two options for storing term visibility: the use of the Flag API to mark term entities with a custom, module-defined flag, and the addition of a new field on the built-in Taxonomy schema. The latter won out, due to its relative ease of access, along with its complete lack of module dependencies.

### Visibility indicators in the admin interface 

During the early stages of development for this module, the only way to find out whether a vocabulary term was flagged for use in Webform was to actually create a Form component that used that vocabulary. To avoid this drawn-out procedure, we modified the term links on each vocabulary page to have them display a small "Shown in Webforms" subtitle whenever their respective fields have been exposed. 

### Behavior for hierarchical terms

Consider a vocabulary that contains two terms on the root, and two children terms attached to the second node. If we were to flag them all and display them on the select option component as is, the webform user would have no way of determining if a term belongs in any sort of hierarchy.

        <div class="form-type-radio">
            <input type="radio" id="term-id-1">Fruit</input>
            <input type="radio" id="term-id-2">Veggies</input>
            <input type="radio" id="term-id-3">Lettuce</input>
            <input type="radio" id="term-id-4">Onions</input>
        </div>

This can be remedied somewhat by adding formating to the term labels depending on hierarchy, but without a way to enforce flagging parentage it is possible to display an incorrect hierarchy.

        <div class="form-type-radio">
            <input type="radio" id="term-id-1">-Fruit</input>
            <input type="radio" id="term-id-3">---Lettuce</input>
            <input type="radio" id="term-id-4">---Onions</input>
        </div>

The implementation of a system that can enforce this behavior is not trivial: it would require both a way to prevent parent terms from being unflagged if they have flagged children, and a way to flag the entire parentage of a child term whenever it is flagged. Due to time constraints and concerns regarding performance, we went with the simple route and treated hierarchical terms in the same manner as "flat" ones. 

## Possible Future Improvements

* Option for hiding vocabularies entirely from Webform. Would require a new flag in the vocabulary table, along with the respective form edits needed to edit that flag. Might also need to consider a means of hiding vocabularies from the pre-built options list when they have no marked terms. This is mostly a QoL improvement, though - not having it does not impair the module's functionality.
* Batch visibility flagging from the term overview page. Modifying the term overview with some checkboxes and an additional submit button is certainly possible - but overriding the theming function for the overview table would add a fair amount of complexity to the module. Is this something better left as a plugin for something like Views Bulk Operations?
* Query optimization. Our pre-built option lists currently use Drupal's built-in taxonomy_get_tree method to get their data. This could be optimized through the use of a custom database query.