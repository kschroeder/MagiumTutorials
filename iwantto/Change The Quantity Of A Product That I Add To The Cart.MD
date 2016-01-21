# I Want To Change The Quantity Of A Product That I Add To The Cart

Changing the quantity of something you add to your Magento shopping cart is quite simple.  Any add to cart functionality will eventually make its way to one of the `Add*ProductToCart` classes.  And because the dependency injection container will provide the same instance to you as it does to any other dependent class you can quite easily make the quantity modifications at any point prior to it being called.

 Consider this example where the original `AddItemToCart` class is used.

```
<?php

use Magium\Magento\AbstractMagentoTestCase;
use Magium\Magento\Actions\Cart\AddItemToCart;
use Magium\Magento\Actions\Cart\AddSimpleProductToCart;

class ChangeSimpleProductQuantityTest extends AbstractMagentoTestCase
{

    public function testChangeQty()
    {
        $this->commandOpen($this->getTheme()->getBaseUrl());

        // Changing the quantity of the product being added to the cart
        $this->getAction(AddSimpleProductToCart::ACTION)->addQty(2);

        $addToCart = $this->getAction(AddItemToCart::ACTION);
        /* @var $addToCart \Magium\Magento\Actions\Cart\AddItemToCart */

        $addToCart->addSimpleItemToCartFromProductPage();

        self::assertEquals(2, $this->webdriver->byCssSelector('.qty')->getAttribute('value'));
    }

}
```