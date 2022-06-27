# Blue Prism Application Modeller Conversion Tool
The Blue Prism Application Modeller Conversion Tool has been designed to allow quick conversion of application modeller elements and attributes. The principal audience for this utility is those wishing to convert objects from Internet Explorer to Chrome, Firefox or Edge Chromium browsers, but it also has the scope to support customized conversions between other application modes.

On 15 June 2022, Microsoft ended the support of Internet Explorer 11: 

For more information, please see https://docs.microsoft.com/en-us/lifecycle/announcements/internet-explorer-11-end-of-support

The current version of the application is [**v1.3.1**]((https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BP_AM_Converter.zip)) (published on 23 August 2021). For details, please see **change log** [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/CHANGELOG.md). It is recommended that existing users of all previous versions to upgrade to this version. 

On 27 June 2022, [a new conversion rules file](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_7.1.0.xml) is added to provide support for Blue Prism versions from ***7.1.0*** or above. It takes advantage of new actions, read actions and conditions introduced in 7.1.0 so that many conversions that were previously not possible can now be achieved. In a testing dataset, around ***77%*** of total errors relating to "'XXX' is not processed due to lack of conversion rules" that were present in earlier versions were completely eliminated. It is recommended to customers who are planning to move to 7.1.0 or higher. V1.3.1 can still be used to support conversion but **BPAMConversionToolPlugin.dll** must be updated to the latest to take advantage of the new function referenced by the conversion rules file.

For all changes made since 25 Feb 2021, see **change log** [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/CHANGELOG.md).

**Please ensure you have Blue Prism set to <u>*English*</u> language before generating "Application Manager Operations.html" file.**

## Disambiguation

While Application Modeller Conversion Tool can be customised to offer IE to Microsoft Edge IE Mode (IE Mode) conversion, a decision was made to create a different utility for IE Mode conversion from the ground up, mainly due to the need in incorporating unique reporting requirements, which is otherwise difficult to fulfil by the Application Modeller Conversion Tool in its current design. A specialised utility ensures users can benefit from the research work done by the Blue Prism team on IE Mode to minimise manual efforts in investigations and remediations.

The following section summaries the usage of different utilities on the DX for migrating away from IE:

- **Application Modeller Conversion Tool**: converting from IE object to modern browser equivalent (i.e., Google Chrome, Firefox or Microsoft Edge Chromium), or other Application Modeller conversions.

