### 1. How does the framework discover active modules and their configuration?


    Mage_Core_Model_Config->_loadDeclaredModules()


This method searches for any xml files under app/etc/modules and loads the modules.
Firstly it calls *_getDeclaredModuleFiles* to get all the files

It then loops through the files and does the following:

- Loads the module configuration
- Adds the modules names which it depends into an array
- Sorts the modules it depends on in *_sortModuleDepends*
- Creates the configuration *Mage_Core_Model_Config_Base* and adds the XML *<config><modules/></config>* to the class
- Adds modules to the configuration
- Adds the modules it depends on to the configuration

### 2. What are the common methods with which the framework accesses its configuration values and areas?


- Mage::->getStoreConfig($path);
- Mage::app()->getStore()->getConfig($path);
- Mage::getStoreConfigFlag($path); *Checks if a value exists*
- Mage::getConfig()->getNode($path);


### 3. How are per-store configuration values established in the XML DOM?

The per-store config values are defined in the config.xml file of your module under the <stores> node and under the <store_code>

**Example:** If you wanted to override the theme

    <stores>
        <default>
            <design>
                <package>
                    <name>default</name>
                </package>
                <theme>
                    <default>modern</default>
                </theme>
            </design>
        </default>
    </stores>


You can also do this with the <websites> and <website_code> nodes


    <websites>
        <base>
            <design>
                <package>
                    <name>default</name>
                </package>
                <theme>
                    <default>modern</default>
                </theme>
            </design>
        </base>
    </websites>




**Note:** Answer taken from [http://magestudyguide.com/#how-are-per-store-configuration-values-established-in-the-xml-dom?](http://magestudyguide.com/#how-are-per-store-configuration-values-established-in-the-xml-dom?)


### 4. By what process do the factory methods and autoloader enable class instantiation?

- Varien_Autoload
- spl_autoload_register

For further information please read [Explain how Magento loads and manipulates configuration information](https://github.com/colinmurphy/magento-exam-notes/blob/master/1.%20Basics/2.%20Configuration/1.Explain%20how%20Magento%20loads%20and%20manipulates%20configuration%20information.md)

### 5. Which class types have configured prefixes, and how does this relate to class overrides?

Not too sure about this question but I think this is what it means:

You can override classes in 2 ways:

- Config Rewrite
- Adding a class file relative in another codePool that is loaded beforehand.
e.g. create app/code/local/Mage/Core/Model/Product.php to override Mage_Catalog_Model_Product

This is not recommended but can be done due to the multiple paths used in set_include_path() in app/Mage.php.


### 6. Which class types and files have explicit paths?

Not sure what this means?

### 7. What configuration parameters are available for event observers?

**type**

- model
- object
- none
- singleton

**class**

Name of the class of the observer e.g. Colin_Bootstrap_Model_Observer


**method**

Name of the method called

### 8. What are the interface and configuration options for automatically fired events?

This is registering an observer.

### 9. What is the structure of event observers, and how are properties accessed therein?

This is how an observer works e.g.

      Mage::dispatchEvent('catalog_product_get_final_price', array('product' => $product, 'qty' => $qty));


Parameters are then accessed as follows:

    public function myObserverMethod($observer)
    {
      $product = $observer->getEvent()->getProduct();
      $qty = $observer->getEvent()->getQty();
      /* Do something */
    }

### 10. What configuration parameters are available for cron jobs?

Under global -> events -> crontab -> name_of_cron there are:

- schedule -> cron_expr
- run -> model

Example:

      <global>
          <crontab>
              <jobs>
                  <colin_bootstrap_cron>
                      <schedule>
                         <cron_expr>*/1 * * * *</cron_expr>
                      </schedule>
                      <run>
                          <model>colin_bootstrap/observer_cron::runCustomCronJob</model>
                      </run>
                  </colin_bootstrap_cron>
              </jobs>
          </crontab>
