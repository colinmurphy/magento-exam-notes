# Define/identify basic terms and elements of ACL

# 1. Overview

ACL stands for Access Control lists. These are used in Magento to authenticate users to use the admin also web API's.

These are configured in the adminhtml.xml files of the modules.

There are 3 parts to ACL

**1. Resources**

Individual parts of the system.

**2. Roles**

The roles accessible to a user.

**3. Users**

The user who is assigned a role.


# 2. Create Menu

For this we are going to create a menu item and give a user access to this.

**Note:** Please note that this xml could be put in the config.xml but would be config/adminhtml instead of config.

So in our adminhtml.xml file we would add the following:


    <config>
        <menu>
            <colin_slider>
                <title>Slider</title>
                <sort_order>80</sort_order>

                <children>
                    <slider translate="title" module="colin_adminhtml">
                        <title>Slider</title>
                        <sort_order>10</sort_order>
                        <action>colin_adminhtml/index</action>
                    </slider>
                </children>
            </colin_slider>
        </menu>


As you can see we are adding the following:

1. A top level menu item called "Slider"
2. A link to our slider controller "/slider/index/".


But in order for user to see this their must be a ACL role registered and added to their role.

So in the adminhtml.xml we add the following:


    <acl>
        <resources>
            <admin>
                <children>
                    <colin_slider>
                        <title>Slider</title>
                        <sort_order>100</sort_order>
                        <children>
                            <slider>
                                <title>Slider</title>
                                <sort_order>200</sort_order>
                            </slider>
                        </children>
                    </colin_slider>
                </children>
            </admin>
        </resources>
    </acl>



# 4. How does ACL work

To understand this we will create a role for "Slider Administrator" who will only have access to the "slider" ACL role.


Users are stored in the **admin_role** table

| role_id     | parent_id     | tree_level     | sort_order     | role_type     | user_id     | role_name     |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| 3 | 1 | 1 | 0 | G | 1 | Administrator Slider |
| 4 | 1 | 2 | 0 | U | 2 | Colin |


The rules are then stored in the **admin_rule** table.

| rule_id     | role_id     | resource_id     | privileges     | assert_id     | role_type     | permission     |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| 22 | 3 | admin/sales/order | NULL | 0 | G | deny |
| 90 | 3 | admin/colin_slider/slider | NULL | 0 | G | allow |

As you can see the Slider Administrator is only allowed to view the slider.

**Note:** Administrators have a special privilege called "All" which allows them to view everything.


This is checked in the observer **Mage_Adminhtml_Controller_Action->preDispatch**


    if ($this->getRequest()->isDispatched()
        && $this->getRequest()->getActionName() !== 'denied'
        && !$this->\_isAllowed()) {
        $this->\_forward('denied');
        $this->setFlag('', self::FLAG_NO_DISPATCH, true);
        return $this;
    }

The *_isAllowed* method is set to true but is overridden by the controller.

So in our index controller we add the following:

      protected function \_isAllowed()
      {
          Mage::getSingleton('admin/session')->isAllowed('colin_slider/slider');
      }

This refers to the privilege in the **admin_rule** table *colin_slider/slider*.


# Questions

## For what purpose is the *_isAllowed()* method used and which class types implement it?

It is used to check to see does the user have correct permission to view that request.

## What is the XML syntax for adding new menu element?


    <config>
        <menu>
            <colin_slider>
                <title>Slider</title>
                <sort_order>80</sort_order>

                <children>
                    <slider translate="title" module="colin_adminhtml">
                        <title>Slider</title>
                        <sort_order>10</sort_order>
                        <action>colin_adminhtml/index</action>
                    </slider>
                </children>
            </colin_slider>
        </menu>

## What is adminhtml.xml used for? Which class parses it, and which class applies it?

    It is used for setting menu items and ACL. This can be also be added instead in the config.xml under the adminhtml node.


## Where is the code located that processes the ACL XML and where is the code that applies it?


    Mage_Admin_Model_Config::loadAclResources()


## What is the relationship between Magento and Zend_Acl?

Mage_Admin_Model_Acl extends Zend_Acl

## How is ACL information stored in the database?

See above

# Further Reading

- [http://magecert.com/adminhtml.html](http://magecert.com/adminhtml.html)
- [http://alanstorm.com/magento_acl_authentication](http://alanstorm.com/magento_acl_authentication)
-  [http://brideo.co.uk/magento-certification-notes/adminhtml/Access-Control-Lists-(ACL)-and-Permissions-in-Magento/](http://brideo.co.uk/magento-certification-notes/adminhtml/Access-Control-Lists-(ACL)-and-Permissions-in-Magento/)
- [http://goweb.vn/kien-thuc-web-vi/thiet-ke-web/63-define-identify-basic-terms-and-elements-of-acl--928.html](http://goweb.vn/kien-thuc-web-vi/thiet-ke-web/63-define-identify-basic-terms-and-elements-of-acl--928.html)
