# ProcessWire newsletter subscription

Allow users to subscribe and unsubscribe to a newsletter.

## Subscription flow

The user enters their email address and some other information (if you want to).
As soon as the form is submitted a mail is sent to the given address containing a confirmation link (double opt-in).
Now the user is added with status hidden and role newsletter.
After visiting the confirmation link the user is subscribed (changed status to active).

## Unsubscription flow

There are two methods to unsubscribe: by using a web form or by providing a link in the newsletter.

### A) using the form

The user enters only their email address and selects the unsubscribe option (if you provide additional fields hide them via JavaScript).
If the user submitted a valid and existing email address, a email is sent to this address containing a confirmation link to end subscription.
After visiting the link the user is unsubscribed (the user will be deleted).

### B) link inside newsletter

The user is unsubscribed after visiting the link immediately.
You have to generate the link parameters depending on email and userAuthSalt.

## Usage

* install the module
* create fields which you want to add to the newsletter subscription form
  * if the field should be required, add this in the field settings
  * field email is mandatory and already preselected
* assign these fields to the user template 
  * [How to assign fields to the user template](https://processwire.com/talk/topic/1156-custom-user-fields/?p=10161)
* fill module settings
  * select fields which should be added to the form - save module config
  * select fields which should be used as username
  * if you want to provide an unsubscribe option, activate the according checkbox
  * enter email from address
  * enter how many days the confirmation links should be valid (default 5 days)
  * define email text messages
* call module
  
  ```php
  echo $modules->get('NewsletterSubscription')->render();
  ```
* if you want to add own classes and/or markup, you can pass an option array as a parameter
* to get an overview of what's possible, have a look at [How to overwrite classes and markup](https://github.com/justonestep/processwire-newslettersubscription/tree/develop#how-to-overwrite-classes-and-markup)

  ```php
  $options = array (
    'markup' => array(
      'InputfieldSelect' => array(
        'item' : "{out}"
      )
    ),
    'classes' => array(
      'form' => 'form  form__super-special-class',
      'InputfieldRadios' => array(
        'item' => 'form__item--options'
      )
    )
  );

  echo $modules->get('NewsletterSubscription')->render($options);
  ```

## Notify admin via email

If you want to notify any person e.g. administator via email when an user has subscribed/unsubscribed,
check the according checkbox in module settings.
As soon as you activate this field, some more fields will appear.
In these fields you can specify further email messages as well as email receiver and sender.
Furthermore you can use placeholders like `%email%` - even for the receiver email addresses.
After the user has visited the confirmation link the admin gets notified.

## How to translate

All phrases like email subjects are translatable. 
Add the module file to the language translator and start translating.
Phrases which don't exist in this file belong to the ProcessWire core.
For example the messages *Please enter a valid e-mail address* or *Missing required value*.

Relevant Files:

- site/modules/NewsletterSubscription.module
- wire/modules/Inputfield/InputfieldEmail.module
- wire/core/InputfieldWrapper.php

Depending on the fields you added to the form there might be some other files.

## How to overwrite classes and markup

* below is the list of all available customization options
  copied from [ProcessWire master 2.7](https://github.com/ryancramerdesign/ProcessWire/blob/master/wire/core/InputfieldWrapper.php) 

```php
$defaultMarkup = array(
  'list' => "\n<ul {attrs}>\n{out}\n</ul>\n",
  'item' => "\n\t<li {attrs}>\n{out}\n\t</li>", 
  'item_label' => "\n\t\t<label class='InputfieldHeader ui-widget-header{class}' for='{for}'>{out}</label>",
  'item_label_hidden' => "\n\t\t<label class='InputfieldHeader InputfieldHeaderHidden ui-widget-header{class}'><span>{out}</span></label>",
  'item_content' => "\n\t\t<div class='InputfieldContent ui-widget-content{class}'>\n{out}\n\t\t</div>", 
  'item_error' => "\n<p class='InputfieldError ui-state-error'><i class='fa fa-fw fa-flash'></i><span>{out}</span></p>",
  'item_description' => "\n<p class='description'>{out}</p>", 
  'item_head' => "\n<h2>{out}</h2>", 
  'item_notes' => "\n<p class='notes'>{out}</p>",
  'item_icon' => "<i class='fa fa-{name}'></i> ",
  'item_toggle' => "<i class='toggle-icon fa fa-angle-down' data-to='fa-angle-down fa-angle-right'></i>", 
  // ALSO: 
  // InputfieldAnything => array( any of the properties above to override on a per-Inputifeld basis)
);

$defaultClasses = array(
  'form' => '', // additional clases for InputfieldForm (optional)
  'list' => 'Inputfields',
  'list_clearfix' => 'ui-helper-clearfix', 
  'item' => 'Inputfield {class} Inputfield_{name} ui-widget',
  'item_label' => '', // additional classes for InputfieldHeader (optional)
  'item_content' => '',  // additional classes for InputfieldContent (optional)
  'item_required' => 'InputfieldStateRequired', // class is for Inputfield
  'item_error' => 'ui-state-error InputfieldStateError', // note: not the same as markup[item_error], class is for Inputfield
  'item_collapsed' => 'InputfieldStateCollapsed',
  'item_column_width' => 'InputfieldColumnWidth',
  'item_column_width_first' => 'InputfieldColumnWidthFirst',
  'item_show_if' => 'InputfieldStateShowIf',
  'item_required_if' => 'InputfieldStateRequiredIf'
  // ALSO: 
  // InputfieldAnything => array( any of the properties above to override on a per-Inputifeld basis)
);
```
