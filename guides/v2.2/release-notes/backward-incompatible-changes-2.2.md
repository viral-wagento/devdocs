---
layout: default
group: release-notes
subgroup: 05_BC
title: Magento 2.2 backward incompatible changes
menu_title: Magento 2.2 backward incompatible changes
version: 2.1
menu_node: parent
menu_order: 1
github_link: release-notes/backward-incompatible-changes-2.2.md
---

### Overview

Magento 2.2 introduces several major changes that may affect the correct functionality of already released external modules. The purpose of this document is to highlight major changes between Magento 2.1 and 2.2.

### API Changes

Magento 2.2 introduces changes in several API classes. These changes are designed to extend overall API coverage and improve developer experience with new features.

#### Store Module
The Store module management has been updated to allow retrieve store by website id.
**Class:** [`Magento\Store\Api\StoreWebsiteRelationInterface`]({{site.mage2200url}}app/code/Magento/Store/Api/StoreWebsiteRelationInterface.php){:target="_blank"}<br/>
**Action:** Added the interface [`Magento\Store\Api\StoreWebsiteRelationInterface`]<br/>
**Action:** Added the method `getStoreByWebsiteId($websiteId)` to [`\Magento\Store\Model\StoreManager`]<br/>
**Description:** Retrieve store by website id<br/>

#### Customer Module
Change was done in order to add default values to customer attributes.
**Class:** [`Magento\Eav\Api\Data\AttributeDefaultValueInterface`]({{site.mage2200url}}app/code/Magento/Eav/Api/Data/AttributeDefaultValueInterface.php){:target="_blank"}<br/>
**Action:** Added the interface [`Magento\Eav\Api\Data\AttributeDefaultValueInterface`]<br/>
**Action:** Added the getter `getDefaultValue()` and setter `setDefaultValue($defaultValue)` to [`Magento\Customer\Model\Data\AttributeMetadata`]<br/>
**Description:** Retrieve and set default values to customer attributes<br/>

#### MessageQueue Module API
The `Magento\Framework\MessageQueue\ConfigInterface` has been deprecated. The following table lists the deprecated methods and their replacements.

Deprecated method | Use instead | Subsequent calls
--- | ---
`getExchangeByTopic($topicName);` | `\Magento\Framework\MessageQueue\Publisher\ConfigInterface::getPublisher($topic)` | Call `getConnection()`  then `getExchange()`
`getQueuesByTopic($topic);` | `\Magento\Framework\MessageQueue\Topology\ConfigInterface::getQueues()`  | Iterate and call `getName()`
`getConnectionByTopic($topic);` |  `\Magento\Framework\MessageQueue\Publisher\ConfigInterface::getPublisher($topic)`| `getConnection()` `getName()`
`getConnectionByConsumer($consumer);` |  `\Magento\Framework\MessageQueue\Consumer\ConfigInterface::getConsumer($name)` | `getConnection()`
`getMessageSchemaType($topic);` | `\Magento\Framework\Communication\ConfigInterface::getTopic($name)` |  Use getter methods to return information from topic data.
`getConsumer($name);` | `\Magento\Framework\MessageQueue\Consumer\ConfigInterface::getConsumer($name)` |  Extract necessary information using accessor methods.
`getConsumerNames();` | `\Magento\Framework\MessageQueue\Consumer\ConfigInterface::getConsumers()` | Iterate and use `getName()` on every item.
`getBinds();` | `\Magento\Framework\MessageQueue\Topology\ConfigInterface::getExchanges()` | Extract necessary information using accessor methods.
`getPublishers();` | `\Magento\Framework\MessageQueue\Publisher\ConfigInterface::getPublishers()` | Iterate and use getter methods.
`getConsumers();` | `\Magento\Framework\MessageQueue\Consumer\ConfigInterface::getConsumers()` | Iterate and use getter methods.
`getTopic($name);` | `\Magento\Framework\Communication\ConfigInterface::getTopic($name)` `\Magento\Framework\MessageQueue\Publisher\ConfigInterface::getPublisher($topic)`| Use `getTopic($name)` for topic information. Use `getPublisher($name)` for publisher-related clients.
`getPublisher($name);` | `\Magento\Framework\MessageQueue\Publisher\ConfigInterface::getPublisher($name)` | Use getter methods to return requested information.
`getResponseQueueName($topicName);` | Magento\Framework\MessageQueue\Rpc\ResponseQueueNameBuilder::getQueueName($topicName) | -

### Changes in repositories

In Magento 2.2 the behavior of repositories regarding the Filters added to Search Criteria  was unified:

- The boolean OR statement joins Filters inside a single Filter Group.
- The boolean AND statement joins Filter Groups inside a Search Criteria.

 In the scope of this work, the following repositories were affected.

#### Affected CE repositories

