Storing files in the Database isnt that straight forward. Firt of all as always, you have to create a Folder. In your src folder, create a Folder named `Core`. In here create a Folder called `Content`. In here, we have to create three php files:

> __Note__
>
> I will callcreate a Job Post Database. If you want to create a other adabase, you shuld change the names accordingly.

`JObPostCollection.php`:
```
<?php declare(strict_types=1);


namespace [prefix]\[pluginName]\Core\Content;

use Shopware\Core\Framework\DataAbstractionLayer\EntityCollection;
use [prefix]\[pluginName]\Core\Content\JobPostEntity;

/**
 * @method void add(JobPostEntity $entity)
 * @method void set(string $key, JobPostEntity $entity)
 * @method JobPostEntity[] getIterator()
 * @method JobPostEntity[] getElements()
 * @method JobPostEntity|null get(string $key)
 * @method JobPostEntity|null first()
 * @method JobPostEntity|null last()
 */
class JobPostCollection extends EntityCollection
{
    protected function getExpectedClass(): string
    {
        return JobPostEntity::class;
    }
}
```
you dont really want to change that many files in here.


`JobPostDefinition.php`
```
<?php

namespace [prefix]\[pluginName]\Core\Content;

use Shopware\Core\Framework\DataAbstractionLayer\EntityDefinition;
use Shopware\Core\Framework\DataAbstractionLayer\Field\BoolField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\CreatedAtField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\Flag\Required;
use Shopware\Core\Framework\DataAbstractionLayer\Field\StringField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\FloatField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\UpdatedAtField;
use Shopware\Core\Framework\DataAbstractionLayer\FieldCollection;
use Shopware\Core\Framework\DataAbstractionLayer\Field\IdField;
use Shopware\Core\Framework\DataAbstractionLayer\Field\Flag\PrimaryKey;
use Shopware\Core\Framework\DataAbstractionLayer\Field\OneToOneAssociationField;

use M28\TestPlugin\Core\Content\JobPostCollection;
use M28\TestPlugin\Core\Content\JobPostEntity;
use Shopware\Core\Content\Product\ProductDefinition;
use Shopware\Core\Content\Rule\RuleDefinition;
use Shopware\Core\Framework\DataAbstractionLayer\Field\FkField;

class JobPostDefinition extends EntityDefinition
{
    public const ENTITY_NAME = '[prefix]_jobs_job_posts'; //your entity name shuld start with your prefix to avoid conflicts.

    public function getEntityName(): string
    {
        return self::ENTITY_NAME;
    }

    public function getCollectionClass(): string
    {
        return JobPostCollection::class;
    }

    public function getEntityClass(): string
    {
        return JobPostEntity::class;
    }

    protected function defineFields(): FieldCollection
    {
        return new FieldCollection([
            (new IdField('id', 'id'))->setFlags(new PrimaryKey(), new Required()),
            (new StringField('title', 'title'))->addFlags(new Required()),
            (new BoolField('uploadToGoogle', 'uploadToGoogle'))),
            (new FloatField('googleSalary', 'googleSalary')),
            (new CreatedAtField()), //This is required
            (new UpdatedAtField()), //This is required
        ]);
    }
   
}
```
Here in the Field Collection you shuld add the Fields you want to have.

`FreeProductsEntity.php`:
```
<?php declare(strict_types=1);

namespace M28\TestPlugin\Core\Content;

use Shopware\Core\Framework\DataAbstractionLayer\Entity;
use Shopware\Core\Framework\DataAbstractionLayer\EntityIdTrait;
use Shopware\Core\Content\Product\ProductEntity;
use Shopware\Core\Content\Rule\RuleEntity;

class JobPostEntity extends Entity
{
    use EntityIdTrait;

    /**
     * @var string
     */
    protected $title;

    /**
     * @var string
     */
    protected $description;

    /**
     * @var string
     */
    protected $image;

    // Add getter and setter methods for the new fields

    public function getImage(): string
    {
        return $this->image;
    }

    public function setImage(string $image): void
    {
        $this->image = $image;
    }
    
    public function getCreationdate(): ?\DateTimeInterface
    {
        return $this->creationdate;
    }

    public function setCreationdate(?\DateTimeInterface $creationdate): void
    {
        $this->creationdate = $creationdate;
    }

    public function getExpiredate(): ?\DateTimeInterface
    {
        return $this->expiredate;
    }

    public function setExpiredate(?\DateTimeInterface $expiredate): void
    {
        $this->expiredate = $expiredate;
    }
}
```

