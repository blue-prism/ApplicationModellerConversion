# Change Log

All notable changes to this project will be documented in this file, from 25 Feb 2021.

## Changes made on 27 Jun 2022

### Core Application

- No change made.

### Conversion Rules Files

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.10.1 or above:

- File is now renamed from ***ConversionRules_IE-ChromeEdgeFirefox_6.10.1.xml*** to ***ConversionRules_IE-ChromeEdgeFirefox_6.10.1-7.0.x.xml***, as it is no longer compatible with Blue Prism 7.1.x or above.
- BPVersion attribute found within the file is also updated to reflect the change.
- Removed 'HTMLGetSelectedItemsText' to 'WebGetSelectedItemsText' action conversion as it is not valid, nor in use.

A new conversion rules file is added to cover conversion from IE to Chrome/Edge/Firefox for 7.1.0 or above:

- ***ConversionRules_IE-ChromeEdgeFirefox_7.1.x.xml***. Please update the plugin **BPAMConversionToolPlugin.dll** to the latest to take advantage of the conversion rules file.

### Plugin

- A new function 'SelectItemV71' is added on to support correct conversion of 'Select Item' action that has been updated in 7.1.0. It has the ability to support *Item Value* argument. Function 'SelectItem' is still retained to support conversion rules files created for versions of Blue Prism before 7.1.0.

## Changes made on 23 Aug 2021

### Core Application

The application has been updated to v1.3.1 to support the following feature:

- Allow access to AppTypeInfoId value post conversion to action and read action conversion functions. This allows the value of 'Is Connected' read action to set to 'IsConnected' when attaching and 'WebIsConnected' when launching.

### Conversion Rules Files

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.10.1 or above:

- A function SetIsConnected is now set on to support correct conversion of 'Is Connected' read action.
- Removed all toActionArguments from all conditions; those arguments did not have any effect on conditions.

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.9.0-6.10.0:

- Removed all toActionArguments from all conditions; those arguments did not have any effect on conditions. 

### Plugin

- AppTypeInfoId value post conversion is added to all action and read action conversion functions.
- A function 'SetIsConnected' is added on to support correct conversion of 'Is Connected' read action. 
- 'SetRadioChecked' and 'SetChecked' functions now consider Flag value with double quotes around the value *true*.

## Changes made on 14 Apr 2021

### Core Application

- No change made.

### Conversion Rules Files

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.10.1 or above:

- Action 'Terminate' is now mapped to 'WebCloseApplication' for Application element; trackingid is added to the Action.

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.9.0-6.10.0:

- Action 'Terminate' is now mapped to 'WebCloseApplication' for Application element; trackingid is added to the Action.

### Plugin

- No change made.

## Changes made on 07 Apr 2021

### Core Application

- No change made.

### Conversion Rules Files

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.9.x:

- Read Action 'IsConnected' is now mapped to 'WebIsConnected' for Application element; trackingid is added to the Read Action.
- File is now renamed from ***ConversionRules_IE-ChromeEdgeFirefox_6.9.x.xml*** to ***ConversionRules_IE-ChromeEdgeFirefox_6.9.0-6.10.0.xml***, as it is no longer compatible with Blue Prism v6.10.1.
- BPVersion attribute found within the file is also updated to reflect the change.

A new conversion rules file is added to cover conversion from IE to Chrome/Edge/Firefox for 6.10.1 or above:

- ***ConversionRules_IE-ChromeEdgeFirefox_6.10.1.xml***

### Plugin

- No change made.

## Changes made on 09 Mar 2021

### Core Application

The application has been updated to v1.3.0 to support the following feature:

- Predict the most appropriate element type to use before conversion, to ensure the best match of existing actions, read actions as well as conditions post conversion. This works very well for actions or read actions that are no longer available in mapped element type as defined in the conversion rules files, but may exist in other element types. *Get Table [HTMLGetTable]* is one such example, as it is available in nearly all IE elements but only present in *Table (Web) [WebTable]* element. As a result of this change, the following conversion rules mismatches can be minimised if not completely eliminated:
  - 'HTMLGetTable' is NOT processed due to lack of conversion rules
  - 'HTMLSelectItem' is NOT processed due to lack of conversion rules
  - 'SelectItem' is NOT processed due to lack of conversion rules
- All IE to Chrome/Edge/Firefox Conversion rules files, as well as plugin DLL *BPAMConversionToolPlugin.dll* must be updated to fully take advantage of this feature.
- The overall duration of the conversion process may take slightly longer as compared to previous versions, but it is expected to save a lot more time on devising manual workarounds post conversion.

### Conversion Rules Files

The following changes have been made to the conversion rules files from IE to Chrome/Edge/Firefox for 6.3.0-6.8.x, as well as 6.9.x or above.

- Added conversion rules to convert from *Focus [HTMLFocus], Invoke Javascript Function [HTMLInvokeJavascriptMethod], Insert Javascript Fragment [HTMLInsertJavascriptFragment]* and *Navigate [HTMLNavigate]* to all conversion rules files. Note that adding those rules will have no effect on direct conversion since they are largely not present in various IE element types. They are mainly used to provide necessary information to help the processing engine to select the best element type possible prior to conversion.
- Added conversion rules to convert from *Select Item [SelectItem]* > *Web Select Item [WebSelectListItem]* for *HTML Combo Box [HTMLCombo]*. Note that adding those rules will have no effect on direct conversion since they are largely not present in various IE element types. They are mainly used to provide necessary information to help the processing engine to select the best element type possible prior to conversion.

