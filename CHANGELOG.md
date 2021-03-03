# Change Log

All notable changes to this project will be documented in this file, from 25 Feb 2021

[TOC]

## Changes made on 04 Mar 2021

### Core Application

The application has been updated to v1.2.0 to support the following feature:

- Allow .NET functions hosted in the plugin to enable more fine-grained and sophisticated action/readaction rule definition, which complements existing XML based rule conversion files. 
  - Function in Action/ReadAction overrides ArgumentIds XML block.
  - Function is executed before toActionArguments/toReadActionArguments block.
  - For function usage, see Plugin section below.

Please update all IE to Chrome/Edge/Firefox conversion rules files as well as the plugin DLL *BPAMConversionToolPlugin.dll* if upgrading.

#### Bug Fixes

- Element Parameters were not considered by Wait Stage conditions, e.g., when using dynamic attributes in conditions.

### Conversion Rules Files

The following changes have been made to conversion rules file rom IE to Chrome/Edge/Firefox for 6.3.0-6.8.x as well as 6.9.x or above. The rules have been customised to offer a more precise conversion solution for various Blue Prism versions.

- Replaced all instances of *Get HTML Identifier [GetHTMLIdentifier]* action to *Get Attribute [WebGetAttribute]* action with Function *SetAttribute*, for all IE based elements, for all Blue Prism versions.
- Replaced all instances of *Get Current Value [ReadCurrentValue]* action to *Get Text [WebGetText]* action with Function *SetText*, for all IE based elements, for all Blue Prism versions. 
- Created a new rule, partially mapped *HTML Check Box [HTMLCheckBox] > Set Checked [SetChecked]* to *Check Box (Web) [WebCheckBox] > Set Attribute [WebSetAttribute]* using Function *SetChecked*, for all Blue Prism versions.
- Created a new rule, partially mapped *HTML Combo Box [HTMLComboBox] > Set Checked [SetChecked]* to *Radio Button (Web) [WebRadioButton] > Set Checked [WebCheckRadio]* using Function *SetRadioChecked*, for all Blue Prism versions.
- Created a new rule, mapped *HTML Combo Box [HTMLCombo] > Select Item [HTMLSelectItem]* action to *List (Web) [WebList] > Select List Item [SelectItem]* using Function *SelectItem*, for all Blue Prism versions. 
- Created a new rule, mapped *Parent Document Loaded [HTMLCheckExistsAndDocumentLoaded]* condition to *Check Exists [CheckExists]* condition, for all IE based elements, for conversion rules files applicable for Blue Prism versions 6.3.0 - 6.8.x.
- Not implemented but possible to achieve using new Function feature:
  - Ability to append "--force-renderer-accessibility" to a Navigate stage implementing *Launch* action.
  - Ability to replace for example "iexplore" with "chrome"  in an Navigate stage implementing *Attach* action.
- Added *trackingid* to all conditions. This is optional but good for consistency.
- Added new rule to map Document Loaded [DocumentLoaded] condition so that:
  - For 6.9.0 or above, mapped to *Parent Document Loaded [WebCheckParentDocumentLoaded]*. Note that this is **not an exact match** but should largely do the job.
  - For 6.3.0 - 6.8.x, mapped to *Check Exists [CheckExists]*. Note that this is **not an exact match** but *Check Exists* is the only option available for those Blue Prism versions.

#### Bug Fixes

- *HTML Combo Box [HTMLCombo] > Get All Items [HTMLGetAllItems]* had a typo so it was set to [GetAllItems] instead. This has caused rules fail to match intended action id. 

### Plugin

New function to support sophisticated conversion on element action and readaction. Such function takes three arguments:

- arg[0]: existing action/readaction id; the function must return an action/readaction id to succeed. Depending on the use case, existing id may be returned to signal conversion is not possible; alternatively return a new id to map the action to something different.
- arg[1]: a dictionary of arguments associated with action/reaction to be converted from. The goal is to rewrite the dictionary object so it only contains matching arguments for the action/readaction id set. 
  - Unneeded arguments should be removed, new arguments should be added in the process. However, toActionArguments or toReadActionArguments found in the conversion rules file can be used instead to achieve the same purpose.

- arg[2]: a dictionary of element attributes already converted. 
  - Separate rules may be written to have different behaviour set for different element type. 
  - For IE to Chrome/Edge/Firefox conversion, use *wCssSelector* attribute, which is only available from v6.9 onwards may be used to distinguish Blue Prism versions. This allows customised rules to be defined for different target Blue Prism versions.

The following functions have been added to the plugin. For details, please see the plugin source code:

- Added *SetText* function. 
  - 6.9.x conversion rules file maps TextArea element type to use *Get Current Value [WebGetValue]*.
  - Conversion rules file before 6.9 does not perform any conversion if element type is TextArea.
  - Everything else converts to *Get Text [WebGetText]*, including all 6.3.0 - 6.8.x cases.
- Added *SelectItem* function. 
  - Conversion drops *Item Position* if *Item Text* exists.
- Added *SetAttribute* function. 
  - Attribute of "value" is converted to use *Get Text [WebGetText]*.
  - Everything else converts to *Get Attribute [WebGetAttribute]*.
- Added *SetRadioChecked* function, for HTML Radio Button.
  - If *Set Checked [SetChecked]* has a value of True, it is converted to *Check Radio [WebCheckRadio]*.
  - Otherwise no conversion is performed.
- Added *SetChecked* function, for HTML Check Box.
  - If *Set Checked [SetChecked]* has a value of True, it is converted to *Set Attribute [WebSetAttribute]*, with attribute value set to "checked".
  - Otherwise no conversion is performed.
- Added *AttachProcessNameChange* function, which is not implemented by default conversion rules files.

The following functions have been modified:

- *SetPath* now removes iframe specific sections.

## Changes made on 25 Feb 2021

### Core Application

- No change made

### Conversion Rules Files

The following changes have been made to conversion rules file rom IE to Chrome/Edge/Firefox for 6.3.0-6.8.x as well as 6.9.x or above.

It is strongly recommended that you update your local conversion rules files.

- *Value* attribute in IE is now mapped to *Web Text* in Chrome/Edge/Firefox. *Value* was previously mapped to *Web Value* which is not appropriate. This change should have a positive effect on many elements that are dependent on *Web Text* to work. As a result of this change, *Value (wValue)* in Chrome/Edge/Firefox no longer has a mapping from IE.
- Added a new conversion rule to allow conversion from *HTMLCombox > HTMLSelectItem* to *WebList > WebSelectItem*. But please note:
    - In IE *Item Text* takes precedence over *Item Position* while in Chrome/Edge/Firefox *Item Index* takes precedence over *Item Text*. 
    - *Item Value* in IE is no longer an option in Chrome/Edge/Firefox therefore it is dropped as part of the conversion.

### Plugin

- No change made

  

  