- **[IE Mode Conversion Assistant](https://digitalexchange.blueprism.com/)**: converting from IE object to run under Microsoft Edge IE Mode only.

## Important Collateral

For **current limitations** of browser conversions using the tool, please see [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BROWSER.md).

You may wish to watch [this blog video](https://community.blueprism.com/blogs/bruce-liu1/2021/03/31/browser-migration) (published on 31st March 2021) to gain a better understanding on the topic of browser migration using Blue Prism.

**User Guide** can be found in [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Blue%20Prism%20User%20Guide%20-%20Application%20Modeller%20Conversion%20Tool.pdf). It is highly recommended that you read it carefully before commencing work with the tool.

For all changes made since 25 Feb 2021, see **change log** [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/CHANGELOG.md).

## Factors that May Impact the Conversion Outcome

### Browsers Do Behave Differently

Browsers sometimes render the same web page differently. Such differences are especially obvious between Internet Explorer (IE) and what we consider as modern browsers, e.g., Chrome/Edge/Firefox. It is like web developers must come up with different code to work with different browsers, adjustments are often needed to cater for such differences when it comes to conversions. It is not the goal of this tool to attempt work around this limitation. It is one of those external factors that is not within the control of Blue Prism, nor the tool itself.

### Modern Browser Mode Limitations

For **current limitations** of browser conversions using the tool, please see [here](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BROWSER.md).

Gaps exist between Blue Prism IE mode and Chrome/Edge/Firefox browser mode; such gaps can be different from one Blue Prism version to another. For example, version 6.9.0 introduced *Get HTML* as well as *Get Current Value* actions that were not available in versions of Blue Prism before that. This means not every action, read action or condition in IE mode has an equivalent in Chrome/Edge/Firefox mode. The tool has attempted to close such gaps as much as possible but there is limitation in what any tool can do in situations like this. From the perspective of the tool, 6.3.0 - 6.8.x may receive less conversions as compared to 6.9.0 - 6.10.0. Blue Prism 7.1.0 has further closed some major gaps identified between IE and modern browser mode, and as a result, many more automated conversions are now possible as compared to earlier versions.

The way how Blue Prism Attach/Detach work is also different than in IE mode. This may mean developers would need to consider revamping some of the existing code to work with the new versions.

Another important change is the unavailability of Active Accessibility (AA) mode in modern browser modes. This means all objects previously spied in AA for IE, must be replaced by elements spied using another method. UIA is a close match to AA and the tool has indeed offered a conversion rules file to be used in a 2nd pass once the object has been successfully converted into modern browser modes. More information on AA to UIA conversion can be found in AA to UIA sections below.

It is important to know that neither AA nor UIA is the best technology fit for modern web applications that are often complex and dynamic in nature. It would be prudent to consider a different spying technique during the migration process for such web applications so the automation can obtain the best performance possible. For a pure HTML application, modern browser mode is always an option. For versions before 6.9.0, spying using XPath in browser mode is another option to consider. For 6.9.0 onwards, both CSS Selector and XPath are available. To gain further understanding of CSS and XPath technique, please consider [this Blue Prism University course](https://university.blueprism.com/learn/course/16924/Spying%2520Using%2520CSS%2520Selector%2520and%2520Xpath).

It is envisaged that future versions of Blue Prism will continue offer more features to further narrow down known function gaps between IE and Chrome/Edge/Firefox modes. This has happened with 7.1.0 release. More Blue Prism version specific conversion rules files would be offered if necessary when new Blue Prism releases have become available.

### Developer Choices

Developers may have made some unconscious decisions when developing Blue Prism automations. They might not be actively aware of the consequences of those decisions as such code would be migrated to modern browsers. This determines some code will be converted quite easily while other may not be converted at all. Two distinct areas where such choices may impact the quality of the conversions are:

- Choice of element attributes
- Choice of actions in fulfilling a specific set of logic

### Accuracy in Predefined Conversion Rules Files

The quality of the conversion rules files associated with the tool also play an important role. We have been incorporating suggestions made to the rules, as well as implementing various bug fixes from the launch date so the community will receive most up to date rules possible. Version 1.2.0 is a big step up in our response where more fine-grained rule definitions are offered for certain actions or read actions that do not have a 100% match between IE and modern browser modes.

We are more than happy to hear your suggestions on how we can improve the application and the conversion rules files that have been created with the tool. You may use [Blue Prism Digital Exchange Community](https://community.blueprism.com/communities/community-home-prod?CommunityKey=1e516cfe-4d1f-4de9-a9eb-58d15bf38c81) to make us aware of those.

## Conversion Rules Files Catalogue

### [ConversionRules_ChromeEdgeFirefox_ExecutablePathChangeOnly_6.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_ChromeEdgeFirefox_ExecutablePathChangeOnly_6.x.xml)
For conversion of Blue Prism objects created using Chrome/Edge/Firefox mode to Chrome/Edge/Firefox mode, e.g., conversion from Chrome to Edge. This conversion rules file only applies **browser executable path change only**, but the objects must already been in one of the Chrome/Edge/Firefox modes. This works for all v6.x versions.

### [ConversionRules_IE-ChromeEdgeFirefox_6.3.0-6.8.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_6.3.0-6.8.x.xml)
For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file converts Blue Prism objects or release files to format that is compatible with Blue Prism version 6.3.0 to 6.8.x. It is suitable if your target Blue Prism environment is between 6.3.0 and 6.8.x.

### [ConversionRules_IE-ChromeEdgeFirefox_6.9.0-6.10.0.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_6.9.0-6.10.0.xml)
For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file converts Blue Prism objects or release files to format that is compatible with Blue Prism version 6.9.0 to 6.10.0. It is suitable if your target Blue Prism environment is between 6.9.0 and 6.10.0.

### [ConversionRules_IE-ChromeEdgeFirefox_6.10.1-7.0.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_6.10.1-7.0.x.xml)
For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file converts Blue Prism objects or release files to format that is compatible with Blue Prism version 6.10.1 to 7.0.x. It is suitable if your target Blue Prism environment is between 6.10.1 and 7.0.x.

### [ConversionRules_IE-ChromeEdgeFirefox_7.1.0.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_IE-ChromeEdgeFirefox_7.1.0.xml)

For conversion of Blue Prism objects created using Internet Explorer mode to Chrome/Edge/Firefox modes. This conversion rules file converts Blue Prism objects or release files to format that is compatible with Blue Prism version 7.1.0 or above. It is suitable if your target Blue Prism environment is 7.1.0 or above.

### [ConversionRules_AA-UIA_6.3.x.xml](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/Conversion%20Rules%20Files/ConversionRules_AA-UIA_6.3.x.xml)
For conversion of Blue Prism objects from Active Accessibility (AA) to UI Automation (UIA). Such objects or releases must have gone through conversion into Chrome/Edge/Firefox mode from IE. This conversion rules file supports all Blue Prism versions from v6.3.0 onwards where Chrome mode firstly became available. For more details please see below.

#### Notes on Active Accessibility (AA) to UI Automation (UIA) Conversion Usage
The AA to UIA conversion is only possible with an updated version of the plugin *BPAMConversionToolPlugin.dll*. Please kindly obtain a new version of the DLL file from the [zip file](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BP_AM_Converter.zip). Note that no update to the core Blue Prism Application Modeller Conversion Tool application is needed if you are already on v1.1.0 or above. If in doubt, just download the latest [BP_AM_Converter.zip](https://github.com/blue-prism/ApplicationModellerConversion/blob/master/BP_AM_Converter.zip) file again and overwrite your local copy.

This conversion rule file only supports conversion of elements from AA to UIA when the Blue Prism object or objects found in the Blue Prism release file have already been converted to use Chrome/Edge/Firefox, i.e. apptypeinfo > id is either *BrowserAttach* or *BrowserLaunch*. Other AA elements found will not be converted, e.g., those associated with a Win32 application.

To prepare an Internet Explorer (IE) object with AA elements for conversion, i.e. apptypeinfo > id is either *HTMLAttach* or *HTMLLaunch*, please firstly convert the object or release into Chrome/Edge/Firefox. You may use one of the conversion rules files prefixed with "ConversionRules_IE-ChromeEdgeFirefox_" to achieve this, please kindly pick one such file that is most appropriate for your Blue Prism version. Once you have managed to obtain the converted object or release file, you can then use it with this conversion rule file in a 2nd pass to convert those AA elements into UIA.

It is important to note that UIA elements are often slower for large and complex web pages, potentially slower than those spied using AA. If you are considering converting from IE based AA elements to Chrome/Edge/Firefox based UIA elements, you may wish to re-spy such elements in Chrome/Edge/Firefox browser modes using CSS Selector/XPath Selector technique to get the best performance. To gain further understanding of this technique, please consider [this Blue Prism University course](https://university.blueprism.com/learn/course/16924/Spying%2520Using%2520CSS%2520Selector%2520and%2520Xpath).

#### Notes on AA to UIA Conversion Logic
Only three attribute mappings are implemented for AA to UIA conversion, they are:

- Name (AA) to UIA Name (UIA)
- Role (AA) to UIA Control Type (UIA)
- Match Index (AA) to Match Index (UIA)

 The key to success is the conversion between AA Role and UIA Control Type.

The AA Role to UIA Control Type conversion logic is defined by the mappings found in [this](https://docs.microsoft.com/en-us/windows/win32/winauto/uiauto-msaa#:~:text=Microsoft%20Active%20Accessibility%20is%20the,products%20and%20automated%20testing%20tools) article from Microsoft. The mapping is implemented with some exceptions as detailed below:
* AA role **ROLE_SYSTEM_CLIENT** is not mapped to any UIA Control Type. This means AA object with Role set to **Client** is not going to work once converted into UIA. You may wish to manually set the target UIA Control Type or respy the element altogether.
* There exist 3 mappings possible for AA role **ROLE_SYSTEM_LIST**, i.e., **DataGrid**, **Header** and **List**. **List** is currently set as the target UIA Control Type. Changes can be made in the plugin source code to have a different mapping set if needed. Once it is done, you must replace existing **BPAMConversionToolPlugin.dll** with the newly compiled file.
* There exist 2 mappings possible for AA role **ROLE_SYSTEM_LISTITEM**, i.e., **DataItem** and **ListItem**. **ListItem** is currently set as the target UIA Control Type. Changes can be made in the plugin source code to have a different mapping set if needed. Once it is done, you must replace existing **BPAMConversionToolPlugin.dll** with the newly compiled file.

It is observed that AA spied objects will often have attributes associated with their parent level elements selected by default. Although it is possible to define mappings on the parent level attributes, a decision has been made to drop them altogether after having converted to UIA mode as:
* Blue Prism spied UIA objects do not usually utilise any of the parent level attributes. 
* Including parent level attributes in UIA elements will often result in no matching elements found error.
That being said, changes may be made to the conversion rules file to enable such mappings if needed.