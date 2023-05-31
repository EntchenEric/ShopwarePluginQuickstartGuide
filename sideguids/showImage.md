To Show a Image you have first get the UUID of the image. Assuming you are using the config, you can get it using `[PluginName].config.[label]`.

Then inside of your file e.g. your Controller, you have to input some usings:
```
use Shopware\Core\Framework\DataAbstractionLayer\Search\Criteria;
use Shopware\Core\Framework\DataAbstractionLayer\EntityRepository;
```

and lets create a private mediaRepository using `private EntityRepository $mediaRepository;` inside of your class.
Now extend your construct function by one argument: `EntityRepository $mediaRepository`

Your construct function shuld now look somewhat like this:

```
    private GenericPageLoaderInterface $genericPageLoader;
    private SystemConfigService $systemConfigService;
    private EntityRepository $mediaRepository;


    public function __construct(
        GenericPageLoaderInterface $genericPageLoader,
        SystemConfigService $systemConfigService,
        EntityRepository $mediaRepository
    ) {
        $this->genericPageLoader = $genericPageLoader;
        $this->systemConfigService = $systemConfigService; // Correct assignment
        $this->mediaRepository = $mediaRepository;
    }
```

Now, we have to get the image URL from the media database. This is fairly simple:
```
        $medias = $this->mediaRepository->search(new Criteria([$imageID]), $context)->first(); //imageID has to be in []
```
If not done, you have to add a extra argument `Context $context` to your function.

You can now get the URL of the image using the getUrl() function e.g. like `$medias->getUrl()`.

Your function shuld look somewhat like this:
```

    /**
     * @Route("/jobs", name="frontend.jobs", methods={"GET"})
     */
    public function showJobs(Request $request, SalesChannelContext $salesChannelContext, Context $context): Response
    {
        $page = $this->genericPageLoader->load($request, $salesChannelContext);
        $Heading = $this->systemConfigService->get('[pluginName].config.Title');
        $StartingText = $this->systemConfigService->get('[pluginName].config.StartingText');
        $imageID = $this->systemConfigService->get('[pluginName].config.img');
        //dd($imageID);
        $medias = $this->mediaRepository->search(new Criteria([$imageID]), $context)->first();
        return $this->renderStorefront('@[pluginName]/storefront/page/jobs.html.twig', [
            'heading' => $Heading,
            'startingText' => $StartingText,
            'imageURL' => $medias->getUrl(),
            'page' => $page
        ]);
    }
```

Now we have to add a extra argument into our service in the `service.xml`: `<argument type="service" id="media.repository" />`

Your service shuld now look somewhat like this:
```
        <service id="M28\TestPlugin\Storefront\Controller\JobController" public="true">
            <argument type="service" id="Shopware\Storefront\Page\GenericPageLoader" />
            <argument type="service" id="Shopware\Core\System\SystemConfig\SystemConfigService" />
            <argument type="service" id="media.repository" />

            <call method="setContainer">
                <argument type="service" id="service_container" />
            </call>
            <call method="setContainer">
                <argument type="service" id="service_container" />
            </call>
            <call method="setTwig">
                <argument type="service" id="twig" />
            </call>

        </service>
```
