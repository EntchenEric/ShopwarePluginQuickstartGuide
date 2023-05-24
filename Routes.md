>__Note__
>
>I will show you how you can create a localhost/jobs page, but you will be able to create any route you want with this tutorial. Please make shure you have a [Skeleton Plugin](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/GettingStarted.md)

# Step 1: Create the Controller

- Inside of the `src` folder, create a `Storefront` folder and inside of that create a `Controller` folder. Inside of the Controller folder create a file accordingly to the route you want to create. **IMPORTANT: the file has to end with `Controller` and it has to be a php file. Best Practice: Write it in UpperCamelCase

Valid Names:
- JobController.php
- MyRouteController.php

Invalid Names:
- Job.php
- JobRoute.php
- JobControler.php (note that you spell Controller right)
-  Route.js


- Copy and Paste the base code from [Shopwares Docs](https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#storefront-controller-class-example) or from here:
```
<?php declare(strict_types=1);

namespace Swag\BasicExample\Storefront\Controller;

use Shopware\Core\System\SalesChannel\SalesChannelContext;
use Shopware\Storefront\Controller\StorefrontController;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

/**
 * @Route(defaults={"_routeScope"={"storefront"}})
 */
class ExampleController extends StorefrontController
{
    /**
    * @Route("/example", name="frontend.example.example", methods={"GET"})
    */
    public function showExample()
    {
    }
}
```
-   you can directly copy and paste from the second ExampleController.php file, but remove the contents inside of the `showExample` function for now. 
-   **IMPORTANT:** change the Namespace. You namespace has to be `[prefix]\[pluginName]\Storefront\Controller` e.g. `namespace Eric\DemoPlugin\Storefront\Controller;`
-   also change the name of the Functions accordingly
-   Please also remove the `: Response` from the showExample function
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

Paste the following Code I got and slightly edited from the [Shopware Docs](https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#services.xml-example) and paste it under the Services:
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

# Step3: Add the Template to the Route

Inside of the Resources folder create a Folder called `views`, inside there `storefront` and inside of that create a Folder called `page` *Make shure to write everything in small letters only*
Here create a [twig](https://twig.symfony.com) file called `[name].html.twig`. 
Example:
jobs.html.twig

now Copy and Paste [This code from the docs](https://developer.shopware.com/docs/guides/plugins/plugins/storefront/add-custom-controller#adding-template) or copy it from here:
```twig
{% sw_extends '@Storefront/storefront/base.html.twig' %}

{% block base_content %}
    <h1>Our example controller!</h1>
{% endblock %}
```

Inside of your php file from earlyer, you now have to add the following code to your function:
```php
return $this->renderStorefront(@[pluginName with Prefix]/storefront/page/[twig file name].html.twig);
```
(when filled out the line shuld look like this: `return $this->renderStorefront(@EricDemoPlugin/storefront/page/jobs.html.twig);`)

if you now reload your localhost/[route] page, you will see the theme. However the contents of e.g. your header are not synced. 

# Step4: Understand how to pass Data to the twig file

inside of the renderStorefront function, add a Array as second attribute like this: `return $this->renderStorefront(@[pluginName with Prefix]/storefront/page/[twig file name].html.twig,[]);`
Inside of this Array you can Pass values to the twig file via `[key] => [value]`. when complete, it looks like this:
```php
return $this->renderStorefront(@EricDemoPlugin/storefront/page/jobs.html.twig,[
    'heading' => 'Welcome to our Job Page!';
]);
```

If you want to see what data you can display in the twig file, you can add `{{ dump() }}` to your twig file like this:
```twig
{% sw_extends '@Storefront/storefront/base.html.twig' %}

{% block base_content %}
    {{ dump() }}
    <h1>Our example controller!</h1>
{% endblock %}
```

This will then display a Array where you can see all the keys and values you have access to, including the heading.
To now use the values in your twig file just add {{ [key] }}. This will display the value of the key. If the Key does not exist, nothing will be displayed. So make shure you spell your key right if you dont get your exspected output.
In our Example this looks like this:
```twig
{% sw_extends '@Storefront/storefront/base.html.twig' %}

{% block base_content %}
    <h1>{{ header }}</h1>
{% endblock %}
```

# Step5: Generic Page loading (Loading in the Header with its contents)

In your Controller.php file you created in Step 1, add the following lines:
```php
use Shopware\Storefront\Page\GenericPageLoaderInterface;
use Shopware\Core\System\SalesChannel\SalesChannelContext;
```
in your Class [Not in the function inside of the class] now Create this variable: `private GenericPageLoaderInterface $genericPageLoader;`

Now add a constructor inside of the class:
```php
public function __construct(GenericPageLoaderInterface $genericPageLoader){
    $this->genericPageLoader = $genericPageLoader;
}
```

you have to add the following attributes to your function: `Request $request, SalesChannelContext $context` and also add the `: Response` back you removed in Step 1

now Inside of your function, add this code:
```php
$page = $this->genericPageLoader->load($request, $context);
```

and then just pass the page variable with the value 'page' to your twig file. Make shure you spell it `page` because Shopware will autodetect it and handle everything for you.

your class shuld now look like this:
```php

```
