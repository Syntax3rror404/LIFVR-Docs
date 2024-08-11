# Lumina Interaction Framework VR (LIFVR) Documentation

## FAQ

### 1. I can't find the plugin content in the content browser. 

Make sure you have enabled the plugin in your project. After enabling it a restart of the engine is required. Then open the **settings** of the content browser (in the content browser window in the right upper corner). There you need to check ```content -> Show Plugin Content```. Now You should see ```Plugins/LIFVR Content``` in the folder structure on the left side of the content browser.

Here you can find the entire BP available content, as well as the base BP classes to start creating your project logic.

### 2. I can't find the C++ classes of the framework.

The C++ classes can be found in the content browser under ```Plugins/LIFVR C++ Classes```. Analougously to 1.) make sure the plugin is enabled in the plugins section of your project and ```Show Plugin Content``` is enabled in the content browser settings. Furthermore you need to enable ```Show C++ Classes``` in the content browser settings.

Here you can find the basic classes of LIFVR. For Blueprint only users it's recomended to create child classes from the BP basic classes of the framework, instead of the C++ classes. This is the most easiest way to use and start scripting you're own logic. (So for example instead of creating a BP child class from the C++ class HexaCharacter use the BP_HexaCharacter in ```Plugins/LIFVR Content/Blueprints/Core/Characters/BP_HexaCharacter```).



Coming soon: A collection of answers to the most common questions our users have.

If you have any questions that you would like to see answered here, please let us know! 

