# 1. Architecture

## What is the order of codepools loaded

- local
- community
- core

## Name all locations an Autoloader will search for a file

1. app/code/local/
2. app/code/community/
3. app/code/core/
4. lib/


## Name the class of the Autoloader

Varien_Autoload

Found: lib/Varien/Autoload.php

## How do you register a parent theme

In app/design/frontend/{{package}}/{{theme}}/etc/theme.xml

    <?xml version="1.0"?>
    <theme>
        <parent>rwd/default</parent>
    </theme>

## Where can JavaScript files be located

1. lib/
2. skin/{{area}}/{{package}}/{{theme}}/js/

## How do you register JavaScript in your theme

Class: Mage_Page_Block_Html_Head

    <reference name="head">

      <action method="addItem">
        <type>skin_js</type>
        <url>js/init.js</url>
      </action>

      <action method="addItem">
        <type>js</type>
        <url>package/lib.js</url>
      </action>

      <action method="addJsIe">
        <url>js/ie.js</url>
      </action>

      <action method="addJs">
        <url>package/lib.js</url>
      </action>

    </reference>

## How do you resolve a Configuration conflict

Depend one module on the other.

    <config>
      <modules>
        <Module_Two>
          <active>true</active>
          <codePool>community</codePool>
          <depends><Module_One/></depends>
        </Module_Two>
      </modules>
    </config>

*depends make the Module_Two load before Module_One*


## How do you resolve a Rewrite conflict

Remove one rewrite and extend the second rewrite on the first.
Or you could use depend which will load one of rewrites before the other and make changes to that class.


# 2. Configuration

## What is the xpath for Mage_Core_Model_Sales_Order

config/global/models/sales/rewrite/order/

## How do you rewrite Mage_Core_Model_Sales_Order

      <global>
        <models>
          <sales>
            <rewrite>
              <order>Colin_Two_Model_Rewrite_Sales_Order</order>
            </rewrite>
          </sales>
        </models>
      </global>

## What is the xpath for a rewrite for Mage_Page_Block_Html_Head

config/global/blocks/page/html_head/

## When a factory file is not found, where is the last place the method checks e.g. Mage::getModel('basics/html_head')

Mage_Basics_Html_Head


## How do you register a controller

    <config>
      <frontend>
        <routers>
          <colin_basics_router>
            <use>standard</use>
            <args>
              <module>Colin_Basics</module>
              <frontName>football</frontName>

## How do you rewrite the catalog controller

    <frontend>
        <routers>
            <catalog>
                <use>standard</use>
                <args>
                    <module>Colin_Bootstrap</module>
                    <frontName>catalog</frontName>
                </args>
            </catalog>
        </routers>
    </frontend>

## How do you load a controller before or after the catalog controller

Using the *modules* parameter.

    <frontend>
        <routers>
            <catalog>
                <args>
                    <modules>
                        <colin_request before="Mage_Catalog">Colin_Request</colin_request>
                    </modules>
                </args>
            </catalog>
        </routers>
    </frontend>

## How do you create an event for an observer

Two Parts:

1. Dispatch Event


    Mage::dispatchEvent('name', array $args);
    Mage::dispatchEvent('catalog_product_get_final_price', array('product' => $product, 'qty' => $qty));


2. Register Observer


      <global>
          <events>
              <catalog_product_get_final_price>
                  <observers>
                      <colin_duplicate_price>
                          <type>singleton</type>
                          <class>Colin_Bootstrap_Model_Observer_Price</class>
                          <method>duplicateFinalPrice</method>
                      </colin_duplicate_price>
                  </observers>
              </catalog_product_get_final_price>
          </events>


## Name the observer class

Varien_Event_Observer

## How do you register a CRON

    <global>
        <crontab>
            <jobs>
                <colin_bootstrap_cron>
                    <schedule>
                       <cron_expr>{{cron expression}}</cron_expr>
                    </schedule>
                    <run>
                        <model>colin_bootstrap/observer_cron::runCustomCronJob</model>
                    </run>
                </colin_bootstrap_cron>
            </jobs>
        </crontab>


      class Colin_Bootstrap_Model_Observer_Cron extends Mage_Core_Model_Abstract
      {
          public function runCustomCronJob(Mage_Cron_Model_Schedule $observer)
          {
              Mage::log("Cron has ran.", null, 'cron.log', true);
              return $this;
          }

      }

## What does Mage_Cron_Model_Observer::dispatch() do

1. Dispatches events that need to be dispatched now (in the past and now)
2. Schedule tasks for the future
3. Cleanup table and clears history and expired jobs

## How do you register default values for a store in the config.xml


    <config>
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


## How do you register default values for a website in the config.XML


    <config>
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

## How do you get an instance of a block

Mage::getBlockSingleton()
Mage::app->getLayout()->createBlock()->toHtml();

## Difference between getModel and getSingleton

getSingleton only load a module once

## How do you get version of Magento

Mage::getVersionInfo()
Mage::getVersion();

## How do you register a variable

Mage::register();

## What is Mage::registry used for

To store classes with a singleton pattern.



# 3. Internationalization

## What is the class for translations

Mage_Core_Model_Translate


## Name the different type of translations

Module Translations: found under app/locale/{{locale}}/{{Module_Name}}.csv
Theme Translations: found under app/{{area}}/{{package}}/{{theme}}/locale/{{locale}}/translations.csv
Inline Translations: stored in core_translate table

## What is the order of translations being loaded

1. Module
2. Theme
3. Inline/Database

Meaning that Database is the highest priority.


## How does developer mode affect translations

When turned on it prevents us using translations of other modules.


## How does a module add a translation file


    <translate>
        <modules>
            <Mage_Core>
                <files>
                    <default>Mage_Core.csv</default>
                </files>
            </Mage_Core>
        </modules>
    </translate>
