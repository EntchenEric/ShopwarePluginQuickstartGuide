
I will show you how you can create a localhost/jobs page, but you will be able to create any route you want with this tutorial.

#Step 1: Create the Controller

- Inside of the `src` folder, create a `Storefront` folder and inside of that create a `Controller` folder. Inside of the Controller folder create a file accordingly to the route you want to create. **IMPORTANT: the file has to end with `Controller` and it has to be a php file. Best Practice: Write it in UpperCamelCase
-   Valid Names:
-     JobController.php
-     MyRouteController.php
-   Invalid Names:
-     Job.php
-     JobRoute.php
-     JobControler.php (note that you spell Controller right)
-     Route.js
- Copy and Paste the base code from [Shopwares Docs](https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#storefront-controller-class-example)
-   you can directly copy and paste from the second ExampleController.php file, but remove the contents inside of the `showExample` function for now. 
-   **IMPORTANT:** change the Namespace. You namespace has to be `[prefix]\[pluginName]\Storefront\Controller` e.g. `namespace Eric\DemoPlugin\Storefront\Controller;`
-   also change the name of the Functions accordingly
-   for now remove the parameters from the showExample function your renamed. Also remove the `: Response`
- Now you have to change the contents of the `@Route("/example", name="frontend.example.example", methods={"GET"})` to `@Route("/[name of Route]", name="frontend.[name of Route]", methods={"GET"})`
-   example: `@Route("/jobs", name="frontend.jobs", methods={"GET"})`
- For testing purposes echo some HTML Code into your function

How your file shuld look now:
```php
<?php declare(strict_types=1);

namespace Eric\DemoPlugin\Storefront\Controller;

use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;
use Shopware\Storefront\Page\GenericPageLoaderInterface;
use Shopware\Storefront\Controller\StorefrontController;
use Shopware\Core\Framework\Routing\Annotation\RouteScope;
use Shopware\Core\System\SalesChannel\SalesChannelContext;

/**
 * @Route(defaults={"_routeScope"={"storefront"}})
 */
class JobController extends StorefrontController
{
    /**
    * @Route("/jobs", name="frontend.jobs", methods={"GET"})
    */
    public function showJobs()
    {
      echo '<h1>This text will be displayed!</h1>';
      exit;
    }
}
```

# Step 2: Create routes.xml

If you now try to go to localhost/jobs, you will get an Error. Thats because we have to create a routes.xml first Before we can Do that, we have to set our Controller as a service.

### Open the `services.xml` file located in `src/Ressources/config`

Paste the following Code I got from the (Shopware Docs)[https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#services.xml-example] and paste it under the Services:
```xml
    <service id="[prefix]\[pluginName]\Storefront\Controller\[className]" public="true">
        <argument type="service" id="Shopware\Storefront\Page\GenericPageLoader" />
            <call method="setContainer">
                <argument type="service" id="service_container" />
            </call>
    </service>
```
Change the Squared Brackets accordingly.

Example of your `service.xml`:
```xml
<?xml version="1.0"?>

<container xmlns="http://symfony.com/schema/dic/services"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://symfony.com/schema/dic/services http://symfony.com/schema/dic/services/services-1.0.xsd">

    <services>

        <service id="Eric\DemoPlugin\Storefront\Controller\JobController" public="true">
        <argument type="service" id="Shopware\Storefront\Page\GenericPageLoader" />
            <call method="setContainer">
                <argument type="service" id="service_container" />
            </call>
        </service>
      
    </services>
</container>
```

Now create inside of the `src/Ressources/config` folder a file called `routes.xml`and Copy and Paste the contents from the [Shopware Docs](https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#routes.xml-example) or copy them from here:
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<routes xmlns="http://symfony.com/schema/routing"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://symfony.com/schema/routing
        https://symfony.com/schema/routing/routing-1.0.xsd">

    <import resource="../../Storefront/Controller/**/*Controller.php" type="annotation" />
</routes>
```

If you now go to [localhost/jobs](http://localhost:80/jobs) you will be on your own page. however this Page does not contain the theme of the store nor the Header and footer. 

## No route found
Try clearing the Cash at the [admin panel](http://localhost/admin#/sw/settings/cache/index) under settings -> System -> Cashes & Indexes -> Clear Cashes

## Your Folder Structure shuld now look like this:
```
developement
|-shopware-apps
|-shopware-plugins
  |-EricDemoPlugin (or your Plugin Name)
    |-src
      |-Resources
        |-config
          |-service.xml
          |-routes.xml
      |-Storefront
        |-Controller
          |-JobController.php (or similar)
      |-EricDemoPlugin.php (or your Plugin Name)
    |-composer.json
```
