To create a Module, you have to create some folders.

Create a index.js file at /src/Resources/app/administration/src/module/[ModuleName]/index.js

here you paste in the following code:

```
import deDE from './snippet/de-DE';
import enGB from './snippet/en-GB';

Shopware.Module.register('[prefix]-[moduleName]', {
    // configuration here
	type: 'plugin',
    name: 'DEMO',
    title: 'general.title',
    description: 'DEMO',
    color: '#ff3d58',
    icon: 'default-shopping-paper-bag-product',

	snippets: {
        'de-DE': deDE,
        'en-GB': enGB
    },

	routes: {
        list: {

        },
    },

	navigation: [{
		id: '[Prefix].[PluginName]',
		label: 'general.label',
		color: '#ff3d58',
		path: '[prefix].[pluginName].[something]', //The path doesnt really matter
		icon: 'default-device-server',
    parent: 'sw-catalogue',
		position: 0
	}]
});
```

> __Note__
> 
> I have no Idea why, but you have to include the route js object at leaste like I have implemented it. You can add more contents, but you can not remove the list js object.

Then we have to create a folder called `snippets` inside of out Module. Here, create two files: `de-DE.json` and `en-GB.json`. Here you can add your Translations. The `en-GB.json` Content for the above example would look like this:
```json
{
  "general": {
    "title": "DEMO TITLE",
    "label": "Klick me!"
  }
}
```

If you dont want any translations, just copy and paste the contents form the english translations into the german ones. 

Now we have to create a main.js file at `/src/Resources/app/administration/src/main.js`. Here just add one line:
```js
import './module/[ModuleName]';
``` 

And thats it! Now you have created a Button in the Admin panel under catalogue that you can klick.

> __Note__
>
> According to the Shopware Docs, they dont accept Plugins that create top level buttons in the Admin pannel. But somewhere else, they sad that you cant create them on top level. So ig dont create them on TopLevel

Your Folder Structure now looks somewhat like this:
```
[PluginName]
|-src
  |-Resources
    |-app
      |-administration
        |-src
          |-main.js
          |-module
            |-[ModuleName]
              |-index.js
              |-snippets
                |-de-DE.json
                |-en-GB.json
```
