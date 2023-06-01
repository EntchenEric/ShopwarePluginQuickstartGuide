Other than writing to the Database, reading Data is fairly simple. For a really simple guide, please head [here](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/sideguids/showImage.md)

First of all, we have to import some things:
```
const { Component, Data, Mixin } = Shopware; //this may not be needed for the reading, but for the example
const { Criteria } = Data;
```

then we can just directly read data from the table via:
```
this.repository = this.repositoryFactory.create('[the name of the Table you want to read]');

        var criteria = new Criteria(1, 25); //you may want to replace this

        this.repository
            .search(criteria, Shopware.Context.api)
            .then(result => {
                this.jobPosts = result; //this is for dealing with the data. Depending on you usecase you can also deal with the data here
            });
```

In a real world example this often gets integraded into a componement. It may look somewhat like this:
```
import template from './[prefix]-jobs-list.html.twig';

const { Component, Data, Mixin } = Shopware;
const { Criteria } = Data;

Component.register('[prefix]-jobs-list', {
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
            jobPosts: null
        };
    },

    created() {
        this.repository = this.repositoryFactory.create('[Table name]');

        var criteria = new Criteria(1, 25);

        this.repository
            .search(criteria, Shopware.Context.api)
            .then(result => {
                this.jobPosts = result;
            });
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
