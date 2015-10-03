### 1. How and when is the include path set up and the auto loader registered?

- These are loaded once the Mage file has been included in the index.php before Mage::run();
- This uses an array of set_include_paths to search the 3 codepools and lib directories for files.
- Classes are loaded by separating the underscore as the directory separator in the autoload class and found in the 4 include directories.

More info on autoloading can be found [here](https://github.com/colinmurphy/magento-exam-notes/blob/master/1.%20Basics/1.%20Architecture/6.%20Explain%20class%20naming%20conventions%20and%20their%20relationship%20with%20the%20autoloader.md)

### 2. How and when does Magento load the base configuration, the module configuration, and the database configuration?

1. Base  - Mage_Core_Model_Config->loadBase()
2. Module - Mage_Core_Model_Config->loadModules()
3. Database - Mage_Core_Model_Config->loadDb()


### 3. How and when are the two main types of setup script executed?

- Mage_Core_Model_Resource_Setup->applyAllUpdates() - Resource Scripts
- Mage_Core_Model_Resource_Setup->applyAllDataUpdates() - Data Scripts

applyAllUpdates() called in *Mage_Core_Model_App->_initModules()* after module config loaded.
applyAllDataUpdates called in *Mage_Core_Model_App->run()* after modules loaded and store parameters set.


### 4. When does Magento decide which store view to use, and when is the current locale set?

1. In the index.php we set the run type and code.
2. Then after the module config is loaded we call - Mage_Core_Model_App->run() calls *Mage_Core_Model_App->_initCurrentStore();*

#### Mage_Core_Model_App->\_initCurrentStore();

This sets the property current store by doing a switch statement on the type (website/store/group) and then getting the code from the config XML.


### 5. Which ways exist in Magento to specify the current store view?

#### 1. index.php/.htaccess

$\_SERVER['Mage_RUN_CODE'] and $\_SERVER['Mage_RUN_TYPE'];

e.g.

    SetEnv MAGE_RUN_TYPE "website"
    SetEnv MAGE_RUN_CODE "german"


### 6. When are the request and response objects initialized?

Early on when Mage_Core_Model_App->run() is called.
Though the request and response would need to set as values in the options which is not passed in the method by default.
