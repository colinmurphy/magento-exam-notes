## Which ways exist to specify the layout update handles that will be processed during a request?

    <reference name="root">
      <update handle="removeFooter" />
    </reference>

    <removeFooter>
      <remove name="footer" />
    </removeFooter>


## Which classes are responsible for the layout being loaded?

- Mage_Core_Controller_Front_Action
- Mage_Core_Controller_Varien_Action
- Mage_Core_Model_Layout
- Mage_Core_Model_Layout_Handle
- Mage_Core_Model_Layout_Element

## How are layout xml directives processed?

1. Mage_Core_Controller_Varien_Action::loadLayout() – processes the request to load layout
2. Mage_Core_Model_Layout::\_\_construct() – loads layout xml
3. Mage_Core_Model_Layout_Update::load() – load layout updates by handles

## Which configuration adds a file containing layout xml updates to a module?

Modules config.xml

      <frontend>
        <layout>
            <updates>
                <catalog>
                    <file>catalog.xml</file>
                </catalog>
            </updates>
        </layout>


## Why is the load order of layout xml files important?

All layout files are merged in one XML object Mage_Core_Model_Layout_Element.
The later the file is loaded, the less chance it will be overridden by another layout.xml file.

This is why local.xml is loaded last or as of 1.9 the themes.xml file/files.
