## 1. What does "Magento loads modules" mean?

1. app/etc/modules/
2. Sort modules
3. config.xml is loaded and added to config tree

## 2. In which order are Magento modules loaded?

1. Mage_All.xml
2. Mage xml
3. Any other xml files

This is set when returning array of files to be loaded.

## 3. Which core class loads modules?

Mage_Core_Model_Config

## 4. What are the consequences of one module depending on another module?

The module must be loaded and exist or otherwise it will cause Magento to throw an exception.

## 5. During the initialization of Magento, when are modules loaded in?

They are loaded before Front Controller Dispatch and after caching has been checked.

## 6. Why is the load order important?

Dependencies and core functionality

## 7. What is the difference regarding module loading between Mage::run() and Mage::app()?

**Note:** Important.

Mage::app() – Initialize application without request processing.
Mage::run() – Run application. Run process responsible for request processing and sending response.
