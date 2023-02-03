# Gas-meter-sensor

With this project you are able to track your gas consumption without any expensive add-ons. 

What do you need?

**Hardware:**
* Aqara door sensor (Link: https://www.amazon.de/Aqara-MCCGQ11LM-Window-Sensor-Fensterssensor/dp/B07D37VDM3/ref=sr_1_3?__mk_de_DE=%C3%85M%C3%85%C5%BD%C3%95%C3%91&crid=3670LWGD33FOG&keywords=aqara+t%C3%BCr&qid=1675447904&s=diy&sprefix=aqara+t%C3%BCr%2Cdiy%2C83&sr=1-3=
* Reed contact (Link: https://www.amazon.de/gp/product/B082NJHSRH/ref=ppx_yo_dt_b_search_asin_image?ie=UTF8&psc=1) 
* If you want to build a case for it you need a 3D printer

**Software:**
* HomeAssistant
* NodeRed 
* FreeCAD
* Cura Slicer 

Let`s start with the concept. 
On a lot gasmeters there is a magnet on the last counter ring. With a reed-contact you can detect each rotation. 
Now you need to get this information to your HomeAssistant. Therefore you can take the door sensor, remove the reed contact and
use your own one. 
The door sensor now detects every rotation and you can count it with HomeAssistant. In my case i used NodeRed for it but you can use 
every other solution. 

![image](https://user-images.githubusercontent.com/55063915/216677814-df09f0c1-a04f-48c3-b0c7-12cc1d2febcb.png)

The most improtant thing is to find the right position. It took me some hours to find out at which position the reed-contact needs to be. 
Some milimeter left or right made the difference. 

After that I designed my case. 

Top: 

![image](https://user-images.githubusercontent.com/55063915/216678154-0c8ad5f0-156e-42da-8588-63fb4634d34a.png)

Bottom:

![image](https://user-images.githubusercontent.com/55063915/216678231-9295af57-cbd1-44b4-9bd0-8ded2dc72624.png)

Then you can solder your reed contact to your Aqara door sensor and assembly everything. 

Top with sensor:

![WhatsApp Bild 2023-02-03 um 19 07 25](https://user-images.githubusercontent.com/55063915/216678410-4f72a1f0-9b56-4d2c-b542-1e476af2b189.jpg)

Ready to use: 

![image](https://user-images.githubusercontent.com/55063915/216678477-2b979eb5-7de9-492b-8c5e-2343bc7146cb.png)

Now you need to connect the sensor with HA. 
Mark Watt provided a really good video for that. (Link: https://www.youtube.com/watch?v=_EZDYUfVnZY) 

If the Aqara door sensor is connected you can use NodeRed. (I`m not the best NodeRed user but for me it works stable and reliable)

![image](https://user-images.githubusercontent.com/55063915/216679284-1a3c48f9-f46a-4726-96bc-b2fd7079d217.png)

Gaszähler: This is your door sensor

Init sumcount: This is your start value of your gas meter

count: This is now your increment with each rotation. 

```
[{"id":"3f5d9f56.573d8","type":"tab","label":"Gasverbrauch","disabled":false,"info":""},{"id":"5c03880cb98e1c25","type":"server-state-changed","z":"3f5d9f56.573d8","name":"Gaszähler","server":"12a3a9ca.e1b6a6","version":4,"exposeToHomeAssistant":false,"haConfig":[{"property":"name","value":""},{"property":"icon","value":""}],"entityidfilter":"binary_sensor.openclose_26","entityidfiltertype":"exact","outputinitially":false,"state_type":"str","haltifstate":"off","halt_if_type":"str","halt_if_compare":"is","outputs":2,"output_only_on_state_change":true,"for":"0","forType":"num","forUnits":"minutes","ignorePrevStateNull":false,"ignorePrevStateUnknown":false,"ignorePrevStateUnavailable":false,"ignoreCurrentStateUnknown":false,"ignoreCurrentStateUnavailable":false,"outputProperties":[{"property":"data","propertyType":"msg","value":"","valueType":"eventData"},{"property":"topic","propertyType":"msg","value":"Gas","valueType":"str"},{"property":"payload","propertyType":"msg","value":"sumcount","valueType":"flow"}],"x":80,"y":120,"wires":[["f060299603fddc4b"],[]]},{"id":"fa16ce8a62357834","type":"inject","z":"3f5d9f56.573d8","name":"Init sumcount","props":[{"p":"payload"}],"repeat":"","crontab":"","once":true,"onceDelay":0.1,"topic":"","payload":"3154.25","payloadType":"num","x":200,"y":40,"wires":[["df5578e3bc1816a9"]]},{"id":"df5578e3bc1816a9","type":"change","z":"3f5d9f56.573d8","name":"","rules":[{"t":"set","p":"sumcount","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":540,"y":40,"wires":[["a76f92f2ea7eb1a6"]]},{"id":"f060299603fddc4b","type":"change","z":"3f5d9f56.573d8","name":"increment count","rules":[{"t":"set","p":"sumcount","pt":"flow","to":"\t$round($flowContext(\"sumcount\")+0.01,2)","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":380,"y":100,"wires":[["a9396de48e43e393","bbb7f3a5520ec8b4"]]},{"id":"a9396de48e43e393","type":"ha-sensor","z":"3f5d9f56.573d8","name":"Keller_Sumcount","entityConfig":"f8f40151bf63b27c","version":0,"state":"payload","stateType":"msg","attributes":[],"inputOverride":"allow","outputProperties":[],"x":670,"y":100,"wires":[[]]},{"id":"bbb7f3a5520ec8b4","type":"debug","z":"3f5d9f56.573d8","name":"debug 2","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":700,"y":160,"wires":[]},{"id":"a76f92f2ea7eb1a6","type":"debug","z":"3f5d9f56.573d8","name":"debug 3","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":760,"y":40,"wires":[]},{"id":"734929b0eb1df411","type":"inject","z":"3f5d9f56.573d8","name":"Init count","props":[{"p":"payload"}],"repeat":"","crontab":"00 00 * * *","once":true,"onceDelay":0.1,"topic":"","payload":"0.00","payloadType":"num","x":350,"y":200,"wires":[["f4bf24e588a96c21"]]},{"id":"f4bf24e588a96c21","type":"change","z":"3f5d9f56.573d8","name":"","rules":[{"t":"set","p":"count","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":520,"y":200,"wires":[[]]},{"id":"99b005e11eb3c9d1","type":"change","z":"3f5d9f56.573d8","name":"increment count","rules":[{"t":"set","p":"count","pt":"flow","to":"$round($flowContext(\"count\")+0.01,2)","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":360,"y":260,"wires":[["050e3cefa817a3bb"]]},{"id":"050e3cefa817a3bb","type":"ha-sensor","z":"3f5d9f56.573d8","name":"Keller_count","entityConfig":"c907cd15925317cc","version":0,"state":"payload","stateType":"msg","attributes":[],"inputOverride":"allow","outputProperties":[],"x":570,"y":260,"wires":[["f8b21302ffa1baa4"]]},{"id":"f8b21302ffa1baa4","type":"debug","z":"3f5d9f56.573d8","name":"debug 1","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":800,"y":260,"wires":[]},{"id":"e99f008bb9c64057","type":"server-state-changed","z":"3f5d9f56.573d8","name":"Gaszähler","server":"12a3a9ca.e1b6a6","version":4,"exposeToHomeAssistant":false,"haConfig":[{"property":"name","value":""},{"property":"icon","value":""}],"entityidfilter":"binary_sensor.openclose_26","entityidfiltertype":"exact","outputinitially":false,"state_type":"str","haltifstate":"off","halt_if_type":"str","halt_if_compare":"is","outputs":2,"output_only_on_state_change":true,"for":"0","forType":"num","forUnits":"minutes","ignorePrevStateNull":false,"ignorePrevStateUnknown":false,"ignorePrevStateUnavailable":false,"ignoreCurrentStateUnknown":false,"ignoreCurrentStateUnavailable":false,"outputProperties":[{"property":"data","propertyType":"msg","value":"","valueType":"eventData"},{"property":"topic","propertyType":"msg","value":"Gas","valueType":"str"},{"property":"payload","propertyType":"msg","value":"count","valueType":"flow"}],"x":80,"y":260,"wires":[["99b005e11eb3c9d1"],[]]},{"id":"12a3a9ca.e1b6a6","type":"server","name":"Home Assistant","addon":true},{"id":"f8f40151bf63b27c","type":"ha-entity-config","server":"12a3a9ca.e1b6a6","deviceConfig":"","name":"keller_sumcount","version":"6","entityType":"sensor","haConfig":[{"property":"name","value":"keller_sumcount"},{"property":"icon","value":""},{"property":"entity_category","value":""},{"property":"device_class","value":"volume"},{"property":"unit_of_measurement","value":"m³"},{"property":"state_class","value":"measurement"}],"resend":false,"debugEnabled":false},{"id":"c907cd15925317cc","type":"ha-entity-config","server":"12a3a9ca.e1b6a6","deviceConfig":"","name":"keller_gcount","version":"6","entityType":"sensor","haConfig":[{"property":"name","value":"keller_gcount"},{"property":"icon","value":""},{"property":"entity_category","value":""},{"property":"device_class","value":"volume"},{"property":"unit_of_measurement","value":"m³"},{"property":"state_class","value":"measurement"}],"resend":false,"debugEnabled":false}]
``` 
After that you can build some fancy dashbaord for your gas consumption: 

![image](https://user-images.githubusercontent.com/55063915/216679988-7500ac08-dccf-4f1f-83f6-fcd36f6d206d.png)

