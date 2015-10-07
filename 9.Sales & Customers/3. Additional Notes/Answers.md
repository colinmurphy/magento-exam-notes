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

**Pending:** Pending orders are brand new orders that have not been processed. Typically, these orders need to be invoiced and shipped.
**Pending PayPal:** Pending PayPal orders are brand new orders that have not been cleared by PayPal. [...]
**Processing:** Processing means that orders have either been invoiced or shipped, but not both.
**Complete:** Orders marked as complete have been invoiced and have shipped.
**Cancelled:** Cancelled orders should be used if orders are cancelled or if the orders have not been paid for.
**Closed:** Closed orders are orders that have had a credit memo assigned to it and the customer has been refunded for their order.
**On Hold:** Orders placed on hold must be taken off hold before continuing any further actions.  

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
          <statues>
            <pending default="1" />
            <pending_paypal />
          </statues>
          <visible_on_front>1</visible_on_front>
        </new_state>
      </states>


## When would an order be closed

When its refunded/credit memo.

## If an order is invoiced or shipped, what status would it be

Processing.

## If an order is complete, can it be refunded (credit memo)

No. If the order has been shipped it can't be refunded.
