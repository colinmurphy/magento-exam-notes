# Which ways exist in Magento to add router classes

There are 2 ways:
1. Web -> routers in config.xml
2. Observer


# 1. Web -> Routers

*app/etc/code/local/Colin/Request/config.xml*

    <global>
      <default>
        <web>
          <routers>
            <my_router_name>
              <area>my_area_name</area>
              <class>Colin_Request_Controller_Router</class>
            </my_router_name>
          </routers>
        </web>

*app/etc/code/local/Colin/Request/Controller/Router.php*


    class Colin_Request_Controller_Router extends Mage_Core_Controller_Varien_Router_Abstract
    {
      public function match(Zend_Controller_Request_Http $request)
      {
          // Match URL and router here
      }
    }


# 2. Observer


*app/code/local/Colin/Request/config.xml*

    <global>  
      <events>
          <controller_front_init_routers>
              <observers>
                  <colin_request_add_router>
                      <type>singleton</type>
                      <class>Colin_Request_Model_Observer</class>
                      <method>controllerFrontInitRouters</method>
                  </colin_request_add_router>
              </observers>
          </controller_front_init_routers>
      </events>
    </global>


*app/code/local/Colin/Request/Model/Observer.php*


    class Colin_Request_Model_Observer extends Mage_Core_Model_Abstract
    {

        public function controllerFrontInitRouters($observer)
        {
            $front = $observer->getEvent()->getFront();
            $router = new Colin_Request_Controller_Router_First();
            $front->addRouter('first', $router);
        }

    }


*app/code/local/Colin/Request/Controller/Router.php*

    class Colin_Request_Controller_Router_First extends Mage_Core_Controller_Varien_Router_Abstract
    {
        public function match(Zend_Controller_Request_Http $request)
        {
            // Do custom check and add controller before
        }
    }
