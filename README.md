# behat-with-dusk
How to integrated Behat and Dusk simply. This way doesn't need Mink Extension. 

Let's install Behat and Laravel Dusk via composer.

    composer require laravel/dusk behat/behat --dev

If your Laravel version is below under 5.4, you need to register ServiceProvider of Laravel Dusk.

Initialize Behat

    php artisan dusk:install
    cd tests
    php ../vendor/bin/behat --init

Open tests/features/bootstrap/FeatureContext.php file and update it like below.

    use Behat\Behat\Context\Context;
    use Tests\DuskTestCase;
    use Laravel\Dusk\Browser;
    
    /**
     * Defines application features from the specific context.
     */
    class FeatureContext extends DuskTestCase implements Context
    {
        public function __construct($name = null, array $data = [], $dataName = '')
        {
            parent::__construct($name, $data, $dataName);
            parent::setUp();
            static::startChromeDriver();
        }
        /**
         * @When /^visit homepage$/
         */
        public function visitHomePage()
        {
            $this->browse(function (Browser $browser) {
                $browser->visit('/');
            });
        }
    }


Make Behat feature file in tests/features folder. Add Feature and Scenario.

    Feature: BDD Test
      In order to learn how to integrate Behat with Laravel Dusk
      Scenario: visit homepage
        When visit hompage
        Then response code should be 200

Done. Run Behat. 

    cd tests
    php ../vendor/bin/behat
    



