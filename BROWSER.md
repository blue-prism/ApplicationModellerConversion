# Limitations on Browser Conversions

## No Conversion Rules - Blue Prism Function Gaps

**Note that this list is not exhaustive.** 

| IE Element Type             | Modern Browser Element Type               | IE Action /  ReadAction / Condition                          | Condition                                    | Reason                                                       | v6.3.0 - v6.8.x | >= v6.9.0 |
| --------------------------- | ----------------------------------------- | ------------------------------------------------------------ | -------------------------------------------- | ------------------------------------------------------------ | :-------------: | :-------: |
| HTML Radio Button           | Radio Button (Web)                        | Set Checked                                                  | When *Set Checked* takes in a **false** flag | *Check Radio* in modern browser mode only implements **checked** state. |        X        |     X     |
| HTML Element                | Web Element                               | Document Loaded                                              | Any                                          | *Document Loaded* not implemented by modern browser mode. Conversion rules files will convert them to *Check Exists* (before 6.9), or *Parent Document Loaded* (6.9 onwards). |        X        |     X     |
| HTML Combo Box              | List (Web)                                | Select Item                                                  | If *Item Value* is used                      | *Item Value* not implemented by modern browser mode.         |        X        |     X     |
| All                         | All except for Table (Web)                | Get Table                                                    | When target element type is not Table (Web)  | *Get Table* is only implemented by *Table (Web)* element type only. |        X        |     X     |
| All                         | All                                       | Get HTML                                                     | Any                                          | Not available before 6.9.0                                   |        X        |           |
| All                         | All                                       | Get Current Value                                            | < 6.9.0                                      | *Get Text* does not work for TextArea element type. *Get Current Value* which works is only offered from 6.9.0 onwards. |        X        |           |
| HTML Combo Box              | All                                       | Count Items, Count Selected Items, Get Selected Items Text   | Any                                          | Those read actions are not available in Chrome/Edge/Firefox. |        X        |     X     |
| HTML                        | Web Element                               | Select Item                                                  | Any                                          | The action is not available in Chrome/Edge/Firefox.          |        X        |     X     |
| All non-Application Element | All                                       | Check URL, Check URL Domain, Check HTML Attribute            | Any                                          | Those conditions are not available in Chrome/Edge/Firefox.   |        X        |     X     |
| Application Element         | Chrome, Edge, Firefox Application Element | Action: Navigate, Invoke JavaScript Function, Insert JavaScript Fragment, Update Cookie | Any                                          | Those actions are not available in Chrome/Edge/Firefox.      |        X        |     X     |
| Application Element         | Chrome, Edge, Firefox Application Object  | Read Action: Get Conflicting Font Characters, Get Document URL, Get Document URL Domain, HTML Snapshot | Any                                          | Those read actions are not available in Chrome/Edge/Firefox. |        X        |     X     |
| Application Element         | Chrome, Edge, Firefox Application Object  | Wait Condition: Check URL, Check URL Domain, Document Loaded | Any                                          | Those conditions are not available in Chrome/Edge/Firefox.   |        X        |     X     |
| HTML Edit                   | Text Edit (Web)                           | Check Value                                                  | Any                                          | This condition is not available in Chrome/Edge/Firefox.      |        X        |     X     |

## Current Tool Limitation

| Description                                                  | Reason                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Win32 spied elements, including but not limited to browser window, browser popup, Save As.. window and etc. | Conversion rules only cover IE based objects spied in HTML. It is not clear either if there exists mapping between different browser windows spied using Win32 mode. |
| Add new actions to an existing stage as part of the conversion, especially if such action refers to a different element. | The tool only works with one element at a time (current element). No capability is present to add non-existent actions. |
| Alter element type based on action set, as some actions may be present in other element types but not the one that is currently mapped to. | The application currently only supports one-to-one mapping between a source element type with the destination element type. |

## Consider Below Conversion Rules Implemented

| Rules                                                        | Remark                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| All *Path* attribute value having multiple HTML tags will be truncated to use the bit from the last **/HTML **text. This is to work with the way how Chrome renders the *Web Path* element for embedded iframe. | This is modelled around the behaviour observed. But doing so does not guarantee that the element will be spyable. |
| **6.3.0 - 6.8.x:** *Document Loaded [DocumentLoaded] > Check Exists [CheckExists]* | This is not an exact match but these Blue Prism versions only have one condition available, i.e. CheckExists. |
| **6.3.0 - 6.8.x:** *Parent Document Loaded [HTMLCheckExistsAndDocumentLoaded] > Check Exists [CheckExists]* | This is not an exact match but these Blue Prism versions only have one condition available, i.e. CheckExists. |
| **6.9.x:** *Document Loaded [DocumentLoaded] > Parent Document Loaded [WebCheckParentDocumentLoaded]* | This is a close match and should largely do the job.         |

## Partial Rules Implemented by Function

- *SetText* function. 
  - 6.9.x conversion rules file maps TextArea element type to use *Get Current Value [WebGetValue]*.
  - Conversion rules file before 6.9 does not perform any conversion if element type is TextArea.
  - Everything else converts to *Get Text [WebGetText]*, including all 6.3.0 - 6.8.x cases.
- *SelectItem* function. 
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