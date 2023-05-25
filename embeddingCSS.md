To use CSS in your custom Route, you have two options:

1: Directly use the CSS as style inside of the HTML Elements

Example: 
`<h1 style="text-align: center>Welcome!</h1>`

>__Note__
>
> This works, but I would sugest that you only use that for Smal projects, just like you would do in normal Web Developement

2: Use a CSS file

Shopware uses [SCSS](https://stackoverflow.com/questions/46400443/what-is-the-difference-between-css-and-scss#:~:text=SCSS%20is%20a%20special%20type,writing%20CSS%20easier%20and%20faster.). You may want to learn SCSS or you just handle your SCSS files like normal CSS ones.

Before proceed, please make shure you already created a [route]() or something similar.

Just like in normal Web developement, add a class to your HTML object so you can change the Stile later on. This looks somewhat like this:
`<h1 class="Eric-Demo-Heading">Welcome!</h1>`
>__Note__
>
> To avoid conflicts always use specific class names including your prefix and plugin name. Note that you can not use spaces in the class name

Now lets create a scss file. Make shure your file structure looks exactly like mine.
If you dont have any of the folders, create them
```
[Plugin Name]
|-src
  |-Resources
    |-app
      |-storefront
        |-src (yes, src in Resources in src)
          |-scss
            |-base.scss
```

> __Warning__
>
> As far as I know, you can not name it something other than `base.scss`

Now add the CSS you want to add. My Code will just be something simple:
```css
.Eric-Demo-Heading{
  text-align: center;
  color: red;
}
```

Every time you make Changes to your SCSS you will have to build your project. in that process, the scss gets translated to a css file. 

# To build your Project
there are two ways:

1: Development:
Run the command `./psh.phar storefront:build`

2: Production:
Run the command: `./bin/build-storefront.sh`

When you want to ship your Plugin, you may want to run the Production command. It will take a bit longer than the Developement build command.

run the Command in your Docker Terminal. If you dont know how, you can larn [here](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/tree/main/sideguids)

And now you shuld see the Changes you made. If not, make shure to [Clear the Cash](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/sideguids/clearCash.md)

If you still dont see any Changes, you may have cashed the CSS on your PC. In Chrome: Hold Shift and press the Refresh Button. Other Browsers may have different Ways to clear the Cash. Google can help you.

