# I Want To Search For A Product

Searching for a product is done through the use of the search action combined with the search result extractor.  Magento has two means of doing searches.  One is through the search box (as shown in the example here), another is via the dropdown suggestions.

```
<?php

use Magium\Magento\AbstractMagentoTestCase;
use Magium\Magento\Actions\Search\Search;
use Magium\Magento\Extractors\Catalog\Search\SearchSuggestions;

class SearchTest extends AbstractMagentoTestCase
{
    public function testSearch()
    {
        $this->commandOpen($this->getTheme()->getBaseUrl());
        $this->getAction(Search::ACTION)->search('search');
        $extractor = $this->getExtractor(\Magium\Magento\Extractors\Catalog\Search\SearchResults::EXTRACTOR);
        /* @var $extractor \Magium\Magento\Extractors\Catalog\Category\Search */
        $extractor->extract();
        $productList = $extractor->getProducts();
        // Do assertions
    }
}
```

Here is the code for doing a test that clicks on a search suggestion.  Note the use of the search suggestion extractor.

```
<?php

use Magium\Magento\AbstractMagentoTestCase;
use Magium\Magento\Actions\Search\Search;
use Magium\Magento\Extractors\Catalog\Search\SearchSuggestions;

class SearchTest extends AbstractMagentoTestCase
{
    public function testSearchSuggestions()
    {

        $this->commandOpen($this->getTheme()->getBaseUrl());
        $this->getAction(Search::ACTION)->type($this->partialSearch);
        $extractor = $this->getExtractor(SearchSuggestions::EXTRACTOR);
        $extractor->extract();

        $suggestions = $extractor->getSuggestions();

        self::assertGreaterThan(0, count($suggestions));
        self::assertGreaterThan(0, $suggestions[0]->getCount());
        self::assertContains($this->partialSearch, $suggestions[0]->getText());

        $suggestions[0]->getElement()->click(); // If it does not work an exception will be thrown.
    }
}
```