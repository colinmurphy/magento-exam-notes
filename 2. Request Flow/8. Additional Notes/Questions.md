# 1. Application Initialization

- List the events which happen in index.php
- What are the parameters for MAGE_RUN_CODE
- What are the parameters for MAGE_RUN_TYPE
- How do you set a different website in the .htaccess file for Apache web server
- How do you init the Mage_Core_Model_App env without a request
- What is the order of base, module & database loaded and what are their methods and class
- What order are modules loaded in and what method sorts the modules
- What is loaded after module files are loaded
- What is the name of the resource config (Mage_Core_Model_Resource_Config)
- Mage_Core_Model_Resource_Config->loadToXml() what does it load into the config
- How and when are the two main types of setup script executed?

# 2. Front Controller

- Name the class for the Front, Frontend and Admin router
- What are the use node frontend and admin router
- What are the 4 routers in order
- How do you add a router before the default router
- What action class does a frontend and admin router extend from
- What is the method a new router needs and what abstract class should it extend from
- What are the xpaths for web configuration

# 3. Modules
- What class is responsible for loading Modules
- What is the difference regarding module loading between Mage::run() and Mage::app()?

# 4. Layout Initialization
- What class is responsible for layout
- What are the 2 methods required to render layout in a Controller
- What is the config for a module to add its own layout xml file
- What does Mage_Core_Controller_Varien_Action->loadLayout() do
- How are directives parsed
- How do you add a custom layout handle
- How do you add content from an observer
