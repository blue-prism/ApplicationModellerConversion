# Limitations on Browser Conversions

## No Conversion Rules - Modern browser mode limitations

The following list highlights areas where modern browser modes have yet to deliver equivalent functionalities as the IE mode. **Note that the following list is not exhaustive.** 

| IE Element Type             | Modern Browser Element Type               | IE Action /  ReadAction / Condition                          | Condition                                    | Reason                                                       | v6.3.0 - v6.8.x | v6.9.0 - v6.10.0 | v6.10.1 - v7.0.x | >= v7.1.0 |
| --------------------------- | ----------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------ | :-------------: | :--------------: | :--------: | :--------: |
| HTML Radio Button           | Radio Button (Web)                        | Set Checked                                                  | When *Set Checked* takes in a **false** flag | *Check Radio* in modern browser mode only implements **checked** state. Manual conversion is needed if *Set Checked* value is set to **false**. |        X        |        X         |        X        | X |
| HTML Combo Box              | List (Web)                                | Get Selected Item Text                                       | Any                                          | In modern browser modes, there is a *Get Selected Items Text* but it generates a collection instead of a text data item. Manual conversion is needed. |        X        |        X         |        X        |  |
| HTML Combo Box              | List (Web)                                | Select Item                                                  | If *Item Value* is used                      | *Item Value* not implemented by modern browser mode. Manual conversion is needed. |        X        |        X         |        X        |  |
| HTML Combo Box              | All Web Elements                          | Count Items, Count Selected Items   | Any                                          | Those read actions are not available in Chrome/Edge/Firefox. Manual conversion is needed. |        X        |        X         | X |  |
| All HTML Elements         | All Web Elements                        | Document Loaded                                            | Any                                        | *Document Loaded* not implemented by modern browser mode. Conversion rules files will convert them to *Check Exists* (before 6.9), or *Parent Document Loaded* (6.9 onwards). |        X        |        X       |        X        | X |
| All HTML Elements           | All Web Elements except for Table (Web)   | Get Table                                                    | When target element type is not Table (Web)  | *Get Table* is only implemented by *Table (Web)* element type only. V1.3.0 or above have the ability to change the element type to allow conversion to succeed. |        X        |        X         |        X        | X |
| All HTML Elements           | All Web Elements                         | Get HTML                                                     | Any                                          | Not available before 6.9.0. Manual conversion is needed.     |        X        |                  |            |            |
| All HTML Elements              | All Web Elements                          | Get Current Value                                            | < 6.9.0                                      | *Get Text* does not work for TextArea element type. *Get Current Value* which is able to read from TextArea is only offered from 6.9.0 onwards. |        X        |                  |            |            |
| HTML Element                | Web Element                               | Select Item                                                  | Any                                          | The action is not available in Chrome/Edge/Firefox. V1.3.0 or above have the ability to change the element type to allow conversion to succeed. |        X        |        X         | X | X |
| HTML Edit                   | Text Edit (Web)                           | Check Value                                                  | Any                                          | This condition is not available in Chrome/Edge/Firefox. Manual conversion is needed. |        X        |        X         | X |  |
| All IE non-Application Element | All                                       | Check HTML Attribute (before v7.1.x), or Check Attribute | Any                                          | This condition is not available in Chrome/Edge/Firefox. |        X        |        X         | X |  |
| IE Application Element | All                                       | Check URL, Check URL Domain            | Any                                          | Those conditions are not available in Chrome/Edge/Firefox. |        X        |        X         | X |  |
| IE Application Element      | Chrome, Edge, Firefox Application Element | Navigate, Invoke JavaScript Function, Insert JavaScript Fragment | Any                                          | Those actions are not available in Chrome/Edge/Firefox. Manual conversion is needed. |        X        |        X         | X |  |
| IE Application Element | Chrome, Edge, Firefox Application Element | Update Cookie | Any | This action is not available in Chrome/Edge/Firefox. Manual conversion is needed. For v7.1.0 onwards, there is ability to change the element type to allow conversion to succeed. | X | X | X |               X                |
| IE Application Element      | Chrome, Edge, Firefox Application Element | Get Document URL, Get Document URL Domain, HTML Snapshot | Any                                          | Those read actions are not available in Chrome/Edge/Firefox. Manual conversion is needed. |        X        |        X         | X |  |
| IE Application Element      | Chrome, Edge, Firefox Application Element | Wait Condition: Check URL, Check URL Domain | Any                                          | Those conditions are not available in Chrome/Edge/Firefox. Manual conversion is needed. |        X        |        X         | X |  |
| IE Application Element | Chrome, Edge, Firefox Application Element | Document Loaded | Any | This condition is not available in Chrome/Edge/Firefox. Manual conversion is needed. | X | X | X | X |

