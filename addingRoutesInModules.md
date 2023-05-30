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
