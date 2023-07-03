 first of all you have to create a `config.xml` file in the following Location:
 ```
 [Plugin Name]
 |-src
  |-Resources
    |-config
      |config.xml
 ```
 
The config.xml file is structured in cards. every config.xml must contain at least one card, every card must contain one title and atleast one input-field. so the bare minimum config.xml looks like this:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="https://raw.githubusercontent.com/shopware/platform/trunk/src/Core/System/SystemConfig/Schema/config.xsd">
    <card>
        <title>Minimal configuration</title>
        <input-field>
            <name>example</name>
        </input-field>
    </card>
</config>
```

There are some types for the input field, a complete list of all types is avaiable [here in the Shopware Docs](https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/add-plugin-configuration#the-different-types-of-input-field)

to add a input-field with the type int, you just have to write `<input-field type="int">` 

>__Note__
>
> The type must always be a String. `type=int` is not a valid type but `type="int"` is

For accessibility you can add some attributes to your input-field:

- Label:
-   The Label will be displayed above your field. Here you can tell the User what he is supposed to enter.
- placeholder:
-   If nothing is enterd in the Field, this text will be displayed. 
- helpText:
-   idk ig it helps?
- defaultValue:
-   This will be the Value of the field
- disabled:
-   if used like `<disabled>true</disabled>` the field will be disabled. every other usecase doesnt make sense with disabled, sind `<disabled>false</disabled>` will just be the default behaviour
- copyable:
-   if set to true, a Button to copy the contents will be added. Used like this: `<copyable>true</copyable>` Note that even if set to false the user can copy the text via normal copy


Advanced input fields:

If you want the user to e.g. upload a Image, you have to use Advanced input fields. To learn more about them, go to the [Shopware Docs](https://developer.shopware.com/docs/guides/plugins/plugins/plugin-fundamentals/add-plugin-configuration#advanced-custom-input-fields)

The Most important ones are:
*The Media selection*:
```xml
<component name="sw-media-field">
    <name>pluginMedia</name>
    <label>Upload media or choose one from the media manager</label>
</component>
```

> __Note__
>
> getting the Image from a Media field is not that staight forward. If you wish to get information about a Image, use [This guide](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/sideguids/showImage.md)

*The Text Editor*:
```xml
<component name="sw-text-editor">
    <name>textEditor</name>
    <label>Write some nice text with WYSIWYG editor</label>
</component>
```

>__Warning__
>
> a input field *must* **ALWAYS** start with the name attribute. If not, it will cause an Error.


>__Note__
> 
> For obvious reasons, choose a different Name for every field.


Next Step will be to [Use the Configuration](https://github.com/kollhdxdlp/ShopwarePluginQuickstartGuide/blob/main/UseConfiguration.md)



