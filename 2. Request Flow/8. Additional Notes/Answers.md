# 1. Application Initialization


##  List the events which happen in index.php

- Checks PHP version
- Adds compiler
- Checks for downloader
- Checks for Mage.php
- Mage::run($code, $type)

##  What are the parameters for MAGE_RUN_CODE

- store
- group
- website

##  How do you set a different website in the .htaccess file for Apache web server


    SetEnv MAGE_RUN_CODE website
    SetEnv MAGE_RUN_TYPE de


##  How do you init the Mage_Core_Model_App env without a request

  Mage::app()

##  What is the order of base, module & database loaded and what are their methods and class

1. Base   Mage_Core_Model_Config->loadBase()
2. Modules Mage_Core_Model_Config->loadModules()
3. Database Mage_Core_Model_Config->loadDb()

##  What order are modules loaded in and what method sorts the modules

1. Mage_All.xml
2. Mage*.xml
3. All other xml files

This loaded in *Mage_Core_Model_Config->_getDeclaredModuleFiles()*


##  What is loaded after module files are loaded

Database and then local.xml


##  What is the name of the resource config

Mage_Core_Model_Resource_Config


## Mage_Core_Model_Resource_Config->loadToXml() what does it load into the config

1. Loads website into website node
2. Loads stores into store node
3. Loads config and database config into default node

##  How and when are the two main types of setup script executed?

1. Mage_Core_Model_Resource_Setup::applyAllUpdates()
2. Mage_Core_Model_Resource_Setup::applyAllDataUpdates()


# 2. Front Controller


##  Name the class for the Front, Frontend and Admin router

1. Mage_Core_Controller_Varien_Front
2. Mage_Core_Controller_Varien_Router_Standard
3. Mage_Core_Controller_Varien_Router_Admin

##  What are the use node frontend and admin router

standard and admin

##  What are the 4 routers in order

1. Admin - Mage_Core_Controller_Varien_Router_Admin
2. Standard - Mage_Core_Controller_Varien_Router_Standard
3. CMS - Mage_Cms_PageController
4. Default - Mage_Core_Controller_Varien_Router_Default

##  How do you add a router before the default router

Using the observer controller_front_init_routers

##  What action class does a frontend and admin router extend from

Mage_Core_Controller_Varien_Router_Abstract

##  What is the method a new router needs and what abstract class should it extend from

public function match(Zend_Controller_Request_Http $request)


# 3. Modules

##  What class is responsible for loading Modules

Mage_Core_Model_Config

##  What is the difference regarding module loading between Mage::run() and Mage::app()?

Mage::app() will run Magento without processing a request whereas Mage::run() will process a request.

# 4. Layout Initialization

##  What class is responsible for layout

Mage_Core_Model_Layout

##  What are the 2 methods required to render layout in a Controller

$this->loadLayout()
$this->renderLayout()


##  What is the config for a module to add its own layout xml file


      <frontend>
        <layout>
          <updates>
            <colin_layout>
              <file>custom_layout.xml


##  What does Mage_Core_Controller_Varien_Action->loadLayout() do

1. Generates the layout handles
2. Generates the layouts and blocks


##  How do you add a custom layout handle


    <controller_action_layout_load_before>
      <observers>
          <colin_request_action_layout_load_before>
              <class>colin_request/observer</class>
              <method>controllerActionLayoutLoadBefore</method>
          </inchoo_controller_action_layout_load_before>
      </observers>
    </colin_request_action_layout_load_before>


    public function controllerActionLayoutLoadBefore(Varien_Event_Observer $observer)
    {

        $layout = $observer->getEvent()->getLayout();
        $layout->getUpdate()->addHandle('my_custom_handle');
    }



##  How do you add content from an observer

Use the observer core_block_abstract_prepare_layout_before and add a block
