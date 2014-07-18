# Helium Fusion

Helium Fusion is a powerful framework that lets you register and manage devices, construct flows, and integrate data with your internal and third party APIs. It's based on [Node-RED](http://nodered.org/), an open source tool from IBM built to wire devices, APIs, and online services together.


## Getting Started 

If you already have a Fusion account, you'll be able to [login](https://fusion.helium.io/) and get to work. If you're yet to register, please [create an account](https://fusion.helium.io/register) to get started. 	


## Registering and Managing Devices

After logging in you'll be taken to your dashboard. You're first presented with your list of Flows. If this is your first time using Fusion, you won't have any Flows, do your list of Flows will be empty. It will look like this:


![No Flow](/img/blank-dashboard.png)

To create a new flow, you would start with that input box in the upper right corner. But first we need to register your device. If you click on the [Devices](https://fusion.helium.io/devices) link in the top left, you'll be taken to this screen:

![No Device](/img/fusion-no-devices.png)

If you're part of the Beta, you'll be using an [Atom](/shields-and-modules/atom-beta-shield/) to connect to Helium. Each Helium module is identified by a globally unique serial number, 12-16 hexadecimal characters in length. To add your device to the network, enter this serial number (which should be labelled clearly on your Atom), along with a New Device Name of your choosing, and click "Register Device" in the upper right. 

As an example, if we used `00212eabae0039e1` for the Serial Number and `My First Device` for your device name, heres's what you would see:

![First Device](https://www.helium.co/docs/img/first-device.png)

Make sure you use your real information when adding your device. **After you've added your device to Fusion and plugged it into a power source, you're connected**. You'll repeat this simple process for all your future devices on the network. 



## Sending Data and Creating Flows 


We can now create a flow that takes data from your Atom and sends it to the network. Flows are pieces of logic that enable you to transform, bend, and route data. We're going to create a simple flow, but as you'll quickly realize, they are a powerful concept that creates unbounded potential when working with device and sensor data. 

Your Atom is programmable via our Arduino Library. Once you've [downloaded and installed the package](/libraries/arduino/#download-and-installation), you'll need to write a Sketch to generate data. We've included some [example programs](/libraries/arduino/#example-programs) that you can use to get started. The Simple Looping Message program is a quick and easy way to get up and running. Take a few minutes to get that loaded up onto your Atom. 

Head back to [your Dashboard](https://fusion.helium.io/flows) so we can create your first flow. Once there, in the upper right hand corner, enter a name for your new flow and select "Create Flow." Here's what you'll see:

![First Flow](https://www.helium.co/docs/img/simple-flow.png)


Click "Edit" and you'll be taken to a workspace to create your flow. You'll want to do a lot of exploring here, but for the moment let's build a flow to get data from the simple looping message program on your Atom to a Debug node so you can see it in action. 

Drag a Helium node from the Inputs column on the left onto the screen. It should be the first listed. Double click the node, enter a name for the Helium node, and click "OK". You'll notice the name you created for the device when you first registered it is already present in the drop down. 

![Simple Atom](https://www.helium.co/docs/img/simple-atom.png)


When that's done, scroll down to the Outputs panel on the left and drag a "Debug" node onto the screen. Double click on this, and enter the following information:

![Debug Edit](https://www.helium.co/docs/img/debug-edit.png)


Now, wire the Simple Looping Atom node to the Simple Looping Debug: 

![Wired Flow](https://www.helium.co/docs/img/flow-wired.png)

The Simple Looping Message sample sketch that we're using for an example program will send the string "Sent from my Helium Device" every five seconds. So, this flow will produce that message every five seconds in the Debug tab on the far right of the screen. In order to deploy this, simply click "Save Flow" and toggle the state to "Active" in the upper right and you should see messages like this:

![Debug Flow](https://www.helium.co/docs/img/running-debug.png)


This is just the beginning of what you can do with your device data, Helium Fusion, and our powerful routing network. You can create inputs, outputs, functions, and many other types of flow to accomplish just about any task needed. 




