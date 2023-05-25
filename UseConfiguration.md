# Before you start, please make shure you have [created a Configuration](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/AddConfiguration.md)

First of all, you have to add the SystemConfigService to your services.xml. 

Just add `<argument type="service" id="Shopware\Core\System\SystemConfig\SystemConfigService" />` as one argument in your service where you want to Use your Configuration. 

Inside your php file, you now have to add the following line `use Shopware\Core\System\SystemConfig\SystemConfigService;`.

Now lets create a variable: `private SystemConfigServie $systemConfigService;`

inside of your __construct function, you now have to add `$systemConfigService` as an Attribute.

Then you will have to set the systemConfigService to the $systemConfigService via `$this->systemConfigService = $systemConfigService`

Your __construct function shuld now look like this (if you also followed the [route guide](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/Routes.md):
```php
    private GenericPageLoaderInterface $genericPageLoader;
    private SystemConfigService $systemConfigService;
 
    public function __construct(GenericPageLoaderInterface $genericPageLoader, SystemConfigService $systemConfigService)
    {
        $this->genericPageLoader = $genericPageLoader;
        $this->systemConfigService = $systemConfigService
    }
```

To get the Data from the congig into a variable, you can use this code: `$configuration = $this->systemConfigService->get('[PluginName].config.[configAttribute]');`
Example: `$header = $this->systemConfigService->get('EricDemoPlugin.config.jobPageHeader');`

then just pass the information to your Route like we did in [Step 4 of the route guide](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/Routes.md)

> __Warning__
>
> If you get an Error, try to [Clear the Cash](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/sideguids/clearCash.md)


If you followed the [route guide](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/Routes.md) your code looks now like this:
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
use Shopware\Core\System\SystemConfig\SystemConfigService;

/**
 * @Route(defaults={"_routeScope"={"storefront"}})
 */
class JobController extends StorefrontController
{
    private GenericPageLoaderInterface $genericPageLoader;
    private SystemConfigServie $systemConfigService;
 
    public function __construct(GenericPageLoaderInterface $genericPageLoader, SystemConfigService $systemConfigService)
    {
        $this->genericPageLoader = $genericPageLoader;
        $this->systemConfigService = $systemConfigService
    }


    /**
    * @Route("/jobs", name="frontend.jobs", methods={"GET"})
    */
    public function showJobs(Request $request, SalesChannelContext $context) :Response
    {
        $page = $this->genericPageLoader->load($request, $context); 
        $Heading = $this->systemConfigService->get('EricDemoPlugin.config.Title');

        return $this -> renderStorefront('@EricDemoPlugin/storefront/page/jobs.html.twig', [
            'heading' => $Heading,
            'page' => $page
        ]);
    }
}
```
