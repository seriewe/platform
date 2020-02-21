UPGRADE FROM 6.1.x to 6.2
=======================

Table of content
----------------

* [Core](#core)
* [Administration](#administration)
* [Storefront](#storefront)
* [Refactorings](#refactorings)

Core
----

* The usage of `entity` in the `shopware.entity.definition` tag is deprecated and will be removed with 6.4.
    * Therefore change:
        `<tag name="shopware.entity.definition" entity="product"/>`
      To:
        `<tag name="shopware.entity.definition"/>`
    * As a fallback, this function is used first 
* We deprecated the `LongTextWithHtmlField` in 6.2, use `LongTextField` with `AllowHtml` flag instead
* The Mailer is not overwritten anymore, instead the swiftmailer.transport is decorated.
    * Therefore the MailerFactory returns a Swift_Transport Instance instead of Swift_Mailer
    * The MailerFactory::create Method is deprecated now

    Before: 
    ```
    new LongTextWithHtmlField('content', 'content')
    ```
  
    After:  
    ```
    (new LongTextField('content', 'content'))->addFlags(new AllowHtml()
    ```
* CartBehavior::isRecalculation is deprecated and will be removed in version 6.3
* Please use context permissions instead:
    * Permissions can be configured in the SalesChannelContext.
    * `CartBehavior` is created based on the permissions from `SalesChannelContext`, you can check the permissions at this class.
    * Permissions exists:
         `ProductCartProcessor::ALLOW_PRODUCT_PRICE_OVERWRITES`
         `ProductCartProcessor::SKIP_PRODUCT_RECALCULATION`
         `DeliveryProcessor::SKIP_DELIVERY_RECALCULATION`
         `PromotionCollector::SKIP_PROMOTION`
    * Define permissions for AdminOrders at class `SalesChannelProxyController` within the array constant `ADMIN_ORDER_PERMISSIONS`.
    * Define permissions for the Recalculation at class `OrderConverter` within the array constant `ADMIN_ORDER_PERMISSIONS`.
    * Extended permissions with subscribe event `SalesChannelContextPermissionsChangedEvent`, see detail at class `SalesChannelContextFactory`
    
Administration
--------------

* `sw-settings-custom-field-set`
    - Removed method which overrides the mixin method `getList`, use the computed `listingCriteria` instead
    - Add computed property `listingCriteria`
* `sw-settings-document-list`
    - Removed method which overrides the mixin method `getList`, use the computed `listingCriteria` instead
    - Add computed property `listingCriteria`
* Refactor  `sw-settings-snippet-list`
    - Removed `StateDeprecated`
    - Remove computed property `snippetSetStore`, use `snippetSetRepository' instead
    - Add computed property `snippetSetRepository`
    - Add computed property `snippetSetCriteria`
* Refactor `sw-settings-snippet-set-list`
    - Remove `StateDeprecated`
    - Remove computed property `snippetSetStore`, use `snippetSetRepository' instead
    - Add computed property `snippetSetCriteria`
    - The method `onConfirmClone` is now an asynchronous method
* Refactor mixin `sw-settings-list.mixin`
    - Remove `StateDeprecated`
    - Remove computed property `store`, use `entityRepository` instead
    - Add computed property `entityRepository`
    - Add computed property `listingCriteria`
* The component sw-plugin-box was refactored to use the "repositoryFactory" instead of "StateDeprecated" to fetch and save data
        - removed "StateDeprecated"
        - removed computed "pluginStore" use "pluginRepository" instead
* The component sw-settings-payment-detail was refactored to use the "repositoryFactory" instead of "StateDeprecated" to fetch and save data
    - removed "StateDeprecated"
    - removed computed "paymentMethodStore" use "paymentMethodRepository" instead
    - removed computed "ruleStore" use "ruleRepository" instead
    - removed computed "mediaStore" use "mediaRepository" instead
* Refactor the module `sw-settings-number-range-detail`
    * Remove LocalStore
    * Remove StateDeprecated
    * Remove data typeCriteria
    * Remove data numberRangeSalesChannelsStore
    * Remove data numberRangeSalesChannels
    * Remove data numberRangeSalesChannelsAssoc
    * Remove data salesChannelsTypeCriteria
    * Remove computed numberRangeStore use numberRangeRepository instead
    * Remove computed firstSalesChannel
    * Remove computed salesChannelAssociationStore
    * Remove computed numberRangeStateStore use numberRangeStateRepository instead
    * Remove computed salesChannelStore use salesChannelRepository instead
    * Remove computed numberRangeTypeStore use numberRangeTypeRepository instead
    * Remove method onChange
    * Remove method showOption
    * Remove method getPossibleSalesChannels
    * Remove method setSalesChannelCriteria
    * Remove method enrichAssocStores
    * Remove method onChangeSalesChannel
    * Remove method configHasSaleschannel
    * Remove method selectHasSaleschannel
    * Remove method undeleteSaleschannel

* Refactored `sw-newsletter-recipient-list`
    * Removed LocalStore
    * Removed StateDeprecated
    * Removed Computed salesChannelStore, use salesChannelRepository instead
    * Removed Computed tagStore, use tagRepository instead
    * Removed Computed tagAssociationStore

* Refactored mapErrorService
    * Deprecated `mapApiErrors`, use `mapPropertyErrors`
    * Added `mapCollectionPropertyErrors` to mapErrorService for Entity Collections
* Deprecated `sw-multi-ip-select` with version 6.4, use the `sw-multi-tag-ip-select`-component instead

Storefront
----------

* We removed the SCSS skin import `@import 'skin/shopware/base'` inside `/Users/tberge/www/sw6/platform/src/Storefront/Resources/app/storefront/src/scss/base.scss`.
    * If you don't use the `@Storefront` bundle in your `theme.json` and you are importing the shopware core `base.scss` manually you have to import the shopware skin too in order to get the same result:

        Before
        ```
        @import "../../../../vendor/shopware/platform/src/Storefront/Resources/app/storefront/src/scss/base.scss";
        ```

        After
        ```
        @import "../../../../vendor/shopware/platform/src/Storefront/Resources/app/storefront/src/scss/base.scss";
        @import "../../../../vendor/shopware/platform/src/Storefront/Resources/app/storefront/src/scss/skin/shopware/base";
* We changed the storefront ESLint rule `comma-dangle` to `never`, so that trailing commas won't be forcefully added anymore


Refactorings
------------


