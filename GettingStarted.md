# Step 1: Create a Docker container
- Somewhere on your Computer, create a Folder. I will call it developement
- Inside of this Folder, create two Folders, I will call them `shopware-apps` and `shopware-plugins`

## Your File structure looks now like this:
```
developement
|-shopware-apps
|-shopware-plugins
```

>__Note__
>
>The folders shuld not contain any contents to avoid problems with Shopware later on

Modify this Command accordingly to your folder locations:
```
docker run -d -p 80:80 -p 22:22 -p 3306:3306 -v {Pluginfolder location}:/var/www/html/custom/plugins -v {Appfolder location}:/var/www/html/custom/apps --env MYSQL_PWD=root dockware/dev:6.5.0.0
```

*Example*
the Path of your plugin folder is `C:\Users\User\developement\shopware-plugins` and the path of your app folder is `C:\Users\User\developement\shopware-apps` then your command shuld look like this:
`docker run -d -p 80:80 -p 22:22 -p 3306:3306 -v C:/Users/User/developement/shopware-plugins:/var/www/html/custom/plugins -v C:/Users/User/developement/shopware-apps:/var/www/html/custom/apps --env MYSQL_PWD=root dockware/dev:6.5.0.0`

## **IMPORTANT**
- I had problems with the developement folder being placed in the root of my drive e.g. `C:\developement` if you have problems too try to move the folder inside of a User
- Note that you do NOT use backslashes (\\) like the path you get when you copy it from Windows. You have to replace them with forward slashes (/)
- If nothing happens or you get a error, make shure Docker Desktop is running

Now open a terminal and paste in the command. You shuld now get a random string e.g. `f3f1afdc6a2c46e6000d96d6bdb5a29487f110bbfd4c154628e2c80a77cc46e4`

If you go to your Docker Containers now you shuld see a new Container that shuld already be started automaticly. If not, start it. Now you can go on localhost and you shuld see a DemoStore.
## Errors that might occure
- `The Port is already in use` you already have something running on port 80. Stop that or change the Port in the command
- `this site could not be reached` you may entered a port. just go to localhost or localhost:80 do NOT go to localhost:3000 or something simmilar
- `this site could not be reached` if the previous solution didnt helped, try to go to http://localhost this may help


# Step 2: Creating a Skeleton Plugin
- Go to Docker Desktop and make shure the container you created in step 1 is active
- Click on the container name
- Go to the `Terminal` Tab
- enter `su`
- enter your password `If you dont know your password or you didnt change it, its probably root`
> __Note__
>
> Your password does not show up when your typing. Just type blind.
- enter the command `bin/console`
- ow a list with all avaiable commands shuld show up. If you get a Error you shuld debug it.


enter the command `bin/console plugin:create [name]` 
## About the name
The name MUST be in UpperCamelCase and **have to** contain a prefix e.g. your name if you want to publish your plugin. 
Im not shure about special characters and non english letters, but I would prevent using them to maximize compatibility

Valid Names:
- EricDemoPlugin
- EricTryesToNameAPlugin

Invalid Names:
- DemoPlugin
- ericdemoplugin
- eric-demo-plugin
- ericDemoPlugin
- 16EricDemoPlugin

>__Note__
>
>you dont get any feedback in the console. If nothing happens, the creation was successfull.

# Step3: Edit the composer.json file

Open the newly created folder with a Code editor like Visual Studio Code and open the `composer.json` file. Here you will have to make some Changes

- the name value has to be Changed to: `[prefix]/[pluginName]` written in lowercase and written in [kebab-case](https://www.pluralsight.com/blog/software-development/programming-naming-conventions-explained#:~:text=kebab%2Dcase%20is%20a%20variable,%2C%20kebabs%2Deaten%2Dtoday.)
-   Example: eric/demo-plugin
- the psr-4 key (the name you entered) has to be change in `[prefix]\\[pluginName]\\` written in [UpperCamelCase](https://www.techtarget.com/whatis/definition/lowerCamelCase#:~:text=UpperCamelCase%20vs.,method%2C%20variable%20and%20parameter%20names.)
-   Example: Eric\\DemoPlugin\\
- the value of shopware-plugin-class (now [pluginname]\\[pluginname]) has to be changed in `[prefix]\\[pluginName]\\[prefix][pluginName]` written in UpperCamelCase
-   Example: Eric\\DemoPlugin\\EricDemoPlugin
- if you want to you can now enter a Description to your plugin. you probably also want to change the label. If you dont know german, just enter the same label as you entered in the english label. Better not delete it, idk if that could cause problems.
- if you want to you can also enter a version of your plugin. To do so, you have to add `"version": "0.0.1",` after the description

Example of the complete `composer.json` file:
```json
{
  "name": "eric/demo-plugin",
  "description": "This plugin is just for Demo purposes",
  "version": "0.0.1",
  "type": "shopware-platform-plugin",
  "license": "MIT",
  "autoload": {
    "psr-4": {
      "Eric\\DemoPlugin\\": "src/"
    }
  },
  "extra": {
    "shopware-plugin-class": "Eric\\DemoPlugin\\EricDemoPlugin",
    "label": {
      "de-DE": "Nur ein Demo Plugin",
      "en-GB": "Just a Demo Plugin"
    }
  }
}
```


# Step 4: Edit the [name].php file

Inside of the `src` Folder you shuld have a php file named after the name you entered in step 2. Open it
Now change line 3 (`namespace [name];`) into `namespace [prefix]\[pluginName]` written in UpperCamelCase
Example: 
`namespace Eric\DemoPlugin;`

# Step 5: test the Plugin

Go back into the Docker Terminal and enter the command `bin/console plugin:refresh`
If you did everything right, you shuld get a response looking somewhat like that:
```
---------------- -------------------------------------------- --------- ----------------- -------- ----------- -------- ------------- 
  Plugin           Label                                        Version   Upgrade version   Author   Installed   Active   Upgradeable  
 ---------------- -------------------------------------------- --------- ----------------- -------- ----------- -------- ------------- 
  EricDemoPlugin   Just a Demo Plugin                           0.0.1                                No          No       No           
 ---------------- -------------------------------------------- --------- ----------------- -------- ----------- -------- -------------
```
If not, try to debug.

Now go to your [Admin Panel](http://localhost:80/admin) at localhost/admin and enter your credentials.
If you dont know what your credentials are or you never set some, you probably want to enter `admin` as the username and `shopware` ad the password 

Here on the left hand side, go to extensions -> My extensions 
Here you can Install your own Plugin by clicking on install and activate it after its installed.

Now you have your own Plugin in the Store!
