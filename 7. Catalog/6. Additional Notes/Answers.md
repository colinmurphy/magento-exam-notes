# 1. Product Type


## What 2 things do we need to do to create a product type

1. Config - global/product/type/{{product_type}}/params
2. Model extends Mage_Catalog_Model_Product_Type_Abstract

## What is the xpath for a new product type

global/product/type/{{product_type}}/params

## What are the parameters for a new product type

1. model - Product Type
2. price model - should extend Mage_Catalog_Model_Product_Type_Price
3. composite - parent product or not
4. indexer_priority - Indexer priority
5. can_use_decimal - Whether quantity can use decimal

If super/parent product


    <allow_product_types>
      <simple />
      <virtual />
    </allow_product_types>


## How do we implement a new price model

1. Extend from Mage_Catalog_Model_Product_Type_Price
2. Change functionality in getFinalPrice()

## What option is required to allow product type have child products

composite

## What node is used to set allowed child products of a parent product type

allow_product_types

## How do we get a type instance class

$product->getTypeInstance()

## What table is grouped products relationships stored in

catalog_product_link

## What is link_type_id for cross sells, upsells and related products

Cross Sells - 5
Upsells - 4
Related Products - 1


## What table is configurable products relationships stored in

catalog_product_super_link

## What table is pricing for associated products for configurables stored in

catalog_product_super_attribute_pricing

## What table is downloads stored in

download_link

## What table is download pricing stored in

download_link_price

## What table is associated bundle products stored in

catalog_product_bundle_selection

## What table are parent/child products stored in

catalog_product_relation

## What table is custom options stored in

catalog_product_option

## What table is tiered pricing stored in

catalog_product_entity_tier_price


# 3. Price Generation

## What are the different price models

1. Default - Mage_Catalog_Model_Product_Type_Price
2. Grouped - Mage_Catalog_Model_Product_Type_Grouped_Price
3. Configurable - Mage_Catalog_Model_Product_Type_Configurable_Price (combines price from catalog_product_super_attribute_pricing)
4. Bundle - Mage_Bundle_Model_Product_Price
5. Downloadable - Mage_Downloadable_Model_Product_Price

## How do you change a price

2 ways:

1. Add a price_model to the product type and extend from Mage_Catalog_Model_Product_Type_Price
2. Use an observer catalog_product_get_final_price

## Under what circumstances are product prices read from the index tables?

Using collections

## How do custom product options influence price calculation?

If a custom product option has a price, then this is appended to the price of the product.

## How are product tier prices implemented and displayed?

Tiered pricing is stored catalog_product_entity_tier_price The price model check the quantity against the quantity in this table along with customer group and website/store.

They are then displayed in template/catalog/product/view/tierprices.phtml



# 4. Catalog Rule

## What module is responsible for catalog price rules

Mage_CatalogRule

## What tables are catalog price rules added to

1. catalogrule - Stores each rule
2. catalogrule_customer_group - Stores the rule id and customer group id
3. catalogrule_group_website - Stores the rule id and website id
4. catalogrule_product - Stores the product and type of discount when rules are applied.
5. catalogrule_product_price - Stores the actual discount for each product for each website.

## How are catalog prices applied and what class and method is used to apply the discount

Using 4 observers

1. catalog_product_get_final_price - Gets price
2. prepare_catalog_product_collection_prices - Gets collection index price
3. prepare_catalog_product_price_index_table - Updates price value for index table
4. catalog_product_type_configurable_price - Updates price for configurable product

Mage_CatalogRule_Model_Rule->applyAll() used to apply prices and store them in the catalogrule_product_price tables.

## What are the 4 types of discounts

1. to_percent
2. from_percent
3. to_fixed_amount
4. from_fixed_amount

## If a discount is applied to the catalog price rule and shopping cart rule and they both are set to stop processing which discounts get applied

Both are applied. The shopping cart rule would apply to the price applied from the catalog price rule.


# 5. Tax


## What module deals with Tax

Mage_Tax

## Name 2 tax class types

Product
Customer

## What are the main tax tables

1. tax_class - stores the tax classes for products and customer_address_save_after
2. tax_calculation - stores the tax calculation
3. tax_calculation_rates stores the tax calculation rates for a rule
4. tax_calculation_rule - stores the id of


## What are the parameters for Tax Rate

1. id
2. country
3. state
4. zip code
5. rate
6. zip in range (yes/no) - from/to

## What are the main tax options for configuration and how do they affect pricing

1. Tax Calculation Method Based on - total,row or unit price
2. Tax Calculation Based on - shipping, billing or origin
3. Prices include/exclude tax
4. Apply Tax - Before/After discount
5. Apply Discount on Prices - Excluding/Including Tax
6. Apply Tax On - Custom Price if available/original price
7. Enable Cross Border Trade -  When catalog price includes tax, enable this setting will fix the price no matter what the customer's tax rate is.

## If 2 tax rules have the same priority and apply to the address, what happens

The latest rule gets applied.

## If 2 tax rules have different priorities and apply to the address, what happens

They both get applied.

## If 2 tax rates in a rule, what happens

The last tax rate gets applied.

## What are the 3 options for Tax Calculation Method Based On

1. Total
2. Unit Price
3. Row

## How is tax calculated and in what class and method

Using totals and Mage_Tax_Model_Calculation->getRates()
