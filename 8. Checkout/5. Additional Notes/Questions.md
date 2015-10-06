# 1. Quote

- What are the main tables for a quote and what their purpose
- How does a a single shipping and MultiShipping quote differ
- What is the class for a one page checkout and a MultiShipping checkout
- Where is additional information for a quote stored
- Difference between grand_total and base_grand_total
- What class and method adds a product to the cart/quote
- How do product types apply add their own data to a quote when a product is added to the cart
- When exactly does inventory decrementing occur
- When exactly does card authorization and capturing occur


# 2. Totals

- How do you register a total
- What are the main parameters for a total
- What is the order in which totals are applied to a quote
- What class calculates and applies the totals
- What are the default totals in Magento in order
- How do you add a custom total
- How do you display a custom total
- What are the 4 types totals are used for

# 3. Shopping Cart Rules

- Which module is responsible for shopping cart price rules?
- What is the difference between shopping cart and catalog price rules?
- What are the limitations of Magento shopping cart rules?

# 4. Shipping Methods

- How do you register a shipping method
- What parameters are for a shipping method
- What abstract class and interface does a shipping method use
- What methods are required for a shipping method and also what property should be set
- How do you set shipping methods
- How do you set the rate
- What should be added to a Mage_Shipping_Model_Rate_Result_Method


# 5. Payment Methods

- What is the xpath for a payment method
- What are the configuration options for a method
- What class does a payment method extend from and what methods are required
- How would you add and validate custom data
- How would you enable logging for debugData
- What property would you set to capture the full credit card number
- What property would you set to prevent refunds
- What properties would you set to authorize and capture
- What property would you use to prevent Multishipping
- What property would you use to prevent use in admin

# 6. Multishipping

- How does the storage of quotes for multishipping and onepage checkouts differ?
- Which quotes in a multishipping checkout flow will be virtual?
- How can different product types be split among multiple addresses when using multishipping in Magento?
- How many times are total models executed on a multishipping checkout in Magento?
- Which model is responsible for multishipping checkout in Magento?
