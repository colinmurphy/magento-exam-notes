# 1. Quote


## What are the main tables for a quote and what their purpose

1. sales_flat_quote
2. sales_flat_quote_address
3. sales_flat_quote_address_item
4. sales_flat_quote_item
5. sales_flat_quote_item_option
6. sales_flat_quote_payment
7. sales_flat_quote_shipping_rate


## How does a a single shipping and MultiShipping quote differ

Multiple addresses are stored in sales_flat_quote_address_item table


## What is the class for a one page checkout and a MultiShipping checkout

Mage_Checkout_OnepageController
Mage_Checkout_MultishippingController

They differ as they extend from Mage_Checkout_Controller_Action instead of Mage_Core_Controller_Front_Action

## Where is additional information for a quote stored

sales_flat_quote_item_option

## Difference between grand_total and base_grand_total

1. Grand Total - total based off the stores currency
2. Base Grand Total - total based off the base currency

## What class and method adds a product to the cart/quote

Mage_Checkout_Model_Cart->addProduct($product, $params)

## How do product types apply add their own data to a quote when a product is added to the cart

Using the *_prepareProduct($Varien_Object $buyRequest, $product, $processMode)* method.

## When exactly does inventory decrementing occur

When an order is created.

## When exactly does card authorization and capturing occur

When an order is placed.


# 2. Totals


## How do you register a total

Add a new total under global/sales/quote/totals/{{name}}/params

The class would then extend from Mage_Sales_Model_Quote_Address_Total_Abstract and have a collect and fetch method.

## What are the main parameters for a total

1. class - Total Class extends from Mage_Sales_Model_Quote_Address_Total_Abstract
2. before - comma separated totals to load before
3. after - comma separated totals to load after
4. renderer
5. admin_renderer

## What is the order in which totals are applied to a quote

| Name | Code     |
| :------------- | :------------- |
| Nominal       | nominal       |
| Subtotal       | subtotal       |
| Shipping       | shipping       |
| Tax       | tax       |
| Grand Total       | grand_total       |


## What class calculates and applies the totals

Mage_Sales_Model_Quote_Address()->collectTotals() calls the class Mage_Sales_Model_Quote_Address_Total_Collector

## How do you add a total

Using the collect method and using *_addAmount* and *_addBaseAmount* methods.

    $this->\_addAmount($price);
    $this->\_addBaseAmount($price);


## How do you display a custom total

Using the fetch method and adding a total to the address object

    $address->addTotal(
        array(
            'code'  => $this->getCode(),
            'title' => $title,
            'value' => $amount
        )
    );
    return $address;

## What are the 4 types totals are used for

1. Quote
2. Order
3. Invoice
4. Credit Memo

# 3. Shopping Cart Rules

## Which module is responsible for shopping cart price rules?

Mage_SalesRule

## What is the difference between shopping cart and catalog price rules?

Cart applies rule to quote whereas price rules apply to products in the cart.

## What are the limitations of Magento shopping cart rules?

1. Only 1 voucher can be applied at once but multiple rules can be applied.
2. Rules cannot rely on each other
3. For admin area orders, shopping cart price rules can be disabled on specific items but they are always applied on all items in the frontend.


# 4. Shipping Methods


## How do you register a shipping method

1. config under default/carrier
2. class extends Mage_

## What parameters are for a shipping method

## What abstract class and interface does a shipping method use

## What methods are required for a shipping method and also what property should be set

## How do you set shipping methods

## How do you set the rate

## What should be added to a Mage_Shipping_Model_Rate_Result_Method


# 5. Payment Methods


## What is the xpath for a payment method

config/default/payment/{{name}}

## What are the configuration options for a method

1. active
2. model
3. order_status
4. title
5. payment_action
6. debug


## What class does a payment method extend from and what methods are required

Mage_Payment_Model_Method_Abstract and no methods required but properties can be set to configure model.


## How would you add and validate custom data

1. Add custom block with extends Mage_Payment_Block_Form
2. Add a method assignData and $this->getInfoInstance()->set{{name}}($data->{{name}});
3. Add a method validate and throw an exception if it fails

## How would you enable logging for debugData

Set debug to 1/true

## What property would you set to capture the full credit card number

$_canSaveCc = false;

## What property would you set to prevent refunds

$_canRefund = false

## What properties would you set to authorize and capture

$_canCapture = true and $_canAuthorize = true

## What property would you use to prevent Multishipping

$_canUseForMultishipping = false

## What property would you use to prevent use in admin

$_canUseInternal = false

# 6. Multishipping


## How does the storage of quotes for multishipping and onepage checkouts differ?

Quote Items are stored in **flat_sales_quote_address_item**.

## Which quotes in a multishipping checkout flow will be virtual?

Virtual Products will be added to **flat_sales_quote_address_item** but it does not have a shipping address. Instead the item is added to the billing address.

## How can different product types be split among multiple addresses when using multishipping in Magento?

The items are added to flat_sales_quote_address_item and then they set a parent address id which is based in flat_sales_quote_address table.

## How many times are total models executed on a multishipping checkout in Magento?

For each address.

## Which model is responsible for multishipping checkout in Magento?


| One Page Checkout     | Multi Shipping     |
| :------------- | :------------- |
| Mage_Checkout_OnepageController | Mage_Checkout_MultishippingController |
| Mage_Checkout_Model_Type_Onepage | Mage_Checkout_Model_Type_Mulitishipping |
