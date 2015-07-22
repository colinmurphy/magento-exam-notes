## Which events are associated with sending output?

There are 4:

- controller_front_init_before
- controller_front_init_routers
- controller_front_send_response_before
- controller_front_send_response_after


## Which class is responsible for sending output?

Mage_Core_Controller_Response_Http

## What are possible issues when this output is not sent to the browser using the typical output mechanism, but is instead sent to the browser directly?

Layout, Blocks not rendered


## How are redirects handled?

With the core_url_rewrite table in the class *Mage_Core_Model_Url_Rewrite_Request*