### Plugin

- Changed SelectItem function so it can work with conversion *Select Item [SelectItem]* to *Web Select Item [WebSelectListItem]* for *HTML Combo Box [HTMLCombo]*.

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

The following changes have been made to conversion rules files from IE to Chrome/Edge/Firefox for 6.3.0-6.8.x as well as 6.9.x or above. The rules have been customised to offer a more precise conversion solution for various Blue Prism versions.

- Replaced all instances of *Get HTML Identifier [GetHTMLIdentifier]* action to *Get Attribute [WebGetAttribute]* action with Function *SetAttribute*, for all IE based elements, for all Blue Prism versions.
- Replaced all instances of *Get Current Value [ReadCurrentValue]* action to *Get Text [WebGetText]* action with Function *SetText*, for all IE based elements, for all Blue Prism versions. 
- Created a new rule, partially mapped *HTML Check Box [HTMLCheckBox] > Set Checked [SetChecked]* to *Check Box (Web) [WebCheckBox] > Set Attribute [WebSetAttribute]* using Function *SetChecked*, for all Blue Prism versions.
- Created a new rule, partially mapped *HTML Radio Button [HTMLRadioButton] > Set Checked [SetChecked]* to *Radio Button (Web) [WebRadioButton] > Set Checked [WebCheckRadio]* using Function *SetRadioChecked*, for all Blue Prism versions.
- Created a new rule, mapped *HTML Combo Box [HTMLCombo] > Select Item [HTMLSelectItem]* action to *List (Web) [WebList] > Select List Item [SelectItem]* using Function *SelectItem*, for all Blue Prism versions. 
- Created a new rule, mapped *Parent Document Loaded [HTMLCheckExistsAndDocumentLoaded]* condition to *Check Exists [CheckExists]* condition, for all IE based elements, for conversion rules files applicable for Blue Prism versions 6.3.0 - 6.8.x.
- Not implemented but possible to achieve using new Function feature:
  - Ability to append "--force-renderer-accessibility" to a Navigate stage implementing *Launch* action.
  - Ability to replace for example "iexplore" with "chrome" in a Navigate stage implementing *Attach* action.
- Added *trackingid* to all conditions for 6.9.x. This is optional but good for consistency.
- Added new rule to map Document Loaded [DocumentLoaded] condition so that:
  - For 6.9.0 or above, mapped to *Parent Document Loaded [WebCheckParentDocumentLoaded]*. Note that this is **not an exact match** but should largely do the job.
  - For 6.3.0 - 6.8.x, mapped to *Check Exists [CheckExists]*. Note that this is **not an exact match** but *Check Exists* is the only option available for those Blue Prism versions.

#### Bug Fixes

- *HTML Combo Box [HTMLCombo] > Get All Items [HTMLGetAllItems]* had a typo so it was mapped to *[GetAllItems]* instead. This has caused conversion from *HTML Combo box > Get All Items* not to match any Web Element action. 

### Plugins

New function to support sophisticated conversion on element action and readaction. Such function takes three arguments:

- arg[0]: existing action/readaction id; the function must return an action/readaction id to succeed. Depending on the use case, existing id may be returned to signal conversion is not possible; alternatively return a new id to map the action to something different.
- arg[1]: a dictionary of arguments associated with the action/reaction to be converted from. The goal is to rewrite the dictionary object so it only contains matching arguments for the to-be action/readaction id. 
  - Unneeded arguments should be removed, new arguments should be added in the process. However, toActionArguments or toReadActionArguments found in the conversion rules file can be used instead to achieve the same purpose.

- arg[2]: a dictionary of element attributes already converted. 
  - Separate rules may be written to have different behaviours set for different element types. 
  - For IE to Chrome/Edge/Firefox conversion, may use *wCssSelector* attribute to tell the Blue Prism version to convert to. This allows customised rules to be defined for different target Blue Prism versions.

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
- Added *AttachProcessNameChange* function, which can be used to modify browser executable name found in Attach stage; this is not implemented by the default conversion rules files.

The following functions have been modified:

- *SetPath* now removes iframe specific sections.

## Changes made on 25 Feb 2021

### Core Application

- No change made.

### Conversion Rules Files

The following changes have been made to conversion rules file from IE to Chrome/Edge/Firefox for 6.3.0-6.8.x, as well as 6.9.x or above.

It is strongly recommended that you update your local conversion rules files.

- *Value* attribute in IE is now mapped to *Web Text* in Chrome/Edge/Firefox. *Value* was previously mapped to *Web Value* which is not appropriate. This change should have a positive effect on many elements that are dependent on *Web Text* to work. As a result of this change, *Value (wValue)* in Chrome/Edge/Firefox no longer has a mapping from IE.
- Added a new conversion rule to allow conversion from *HTMLCombox > HTMLSelectItem* to *WebList > WebSelectItem*. But please note:
    - In IE *Item Text* takes precedence over *Item Position* while in Chrome/Edge/Firefox *Item Index* takes precedence over *Item Text*. 
    - *Item Value* in IE is no longer an option in Chrome/Edge/Firefox therefore it is dropped as part of the conversion.

### Plugin

- No change made.