## Current Tool Limitation

| Description                                                  | Reason                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Win32 spied elements, including but not limited to browser window, browser popup, Save As.. window and etc. | Conversion rules only cover IE based objects spied in HTML. It is not clear either if there exists mapping between different browser windows spied using Win32 mode. |
| Add new actions to an existing stage as part of the conversion, especially if such action refers to a different element. | The tool only works with one element at a time for any given action, i.e., the current element. No capability is present to add non-existent actions. |
| Change stage output data item type to work with the new action. | It is difficult to work out if it will be entirely possible to change the output data item type without affecting other usage of the same data item. |

## Consider Below Conversion Rules Implemented

| Rules                                                        | Remarks                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| All *Path* attribute having multiple HTML tags found within it will be truncated to start from the last **/HTML**. This is to work with the way how Chrome/Edge/Firefox renders the *Web Path* element for embedded iframe. | This is modelled around the behaviour observed. But doing so does not guarantee that the element will be spyable post conversion. |
| **6.3.0 - 6.8.x:** *Document Loaded [DocumentLoaded] > Check Exists [CheckExists]* | This is not an exact match but these Blue Prism versions only have one condition available, i.e. CheckExists. |
| **6.3.0 - 6.8.x:** *Parent Document Loaded [HTMLCheckExistsAndDocumentLoaded] > Check Exists [CheckExists]* | This is not an exact match but these Blue Prism versions only have one condition available, i.e. CheckExists. |
| **6.9.x - 6.10.0:** *Document Loaded [DocumentLoaded] > Parent Document Loaded [WebCheckParentDocumentLoaded]* | This is a close match and should largely do the job.         |
| **>= 6.10.1:** *Document Loaded [DocumentLoaded] > Parent Document Loaded [WebCheckParentDocumentLoaded]* | This is a close match and should largely do the job.         |

## Partial Rules Implemented by Function (v1.2.0 or above)

- *SetText* function. 
  - 6.9.x conversion rules file maps TextArea element type to use *Get Current Value [WebGetValue]*.
  - Conversion rules file before 6.9 does not perform any conversion if element type is TextArea.
  - Everything else converts to *Get Text [WebGetText]*, including all 6.3.0 - 6.8.x cases.
- *SelectItem* function. 
  - Conversion drops value of argument *Item Position* if *Item Text* exists.
  - Conversion drops *Item Value* due to lack of equivalent in modern browser mode.
- *SelectItemV71* function, for v7.1.0 onwards only.
  - Conversion drops value of argument *Item Position* if *Item Text* exists
  - Conversion sets the *Item Value* in this function.
- *SetAttribute* function. 
  - If *value* of Attribute is set, it is converted to use action *Get Text [WebGetText]*.
  - Everything else converts to *Get Attribute [WebGetAttribute]*.
- *SetRadioChecked* function, for HTML Radio Button.
  - If *Set Checked [SetChecked]* has a value of True or "True" (case insensitive) , it is converted to *Check Radio [WebCheckRadio]*.
  - Otherwise no conversion is performed.
- *SetChecked* function, for HTML Check Box.
  - If *Set Checked [SetChecked]* has a value of True or "True" (case insensitive), it is converted to *Set Attribute [WebSetAttribute]*, with attribute value set to "checked".
  - Otherwise no conversion is performed.