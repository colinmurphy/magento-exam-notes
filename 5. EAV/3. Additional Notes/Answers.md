# 1. Concepts

## Name all 8 entity types

| Name     | Entity Type Code     | Entity Model     |
| :------------- | :------------- | :-------------     |
| 1. Customer      | customer       | customer/customer     |
| 2. Customer Address      | customer_address       | customer/address     |
| 3. Product      | catalog_product       | catalog/product     |
| 4. Category      | catalog_category       | catalog/category     |
| 5. Order      | order       | sales/order     |
| 6. Invoice      | invoice       | sales/order_invoice     |
| 7. Credit Memo      | creditmemo       | sales/order_creditmemo     |
| 8. Shipment      | shipment       | sales/shipment     |

## How do you load all 8 types

    Mage::getModel('customer/customer')
    Mage::getModel('customer/address')
    Mage::getModel('catalog/product')
    Mage::getModel('catalog/category')
    Mage::getModel('sales/order')
    Mage::getModel('sales/order_invoice')
    Mage::getModel('sales/order_creditmemo')
    Mage::getModel('sales/shipment')


## Name the table entity types are stored from

eav_entity_type

## How do we change the setup class for a resource

Add a class to the setup in config.
The xpath would be global/resources/name_of_resource/setup/class


## What is the name of the abstract class for EAV Resource Model & Collection

- Resource Model: Mage_Eav_Model_Entity_Abstract
- Collection: Mage_Eav_Model_Entity_Collection_Abstract

## How do we register a new EAV type

1. Create module with model, resource model, table and setup script in config.
2. Resource Model extends from Mage_Eav_Model_Entity_Abstract and sets a type using setType
3. Collection extends from Mage_Eav_Model_Entity_Collection_Abstract and sets the resource model
4. Setup class extends from or is Mage_Eav_Model_Entity_Setup
5. Run $this->addEntityType($name, array $params) which creates the entity type in eav_entity_type table
6. Add tables to resource model and run $this->createEntityTables($table)
7. Add Attributes using $this->addAttribute()

## How do we create tables for the EAV type

$this->createEntityTables($table)

## Name all fields which can be passed in to create a EAV type and what their purpose is


## Name all the entity attribute value table types (text, decimal)

1. char
2. text
3. datetime
4. int
5. varchar
6. decimal



# 2. Attributes

## Do you use entity_type_code or entity_model when creating/update EAV attribute

entity_type_code. We only use entity model when getting Model/collection.

## Differences between create and update attribute

$this->addAttribute($eav_type, $name, array $params)
$this->updateAttribute($eav_type, $attribute, $column, $value); e.g. update frontend_model


## How do you get attribute value for a model

echo Mage::getSingleton('eav/config')
    ->getAttribute("colin_eav_blog", 'content')
    ->getFrontend()
    ->getExcerpt($blog, 10);
