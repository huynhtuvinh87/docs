# Welcome to Helium Red

These are the Alpha Helium Red docs. This will be their home until the consolidated Helium Docs site is launched. Here you will find basic documentation on how to use the [Helium Red](https://red.helium.io) to build applications. 


* [What is Helium Red?](#what-is-helium-red)
* [Inputs and Adding Your First Helium Device](#inputs-and-adding-your-first-helium-device) 
* [Sending Data and Using the Debug Console](#sending-data-and-using-the-debug-console)
* [Sending Helium Device Data to Twitter](#sending-helium-device-data-to-twitter)


## What is Helium Red?

Helium Red is a powerful framework that lets you register and manage devices, construct flows, and integrate data with your internal and third party APIs. It's based on [Node-RED](http://nodered.org/), an open source tool from IBM built to make it easy to wire devices, APIs, and online services together. 

Helium Red is optimized for Helium's device and connectivity platform. As such, you should start with our documentation and tutorials. However, the [Node-RED documentation](http://nodered.org/docs/) should also be useful for topics not covered here. 

### Inputs and Adding Your First Helium Device 

Helium Devices are managed in Helium Red as "inputs." Once you're logged into [the UI](http://red.helium.io/red/), you'll be presented with a workspace. The left, vertical pane is where inputs are listed.  

To add your first device, drag the "Helium-in" node into the center of the screen. It will look like this when you're done:

![node_on_screen](https://raw.githubusercontent.com/helium/red-docs/master/images/node_on_screen.png)

Helium Devices are identified by their pre-assigned MAC addresses. To add yours to the platform, double click on the Helium-in node you just dragged onto the screen and click the "Add new helium device..." option from the Device drop down and select the pencil icon to the right of the right of the drop down. This will prompt you to enter a MAC address and Name for your device. 

![node_name_add](https://raw.githubusercontent.com/helium/red-docs/master/images/node_name_add.png)

Enter your MAC Address and a name for your device and click "Add" to save the node. Congratulations. You've now got a node on the network and are ready to start sending data. 

## Sending Data and Using the Debug Console

Once you've added your first node, you can start sending data to the platform. (This assumes that you're running or are within range of a Helium Bridge. If you've managed to get your hands on a Helium Device but don't have access to a Bridge, stop reading this and email **mark@helium.co**.) 

Your Helium Arduino Shield is programmable via our [Arduino Library](http://arduino.docs.helium.co/). Once you've downloaded and installed the package, you'll need to write a Sketch to generate data. (We've included some sample programs that you can use to get started. The [Simple Looping Message](http://arduino.docs.helium.co/#simple-looping-message) is a quick and easy way to get up and running.)

The easiest way to see data flowing into Helium Red is to take your messages and pipe them into a debug node. On the left side of your screen, drag a "debug" node from the outputs panel. After you've done that, click and drag a wire from the right side of the "helium in" node and connect it to the left side of the debug node. When completed, it'll look like this:

![helium_in_to_debug](https://raw.githubusercontent.com/helium/red-docs/master/images/helium_in_to_debug.png)

Double click the debug node to configure the options. For the purposes of this tutorial, use "Payload Only" for the **Output** drop down, "debug tab" for the **to** drop down, and "My First Debug" for the **Name** field. Click "Ok" to save the configuration. 

The last step is to deploy the flow you've just created. To do this, click the large, red "Deploy" button located in the top right corner of the UI. Once you've done that, you'll soon start to see glorious messages flood your debug tab, located on the far right of the screen. For example, if you're using our [Simple Looping Message](http://arduino.docs.helium.co/#simple-looping-message) program, here's what you'll see:

![debug_gif](https://raw.githubusercontent.com/helium/red-docs/master/images/debug.gif)

That's it. You're now passing messages from your Helium Device onto the Network and visualizing them in Helium Red. Once you have a platform for data capture and routing, the possibilities are endless. Let's take a look at one example flow you can create from you device data and the prebuilt Twitter flow. 

## Sending Helium Device Data to Twitter 

One of the more powerful aspects of Helium Red is that is automatically enables you to make use of the library of prebuilt nodes the Node-RED community has created and published. You can get a full list of them on the [Node-RED site](http://flows.nodered.org/). In the left panel of your Helium Red UI you'll notice that some are already present. Scroll down to the "social" section and drag the Twitter out node. It'll look like this:

![twitter_out_node](https://raw.githubusercontent.com/helium/red-docs/master/images/twitter_out_node.png)

Drag this out and connect it to your Helium-in node:

![helium_in_to_twitter_out_node](https://raw.githubusercontent.com/helium/red-docs/master/images/helium_in_to_twitter_out_node.png)

We can now configure the Helium Device to tweet the message payload when it arrives. Double click the Twitter out node and add your credentials (or those of your favorite fake account). After that's configured, click the Deploy button again in the upper right hand corner. Shortly thereafter, navigate to the Twitter account you're using and you'll see magic similar to [this](https://twitter.com/pharkmillups/status/471467075399974912).

This is just the beginning of what you can do with your device data, Helium Red, and our powerful routing network. You can create inputs, outputs, functions, and many other types of flow to accomplish just about any task needed. 



 