- `\Magento\Catalog\Api\AttributeSetRepositoryInterface `
- `\Magento\Catalog\Api\CategoryAttributeRepositoryInterface`
- `\Magento\Catalog\Api\ProductAttributeGroupRepositoryInterface`
- `\Magento\Catalog\Api\ProductAttributeRepositoryInterface`
- `\Magento\Cms\Api\BlockRepositoryInterface`
- `\Magento\Cms\Api\PageRepositoryInterface`
- `\Magento\Eav\Api\AttributeGroupRepositoryInterface`
- `\Magento\Eav\Api\AttributeRepositoryInterface`
- `\Magento\Eav\Api\AttributeSetRepositoryInterface`
- `\Magento\Sales\Api\CreditmemoCommentRepositoryInterface`
- `\Magento\Sales\Api\CreditmemoItemRepositoryInterface`
- `\Magento\Sales\Api\CreditmemoRepositoryInterface`
- `\Magento\Sales\Api\InvoiceCommentRepositoryInterface`
- `\Magento\Sales\Api\InvoiceItemRepositoryInterface`
- `\Magento\Sales\Api\InvoiceRepositoryInterface`
- `\Magento\Sales\Api\OrderAddressRepositoryInterface`
- `\Magento\Sales\Api\OrderItemRepositoryInterface`
- `\Magento\Sales\Api\OrderPaymentRepositoryInterface`
- `\Magento\Sales\Api\OrderStatusHistoryRepositoryInterface`
- `\Magento\Sales\Api\ShipmentCommentRepositoryInterface`
- `\Magento\Sales\Api\ShipmentItemRepositoryInterface`
- `\Magento\Sales\Api\ShipmentRepositoryInterface`
- `\Magento\Sales\Api\ShipmentTrackRepositoryInterface`
- `\Magento\Sales\Api\TransactionRepositoryInterface`
- `\Magento\Ui\Api\BookmarkRepositoryInterface`
- `\Magento\Vault\Api\PaymentTokenRepositoryInterface`

#### Affected EE repositories

- `\Magento\GiftCardAccount\Api\GiftCardAccountRepositoryInterface `
- `\Magento\GiftWrapping\Api\WrappingRepositoryInterface`
- `\Magento\Rma\Api\CommentRepositoryInterface `
- `\Magento\Rma\Model\RmaRepository`
- `\Magento\Rma\Model\Service\RmaManagement `
- `\Magento\Rma\Model\Rma\Status\HistoryRepository`
- `\Magento\Rma\Api\RmaRepositoryInterface `
- `\Magento\Staging\Api\UpdateRepositoryInterface `
- `\Magento\VersionsCms\Api\HierarchyNodeRepositoryInterface`

For details about repositories see the [Searching with repositories]({{page.baseurl}}extension-dev-guide/searching-with-repositories.html) topic.

### Compiler changes

This release removes the multi-tenant compiler option and support of the definitions option for the single tenant compiler in the `env.php` file.

The following classes are no longer available:

* `Magento\Setup\Console\Command\DiCompileMultiTenantCommand`
* `Magento\Framework\ObjectManager\Relations\Compiled`
* `Magento\Framework\ObjectManager\Definition\Compiled\Serialized`
* `Magento\Framework\ObjectManager\Definition\Compiled\Binary`
* `Magento\Framework\Interception\Definition\Compiled`

The `bin/magento setup:config:set` command no longer has the `--definition-format` option.

### Changes in UI

### Database Schema changes

### Database data format changes

This release replaces usages of `unserialize` with [`json_decode`]({{page.baseurl}}extension-dev-guide/framework/serializer.html).

The release also provides upgrade scripts that convert Magento data stored in serialized format.

Extension developers should review the following cases to see what actions they should take for their extensions:

**Case 1:**  
Your extension declared fields for automatic serialization/unserialization using the `_serializableFields` parameter of `\Magento\Framework\Model\ResourceModel\Db\AbstractDb`.  
**Solution:**  
Write an [upgrade script]({{page.baseurl}}ext-best-practices/tutorials/serialized-to-json-data-upgrade.html) to convert your extension data to JSON format.

**Case 2:**  
Your extension stores or adds serialized data to Magento entities (e.g. new/custom attributes for Product).  
**Solution:**  
Your extension will continue working normally, but we recommend switching to JSON for security reasons.
You also need to write an upgrade script for the data in the database.

**Case 3:**  
Your extension relies on serialized data from Magento entities.  
**Solution:**  
Update all usages to expect the data in JSON format.

**Case 4:**  
The serialize/unserialize logic belongs to your extension and stores data in a new table or field.  
**Solution:**  
Your extension will continue working normally, but we recommend switching to JSON for security reasons.
You also need to write an upgrade script for the data in the database.

**Case 5:**  
Your extension accesses values in the `core_config_data` table using the following paths:

* `payment/braintree/countrycreditcard`
* `design/theme/ua_regexp`
* `cataloginventory/item_options/min_sale_qty`
* `currency/options/customsymbol`
* `admin/magento_logging/actions`

**Solution:**  
Update your extension to use `\Magento\Framework\Serialize\Serializer\Json` for serializing/unserializing data instead of the native PHP serialize/unserialize functions.

**Case 6:**  
Your extension uses a backend model that extends `\Magento\Config\Model\Config\Backend\Serialized` to save/load data to/from the database.
**Solution:**  
Write an [upgrade script]({{page.baseurl}}ext-best-practices/tutorials/serialized-to-json-data-upgrade.html) to update data stored by the extension in the `core_config_data` table from serialized to JSON format.

**See:** 

* [Serialize to JSON data upgrade]({{page.baseurl}}ext-best-practices/tutorials/serialized-to-json-data-upgrade.html)
* [Serialize Library]({{page.baseurl}}extension-dev-guide/framework/serializer.html)

#### Staging (EE Only)

### Persistence management

#### Data interfaces persistence