I suggest creating the JobPostEntity.php with ChatGPT.

Then we have to create a `Migration` folder inside of our src folder. Make shure you spell it correct.

In here create a `Migration[timestamp].php` file. Replace the timestamp with the [current timestamp](https://www.unixtimestamp.com/). You do this to avoid conflicts.

Inside of your migration file, you now paste in this code and complete the SQL querry to fit your needs.:
```
<?php declare(strict_types=1);

namespace M28\TestPlugin\Migration;

use Doctrine\DBAL\Connection;
use Shopware\Core\Framework\Migration\MigrationStep;

class Migration202306011200 extends MigrationStep
{
    public function getCreationTimestamp(): int
    {
        return 202306011200;
    }


    public function update(Connection $connection): void
    {
        $connection->executeStatement('
            CREATE TABLE IF NOT EXISTS `m28_jobs_job_posts` (
                `id` BINARY(16) NOT NULL,
                `created_at` DATETIME(3) NULL,
                `updated_at` DATETIME(3) NULL,
                PRIMARY KEY (`id`)
            ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
        ');
    }

    public function updateDestructive(Connection $connection): void
    {
        // Implement this method if you have destructive changes for your plugin
    }
}

```

again, I suggest creating the SQL querry using ChatGPT because its easyer.

Next step: Go to your service.xml and add this Service:
```
        <service id="[prefix]\[pluginName]\Core\Content\JobPostDefinition"> <-- Make shure to change this
            <tag name="shopware.entity.definition" entity="[prefix]_jobs_job_posts" />
        </service>
```

> __Warning__
> 
> For now on, im assuming you have created a module. If you didnt, then you may follow this tutorial further and try to change the things you need. If not then refer to the shopware docs. I may add more ways of saving Data in the database later.

To create a Page where you can Input some data, you can Use this code inside of your twig:
```
			<sw-card>
				<sw-text-field v-model="jobposts.title" :label="$t('jobcreator.title.label')" :placeholder="$t('jobcreator.title.placeholder')" :helptext="$t('jobcreator.title.helpText')" required></sw-text-field>
				<sw-text-editor v-model="jobposts.description" :label="$t('jobcreator.description.label')" :placeholder="$t('jobcreator.description.placeholder')" :helptext="$t('jobcreator.description.helpText')" required></sw-text-editor>
				<sw-media-field v-model="jobposts.image" :label="$t('jobcreator.image.label')" :helptext="$t('jobcreator.image.helpText')"></sw-media-field>
				<sw-colorpicker v-model="jobposts.mainColor" :label="$t('jobcreator.maincolor.label')" :helptext="$t('jobcreator.maincolor.helpText')"></sw-colorpicker>
				<sw-colorpicker v-model="jobposts.secondaryColor" :label="$t('jobcreator.secondcolor.label')" :helptext="$t('jobcreator.secondcolor.helpText')"></sw-colorpicker>
			</sw-card>

			<sw-card>
				<sw-text-field :label="$t('jobcreator.google.information.label')" :placeholder="$t('jobcreator.google.information.placeholder')" :helptext="$t('jobcreator.google.information.helpText')" :value="$t('jobcreator.google.information.value')"></sw-text-field>
				<sw-switch-field v-model="jobposts.uploadToGoogle" :label="$t('jobcreator.google.upload.label')" :helptext="$t('jobcreator.google.upload.helpText')" size="big" nomargintop></sw-switch-field>
				<sw-text-field v-model="jobposts.googleSalary" :label="$t('jobcreator.google.salary.label')" :placeholder="$t('jobcreator.google.salary.placeholder')" :helptext="$t('jobcreator.google.salary.helpText')"></sw-text-field>
				<sw-text-field v-model="jobposts.googleAddress" :label="$t('jobcreator.google.address.label')" :placeholder="$t('jobcreator.google.address.placeholder')" :helptext="$t('jobcreator.google.address.helpText')"></sw-text-field>
				<sw-text-field v-model="jobposts.googlePostcode" :label="$t('jobcreator.google.postcode.label')" :placeholder="$t('jobcreator.google.postcode.placeholder')" :helptext="$t('jobcreator.google.postcode.helpText')"></sw-text-field>
				<sw-text-field v-model="jobposts.googleCountry" :label="$t('jobcreator.google.country.label')" :placeholder="$t('jobcreator.google.country.placeholder')" :helptext="$t('jobcreator.google.country.helpText')"></sw-text-field>
			</sw-card>
```

## Make shure that you paste this code inside of a Block inside of a template. Learn more [here](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/addingRoutesInModules.md)

> __Warning__
> 
> The v-model is important. Make shure you give it a fitting name. Also make shure to extend your snippets for translations.

Inside of the index.js of your componement, add this code:

```
import template from './[prefix]-jobs-detail.html.twig';

const { Component, Data, Mixin, Context } = Shopware;
const { Criteria } = Data;


Component.register('[prefix]-jobs-detail', {
    template,

    metaInfo() {
        if (this.jobposts && this.jobposts.title) {
            return {
                title: this.$createTitle()
            };
        }
    },

    inject: [
        'repositoryFactory'
    ],

    //computed not relevant for entities, but Userfeedback is always a good idea
    mixins: [
        Mixin.getByName('notification')
    ],

    data() {
        return {
            repositoryJobposts: null,
            //fill in here your information. Read more in the warning below
            jobposts: {
                title: '',
                description: '',
                image: '',
                mainColor: '',
                secondaryColor: '',
                uploadToGoogle: false,
                googleSalary: null,
                googleAddress: '',
                googlePostcode: '',
                googleCountry: '',
            },
            isLoading: false,
        };
    },


    created() {
        this.repositoryJobposts = this.repositoryFactory.create(''); //enter here your entity name name from the ..Definition.php file
        this.getjobposts();
    },

    methods: {
        getjobposts() {
            this.repositoryJobposts
                .get(this.$route.params.id, Context.api)
                .then(entity => {
                    this.jobposts = entity;
                    this.isLoading = false;
                });
        },



        onClickSave() {
            console.log(this)
            console.log("1")
            this.isLoading = true;
            console.log("2")
            console.log(this.repositoryJobposts)
            this.repositoryJobposts
                .save(this.jobposts, Context.api)
                .then(() => {
                    this.getjobposts();
                    this.isLoading = false;
                    this.processSuccessCreated = true;

                    if (!this.$route.params.id) {
                        this.$router.push({ name: 'm28.jobs.list' });
                        this.createNotificationSuccess({
                            title: this.$t('general.success'),
                            message: this.$t('general.createSuccess')
                        });
                    }
                })
                .catch(exception => {
                    this.isLoading = false;

                    console.log(exception)

                    this.createNotificationError({
                        title: this.$t('general.errorTitle'),
                        message: exception
                    });
                });
        },
    },
  
    //computed not relevant for this, you may change this
    computed: {
        columns() {
            return [
                {
                    property: 'title',
                    dataIndex: 'title',
                    label: this.$t('table.jobTitle'),
                    inlineEdit: 'string',
                    allowResize: true,
                    primary: true,
                    routerLink: 'm28.jobs.detail',
                },
                {
                    property: 'description',
                    dataIndex: 'description',
                    label: this.$t('table.jobDescription'),
                    inlineEdit: 'number',
                    allowResize: true,
                },
            ];
        }
    },
});
```

> __Warning__
> 
> Inside of the data() make shure to replace the jobposts with what you called your v-model. e.g. your v-model on the name Inputfield is hans.firstname then it has to look like this:

``` 
hans: {
  firstname: '',
}
```

And thats it. Now everything shuld be working. To check if everything works as expected, you can go to http://localhost/adminer.php and look into the database. The default username and password are `root`
