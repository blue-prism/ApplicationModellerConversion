# Change Log

All notable changes to this project will be documented in this file, from 25 Feb 2021

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