# 1. Admin Order Creation


## How do you prevent a payment method being used by an admin order

Set the property *$_canUseInternal* to false in the payment method.

## What is the controller for creating admin Orders

Mage_Adminhtml_Sales_Order_CreateController

## What is abstract class for admin order controllers

Mage_Adminhtml_Controller_Action

## How does Magento calculate price when an order is created from the admin

In *Mage_Adminhtml_Sales_Order_CreateController_processActionData()*

## Which steps are necessary in order to create an order from the admin?

1. Create Order Under Sales -> Orders
2. Select Customer
3. Select Store and enter details
4. Add Products
5. Set Billing Address
6. Set Shipping Address
7. Set Shipping Method
8. Set Payment Method
9. Place Order

## What happens when existing orders are edited in the admin?

They are re-created with using the same order number and a "-1" added to the order number


# 2. Order Status & State

## Difference between status and state

State is an internal state for Magento which can have multiple statuses

## List all statuses

Answer from [http://magento.stackexchange.com/a/516/6617](http://magento.stackexchange.com/a/516/6617)

- **Pending:** Pending orders are brand new orders that have not been processed. Typically, these orders need to be invoiced and shipped.
- **Pending PayPal:** Pending PayPal orders are brand new orders that have not been cleared by PayPal. [...]
- **Processing:** Processing means that orders have either been invoiced or shipped, but not both.
- **Complete:** Orders marked as complete have been invoiced and have shipped.
- **Cancelled:** Cancelled orders should be used if orders are cancelled or if the orders have not been paid for.
- **Closed:** Closed orders are orders that have had a credit memo assigned to it and the customer has been refunded for their order.
- **On Hold:** Orders placed on hold must be taken off hold before continuing any further actions.  

## List all the states in order

1. new
2. pending_payment
3. processing
4. complete
5. closed
6. canceled
7. holded


## How do you add your own state


  <global>
    <sales>
      <order>
        <states>
          <new_state>
            <label>New State</label>
            <description>This is a state description</state>
            <status>
              <pending default="1" />
              <pending_paypal />
            </status>
            <visible_on_front>1</visible_on_front>
          </new_state>
        </states>


## When would an order be closed

When its refunded/credit memo.

## If an order is invoiced or shipped, what status would it be

Processing.

## If an order is complete, can it be refunded (credit memo)

No. If the order has been completed, it can't be refunded.
Can also depend on payment method allowing refunds.

# Card Authorization and Capture

## Which class is responsible for capturing funds

The payment method which is called by the invoice class

## When are invoices created

When an order is billed or in admin.

## What class is the payment class

Mage_Sales_Model_Order_Payment

## Whats the difference between authorize and capture

1. Authorize - It checks the funds and informs the payment service to block those funds from being used.
2. Capture - This captures the funds.

## What are the different types of captures

Online & Offline Capture.

## How would a custom payment gateway authorize and capture funds

Using the methods authorize and capture providing their properties have been set to true.


# 5. Credit Memos


# What state is an order set to after being refunded

Closed.

# When can an order not be refunded?

When the order is complete and the payment method does not allow refunds.

# How does Magento process taxes when refunding an order?

It basis it off admin tax origin.

# How does Magento process shipping fees when refunding an order?

Handled by the Credit Memo model when calculating different totals.

# What is the difference between online and offline refunding?

Offline refunds refund the order on Magento without actually taking care of the refund transaction. Whereas online does both.

# What is the role of the credit memo total models in Magento?

Credit memo totals keep a track on how much is refunded per total.

# 6. Cancelled Orders

## When can an order be cancelled

1. Payment method allows for the order to be void e.g. the property *$_canVoid*
2. If all items have not been invoiced
3. If the order is not cancelled, complete or closed
4. Or if the order is not already flagged as cancelled

# 7. Partial Order Operations

## What are the different types

1. Order
2. Invoice
3. Shipping

## How do they affect the state of an order

They set the order state to processing.
