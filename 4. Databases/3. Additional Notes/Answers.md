# 1. Models, Resource Models & Collections

## How do you register a custom table for a Resource Model

    <models>
        <colin_database>
            <class>Colin_Database_Model</class>
            <resourceModel>colin_database_resource</resourceModel>
        </colin_database>
        <colin_database_resource>
            <class>Colin_Database_Model_Resource</class>
            <entities>
                <results>
                    <table>football_results</table>
                </results>
            </entities>
        </colin_database_resource>
    </models>

Can be accessed:

  Mage::getModel('colin_database/results');
  Mage:getModel('core_resource')->getTableName('colin_database/results');


## What is the xpath for the table name for the custom table

global/models/{{name}}/resourceModel/{{resourceModelName}}

global/models/{{resourceModelName}}/
  /class/{{className}}
  /{{tableName}}/{{name}}

## How do you register a model, resource model and collection to use a custom table

  using the init method foreach.

- Model: used to register resource Model
- Resource Model: used to register table and main field. If the 2nd param is empty, then id is used.
- Collection: used to register model and resource model. If 2nd parameter is empty, then model used.

## Name abstract classes for Model, Resource Model and Collections

- Model: Mage_Core_Model_Abstract
- Resource Model: Mage_Core_Model_Resource_Db_Abstract
- Collection: Mage_Core_Model_Resource_Db_Collection_Abstract

## How do you register a setup script in the config.xml

    <global>
      <resorces>
          <colin_database_setup>
              <setup>
                  <module>Colin_Database</module>
              </setup>
          </colin_database_setup>


## What is the name of the class does $this refer to in a SQL setup script

- Non EAV: Mage_Core_Model_Resource_Setup
- EAV: Mage_Eav_Model_Entity_Setup

## What is the name of the class does $this->getConnection() refer to in a SQL setup script

This is the adapter class Magento_Db_Adapter_Pdo_Mysql

## How do you register a custom connection

    <global>
      <resources>
          <wordpress_db>
              <connection>
                  <host><![CDATA[127.0.0.1]]></host>
                  <username><![CDATA[root]]></username>
                  <password><![CDATA[root]]></password>
                  <dbname><![CDATA[wp_exam]]></dbname>
                  <initStatements><![CDATA[SET NAMES utf8]]></initStatements>
                  <model><![CDATA[mysql4]]></model>
                  <type><![CDATA[pdo_mysql]]></type>
                  <active>1</active>
                  <persistent>0</persistent>
              </connection>
          </wordpress_db>
          <wordpress_read>
              <connection>
                  <use>wordpress_db</use>
              </connection>
          </wordpress_read>
          <wordpress_write>
              <connection>
                  <use>wordpress_db</use>
              </connection>
          </wordpress_write>

Then in your code:

    $connection = Mage::getModel('core/resource')->getConnection('wordpress_read');

## How do you override the default_setup connection outside of local.xml file

Add a local_dev.xml to app/etc directory and set "default_setup" use param to a new connection:

    <resources>
      <default_setup>
        <connection>
          <use>dev_setup</use>
        </connection>
      </default_setup>
      <dev_setup>
          <connection>
              <host><![CDATA[127.0.0.1]]></host>
              <username><![CDATA[root]]></username>
              <password><![CDATA[root]]></password>
              <dbname><![CDATA[mag_exam]]></dbname>
              <initStatements><![CDATA[SET NAMES utf8]]></initStatements>
              <model><![CDATA[mysql4]]></model>
              <type><![CDATA[pdo_mysql]]></type>
              <pdoType><![CDATA[]]></pdoType>
              <active>1</active>
          </connection>
      </dev_setup>
    </resources>


## What are the different ways of loading a row with Mage::getModel()

    Mage::getModel('colin_database/results')->load($id)
    Mage::getModel('colin_database/results')->load($field, $id)
    Mage::getModel('colin_database/results')->getCollection()
      ->addFieldToFilter($field, $id)

## How would you save additional data after a product was saved

Using the observer model_after_save.

    <global>
        <events>          
            <catalog_product_save_after>
                <observers>
                    <mymodule>
                        <type>singleton</type>
                        <class>mymodule/observer</class>
                        <method>catalog_product_save_after</method>
                    </mymodule>
                </observers>
            </catalog_product_save_after>



        public function catalog_product_save_after($observer)
        {
            $product = $observer->getProduct();
            if(!$product->getMetaTitle()){
                $name = $product->getName();           
                $metaTitle = str_replace(' - ', ' ', $name);
                $product->setMetaTitle($metaTitle);
                $product->getResource()->saveAttribute($product, 'meta_title');
            }
        }




## How do you get Varien_Db_Select in Magento

    Mage::getModel('core/resource')->getConnection('core_read')->select();
    Mage::getModel('colin_database/results')->getCollection()->getSelect();

## How do you filter a field in a collection

Non EAV: Mage::getModel('colin_database/results')->addFieldToFilter();
EAV: Mage::getModel('colin_database/results')->addAttributeToFilter();

## How do you filter a attribute in a collection

Mage::getModel('colin_database/results')->addAttributeToFilter();

## How do you order a Collections

    $collection->setOrder($field, $dir);
    $collection->getSelect()->order('field dir');

## How do you set a limit for a Collection

    $collection->setCurPage(1)->setPageSize(5);
    $collection->getSelect()->limit(5);

# 2. Install/Update Scripts

## What is the name of the method to get Table name for Mage_Core_Model_Resource and Mage_Core_Model_Mysql4_Abstract

- Mage_Core_Model_Resource->getTableName($modelEntity);
- Mage_Core_Model_Mysql4_Abstract->getTable($entityName);

## How do you create a table with a install script

$this->addColumn()

## How do you start and end a setup script

- $this->startSetup()
- $this->endSetup()

## Name the methods which are called for install/upgrade setup and data scripts in the Request Initialization

- Mage_Core_Model_Resource::applyAllUpdates();
- Mage_Core_Model_Resource::applyAllDataUpdates();


## path to install setup script and data script

- data/{setup_name}/data-install-{version}.php
- sql/{setup_name}/install-{version}.php

## path to upgrade setup script and data script

- data/{setup_name}/data-upgrade-{old_version}-{new_version}.php
- sql/{setup_name}/install-{old_version}-{new_version}.php
