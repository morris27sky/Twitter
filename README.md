Pre req:

php 5.3

Installation
================
mkdir behat-twitter

cd behat-testing-ftw

curl -s http://getcomposer.org/installer | php

vi composer.json

Composer
=================
php composer.phar install

Dir should look like this
bin     composer.json   composer.lock   composer.phar   vendor


Now run bin/behat --init which should creat the following

+d features - place your *.feature files here
+d features/bootstrap - place bootstrap scripts and static files here
+f features/bootstrap/FeatureContext.php - place your feature related code here

Feature
================
cd features
vi TwitterDirectMessaging.feature

copy and paste the feature from the feature file TwitterDirectMessaging.feature


featureContext
===============

should look like this

<?php

use Behat\Behat\Context\ClosuredContextInterface,
    Behat\Behat\Context\TranslatedContextInterface,
    Behat\Behat\Context\BehatContext,
    Behat\Behat\Exception\PendingException;
use Behat\Gherkin\Node\PyStringNode,
    Behat\Gherkin\Node\TableNode;

use Behat\MinkExtension\Context\MinkContext;
use Behat\MinkExtension\Context\RawMinkContext;

use Selenium\Client as SeleniumClient;

use OrangeDigital\BusinessSelectorExtension\Context\BusinessSelectorContext;

//
// Require 3rd-party libraries here:
//
require_once 'PHPUnit/Autoload.php';
require_once 'PHPUnit/Framework/Assert/Functions.php';
//

/**
 * Features context.
 */
class FeatureContext extends BehatContext
{
 
    public $output;
    public $parameters;
 
    public function __construct(array $parameters)
    {
        $this->parameters = $parameters;
        $this->useContext('mink', new MinkContext($parameters));
        //$this->useContext('business', new BusinessSelectorContext($parameters));
    }
    
    protected function loadYaml($path) {
        if (!file_exists($path)) {
            throw new \RuntimeException('File: ' . $path . ' does not exist');
        }

        $parser = new \Symfony\Component\Yaml\Parser();

        $string = file_get_contents($path);

        $result = $parser->parse($string);

        if (!$result) {
            throw new \RuntimeException('Unable to parse ' . $path);
        }

        return $result;
    }


    public function getSession($name = null)
    {
      return $this->getSubcontext('mink')->getSession($name);
    } 

