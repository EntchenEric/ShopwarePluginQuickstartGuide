### Note: Make shure you have read the [previous tutorial](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/CustomModules.md) before proceeding.

At first, lets create some folders. Inside of your Module, create a Folder inside of your module called "page". In here all of your routes will be specified. In here, create another Folder called [prefix]-[module]-[route]. I will call the Folder eric-jobs-list.

In here create two files: a `index.js` and a `[prefix]-[module]-[route].html.twig` file. 

Your folder structure now looks like this:
```
... (for the full path please refer to https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/CustomModules.md)
|-administration
  |-src
    |-main.js
    |-module 
      |-[modulename]
        |-index.js
        |-snippet
          |-de-DE.json
          |-en-GB.json
        |-page
          |-[Routename]
            |-index.js
            |-[routename].html.twig
         
```

Now lets start creating a Demo Route for test purposes. Go into your `html.twig` file and paste the following code:
```
{% block m28_jobs_list %}
    <sw-page>

        <template slot="smart-bar-actions">
        </template>

        <template #content>
            {% block [prefix]_[module]_[route]_content %}
                <sw-entity-listing
                    :showSelection="false"
                    :columns="columns"
                >
                </sw-entity-listing>
            {% endblock %}
        </template>
    </sw-page>
{% endblock %}
```

Next Step; `index.js`:
first of all, lets Import the template that we just created via `import template from './[name of your twig file].html.twig';`
then import the Component from Shopware: `const { Component } = Shopware;`
And now we can start

first of all, we register the componement: 
```
Component.register('m28-jobs-list', {
});
```

## All of your Code will be inside of this function.

Then we just have to input enter the following:
```
    template,

    metaInfo() {
        return {
            title: this.$createTitle()
        };
    },

    inject: [
        'repositoryFactory'
    ],

    data() {
        return {
            repository: null,
        };
    },

    created() {
    },

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
                }, 
                {
                    property: 'description',
                    dataIndex: 'description',
                    label: this.$t('table.jobDescription'),
                    inlineEdit: 'string',
                    allowResize: true,
                },
            ];
        }
    },
```

Your Complete Code will look like this:
```
import template from './[NAME OF YOUR TWIG].html.twig';

const { Component } = Shopware;

Component.register('[NAME OF YOUR ROUTE]', {
    template,

    metaInfo() {
        return {
            title: this.$createTitle()
        };
    },

    inject: [
        'repositoryFactory'
    ],

    data() {
        return {
            repository: null,
        };
    },

    created() {
    },

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

Now we have a Table and a Button you can click on. Lets create a new Route where you can enter Information that we later will display in the table. 

> __Note__
> 
> Because this part of the Tutorial needs a more direct example to be understandable, I will only talk about my Jobs page. You can, ofcourse change all the data to fit your needs

First of all, lets create some new files. Inside of your `page` folder, create the following folders:
`[prefix]-jobs-create` and a `[prefix]-jobs-detail` folder.
In here, create two index.js files. 

Lets focus on the jobs-create index first:
Here you just have to write 6 lines of code:
```
const { Component, Context } = Shopware;

Component.extend('m28-jobs-create', 'm28-jobs-detail', {
    methods: {
    }
});
```

This code just means that we use the create page as out detail page. This has the advantage, that we can use the detail page somewhere else and don't have to have the same code twice in our code.

Then inside of the jobs-detail index.js we have to write some more code:

- first some imports: `const { Component, Data, Mixin } = Shopware;`
- Then lets register the componement: `Component.register('[prefix]-jobs-detail', {})`
- now inside of your componement.register, add the following:
```
    template,

    metaInfo() {
        return {
            title: this.$createTitle()
        };
    },

    inject: [
        'repositoryFactory'
    ],

    data() {
        return {
            repository: null,
        };
    },

    created() {
    },

    computed: {
        columns() {
            return [
                {
                    property: 'name',
                    dataIndex: 'name',
                    label: this.$t('table.jobTitle'),
                    inlineEdit: 'string',
                    allowResize: true,
                    primary: true,
                    routerLink: 'm28.jobs.detail',
                }, 
                {
                    property: 'cartValueGross',
                    dataIndex: 'cartValueGross',
                    label: this.$t('table.jobDescription'),
                    inlineEdit: 'number',
                    allowResize: true,
                },
            ];
        }
    },
```

As you may noticed we dont have a template yet. lets first import it and then create it:
Add this line on top of your code:
`import template from './[prefix]-jobs-detail.html.twig';` 
Now inside of your jobs-detail folder, create a [prefix]-jobs-detail.html.twig file and paste this:
```
{% block m28_jobs_detail %}
    <sw-page class="m28-jobs-detail">
        <template slot="smart-bar-actions">
            <sw-button variant="primary" :routerLink="{ name: 'm28.jobs.create' }">
                {{ "'Submit'" }}
            </sw-button>
        </template>
        <template #content>
            <sw-card>
                <sw-text-field
                    :label="'dasas dasd'"
                    :placeholder="'adsa sd'"
                    :helpText="'dasadsadas'"
                    required
                ></sw-text-field>
            </sw-card>
        </template>
    </sw-page>
{% endblock %}
```

> __Note__
> 
> Normaly, the fields are `sw-{field}` where Field is the render you want from [here](https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/add-plugin-configuration#the-different-types-of-input-field)

If you want to, you can also create a `[prefix]-jobs-detail.scss` file to add Styling.

Next step: adding the routes:
Go to your index.js of your module and inside of your routes add the following:
```
        create: {
            component: '[prefix]-jobs-create',
            path: 'create',
            meta: {
                parentPath: '[prefix].jobs.list'
            }
        },
        detail: {
            component: '[prefix]-jobs-detail',
            path: 'detail',
            meta: {
                parentPath: '[prefix].jobs.list'
            }
        },
```

> __Warning__
>
> Make shure you have the exact same in the component as you have in your index.js in the Component.register. Aspecially look out for mistakes like jobs-create and create-jobs

Now we just have to buld out Plugin and everything shuld work.

Build you Plugin using the Command `bin/build-administration.sh`.
