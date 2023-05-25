To add JS Code to your code, there are 2 Ways:

1: echo a script inside a script tag with php

```php
echo '<script>';
echo 'console.log("This Code will be Executed, but at what cost?")';
echo '</script>';
```

I dont Recommend using that way but who am I to tell you what to do

2: Add a JS File:

Create a File in the Following File Structure. If you are missing folders, create them:
```
[Plugin Name]
|-src
  |-Resources
    |-app
      |-storefront
        |-src (yes, src in Resources in src)
          |-main.js
          |- [Plugin Name] (has to be the exact same as the Folder in the first Place)
            |-[filename].js (the file name does not really matter. I will use `jobs-post.js`)
```

inside of your own js file (NOT the main.js) add the following lines:
```js
import Plugin from "src/plugin-system/plugin.class";

export default class [className] extends Plugin{
}
```
Make shure to use a fitting Class Name

Now inside of the Class you can just write your JS code. for example:
```js
import Plugin from "src/plugin-system/plugin.class";

export default class JobPost extends Plugin{
     init(){
        window.addEventListener("scroll", this.onScroll.bind(this))
     }

     onScroll(){
        if((window.innerHeight + window.pageYOffset) >= document.body.offsetHeight){
            alert("Seems like there is nothing more to see!")
        }
     }
}
```

The init function gets called whenever the page is loaded. call your functions from here and add your eventListeners here.

Then go inside of your `main.js` and import your Plugin:
```js
import JobPost from './[plugiNname]/[fileName]';
```

then you have to register the Plugin via Shopwares Plugin Manager:
```js
const PluginManager = window.PluginManager;
PluginManager.register('[Name]', [ClassName]);
```

>__Warning__
>
> To avoid conflics, never use the same ClassName in two js files.

To see your Changes you have
# to build your Project.
there are two ways:

1: Development:
Run the command `./psh.phar storefront:build`

2: Production:
Run the command: `./bin/build-storefront.sh`

When you want to ship your Plugin, you may want to run the Production command. It will take a bit longer than the Developement build command.

run the Command in your Docker Terminal. If you dont know how, you can larn [here](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/tree/main/sideguids)

And now you shuld see the Changes you made. If not, make shure to [Clear the Cash](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/sideguids/clearCash.md)

If you still dont see any Changes, you may have cashed something on your PC. In Chrome: Hold Shift and press the Refresh Button. Other Browsers may have different Ways to clear the Cash. Google can help you.

>__note__
>
> The Code will be Executed in every Page of the Store
