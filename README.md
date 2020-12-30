# ALPR, ANPR on Node-RED
Get high-accuracy, developer-friendly **automatic license plate recognition** ([ALPR](https://platerecognizer.com/?utm_source=github&amp;utm_medium=website)) or automatic number plate recognition ([ANPR](https://platerecognizer.com/?utm_source=github&amp;utm_medium=website)) on Node-RED!

Our machine-learning software:

- Works on **dark, low-res, blurry images** and tough angles, all vehicle types, etc.  See our full [ALPR results](https://platerecognizer.com/alpr-results/?utm_source=github&amp;utm_medium=website).
- Decodes **license plate** , vehicle type (e.g. SUV, van, pickup truck), [**vehicle make model**](https://platerecognizer.com/vehicle-make-model-recognition-with-color/?utm_source=github&amp;utm_medium=website) (e.g. Honda Accord), color, and orientation. Ignores bumper stickers, car signs, etc.
- Is optimized for all [50 USA States](https://platerecognizer.com/alpr-for-usa/?utm_source=github&amp;utm_medium=website), [India](https://platerecognizer.com/anpr-for-india?utm_source=github&amp;utm_medium=website), [Brazil](https://platerecognizer.com/anpr-for-brazil/?utm_source=github&amp;utm_medium=website) and [**90+ countries worldwide**](https://platerecognizer.com/countries/?utm_source=github&amp;utm_medium=website).

Get license plate reader deployed on Node-RED in under 60 minutes:

- Has [fast inference speed](https://platerecognizer.com/snapshot/#speeds) up to 21 ms.
- Runs on-premise on **Linux,** [**Windows**](https://platerecognizer.com/alpr-on-windows/?utm_source=github&amp;utm_medium=website), Mac, [**Jetson**](https://platerecognizer.com/alpr-on-nvidia-jetson-devices/?utm_source=github&amp;utm_medium=website) **,** [**Kubernetes**](https://platerecognizer.com/anpr-on-kubernetes/?utm_source=github&amp;utm_medium=website), [Raspberry Pi](https://platerecognizer.com/anpr-on-raspberry-pi/?utm_source=github&amp;utm_medium=website), [Zynq](https://platerecognizer.com/alpr-for-xilinx-zynq/?utm_source=github&amp;utm_medium=website), [96Boards](https://platerecognizer.com/alpr-for-96boards/?utm_source=github&amp;utm_medium=website), [LattePanda](https://platerecognizer.com/anpr-on-lattepanda/?utm_source=github&amp;utm_medium=website) and more.

ALPR, ANPR software on Node-RED is ideal for parking, toll, community security, and other use cases.

Our [license plate recognition (LPR)](https://platerecognizer.com/snapshot/?utm_source=github&amp;utm_medium=website) software can also forward results to our full **ALPR Dashboard** and [**Parking Management software**](https://parkpow.com/?utm_source=github&amp;utm_medium=website) solution, ParkPow.

Sign up for a [**Free Trial**](https://app.platerecognizer.com/accounts/signup/?utm_source=github&amp;utm_medium=website) now (no credit card required) or **learn more** at [https://platerecognizer.com](https://platerecognizer.com/).


## Install
Run the following npm command in your Node-RED user directory (typically ~/.node-red):
```
npm install node-red-contrib-plate-recognizer
```
Note that you need to ***signup*** for an account on [platerecognizer.com](https://app.platerecognizer.com/start/), and paste your private token into this node's config screen.  With a free account there is a limit to recognize 2500 images per month, but they also offer various paid license models.

## Node usage
This node will detect and recognise license plates in an image, using a deep learning cloud service.  The AI cloud service has been trained for license plates for more than 100 countries.

Send an image (as buffer or base64 encoded string) via an input message, to start a recognition:

![Basic flow](https://user-images.githubusercontent.com/14224149/74985812-88e78f00-5438-11ea-8f5c-790730d77047.png)

```
[{"id":"38586517.a5bf9a","type":"plate-recognizer","z":"d9a54719.b13a88","name":"","inputField":"payload","inputFieldType":"msg","outputField":"payload","outputFieldType":"msg","url":"https://api.platerecognizer.com/v1/plate-reader/","ignoreDuring":false,"makeAndModel":false,"statusText":"none","cameraId":"","regionFilter":false,"timestamp":false,"regionList":"","regionListType":"json","x":880,"y":660,"wires":[["30cb89da.c35546"],[]]},{"id":"e4699284.081c7","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":500,"y":660,"wires":[["9e2c9295.7f9a9"]]},{"id":"30cb89da.c35546","type":"debug","z":"d9a54719.b13a88","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","targetType":"full","x":1060,"y":660,"wires":[]},{"id":"9e2c9295.7f9a9","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"https://www.mercedes-benz.com/en/classic/history/h-number-plate-2020/_jcr_content/root/paragraph/paragraph-right/paragraphimage/image/MQ6-8-image-20191205151927/02-mercedes-benz-classic-h-number-plate-2020-2560x1440.jpeg","tls":"","persist":false,"proxy":"","authType":"","x":680,"y":660,"wires":[["38586517.a5bf9a"]]}]
```

The output message will contain the recognition results (in json format):

![Basic output](https://user-images.githubusercontent.com/14224149/75106301-521d9e80-561b-11ea-89b1-41dbf53bfac6.png)

The *"results*" will be a json array, containing a separate element for each license plate that has been recognised.  When the image contains N cars, then the array will contain N elements.

For each recognised license plate, some basic information will be delivered:

+ *box:* the bounding box (coordinates) where the vehicle is located inside the image.

+ *plate:* the license plate itself as plain text.

+ *region:* the region of the license plate (e.g. *"be"* for Belgium).  This object contains some nested fields:

   ![region](https://user-images.githubusercontent.com/14224149/75106572-7e86ea00-561e-11ea-9491-40f392e5b5b8.png)
   
   + *code:* the region code (from the [region list](http://docs.platerecognizer.com/#regions-supported)).
   + *score:* the confidence level for the region prediction, which is a value between 0 and 1 (with 1 the highest confidence).

+ *vehicle:* information about the vehicle itself.  This object contains some nested fields:

   ![vehicle](https://user-images.githubusercontent.com/14224149/75106701-8abf7700-561f-11ea-92a2-234bf6cc8887.png)

   + *type:* the type of vehicle (which can be Ambulance, Bus, Car, Limousine, Motorcycle, Taxi, Truck, Van, Unknown).
   + *score:* the confidence level for the vehicle prediction, which is a value between 0 and 1 (with 1 the highest confidence).
   + *box:* the bounding box (coordinates) where the vehicle is located inside the image.
   
+ *score:* the confidence level for the license plate text prediction, which is a value between 0 and 1 (with 1 the highest confidence).

+ *candidates:* sometimes the service isn't really sure whether it has recognised the license plate correctly.  Therefore a list of possible plate *'candidates'* will be supplied.  The first candidate is the same plate that has already been offered at the higher level:

   ![image](https://user-images.githubusercontent.com/14224149/75106803-7b8cf900-5620-11ea-8b05-ee94c093f9b7.png)

   In this case the AI service thinks (with 90,3% certainty) that the plate its "s0k92h", but it might be that the plate is "sok92h" (with 90,1% certainty).  In this case the confusion is between the number "0" and the character "o".

+ *dscore:* the confidence level for the license plate detection, which is a value between 0 and 1 (with 1 the highest confidence).

## Node properties
The node can be configured via a series of settings on the config screen:

### Input field
The field of the input message which will need to contain the input image.  By default ```msg.payload``` will be used.  The image should be a binary Buffer or a base64 encoded string.

### Output field
The field of the output message where the recognition result will be stored (in JSON format).  By default ```msg.payload``` will be used.

### API token
Create an account at [platerecogniser.com](https://platerecognizer.com/) and enter your private API token here.

### URL
Specify the URL of the recognition service, to allow different kind of setups:
+ Use the official *cloud service*, which will be the default (and most used) option.
+ Use a *local installation* (based on the SDK).
+ Use a local *Docker container*.

### Camera ID
Optionally a camera id can be specified, which will be sent to the recognition service.

### Status text
Specify how the recognition result needs to be displayed in the node status label:
+ *None:* Show no recognition results.
+ *Plate count:* Show the number of plates that have been recognised in the image.
+ *Plates:* Show a (comma separted) list of the plates that have been recognized in the image.
+ *Plates and scores:* Same as the previous option, but now the 'score' percentage is also added.

### Ignore images arriving during recognition
When selected images will automatically be skipped, when the previous image is still being recognized.  When deselected multiple images can be recognized simultaneously.

### Send Timestamp
When selected, the current timestamp will be sent in the request to the recognition service.</p>

### Predict vehicle make and model (MMC)
When selected not only the plate will be recognized, but there will also be a prediction of the vehicle brand and type.  

CAUTION: this is only supported for some paid account types!

### Specify one or more regions
When selected, an array of region codes can be specified (see [supported regions](http://docs.platerecognizer.com/#regions-supported). 

## Example flow (different cases)

The following flow explains some different use cases:
+ Image contain a single car.
+ Image containing a single car, but photografed from an angle.  It is important to be able to recognize license plates at angles, because a camera won't always be positioned directly in front of the cars.
+ Image containing two cars, which means the array will contain two individual recognitions.
+ Image containing no cars, which means the array will be empty.
+ Image containing a truck with a license plate, but also some texts on the truck itself.  There will be multiple recognitions in the array (because the texts will also be detected!).

Note that the [node-red-contrib-image-output](https://github.com/rikukissa/node-red-contrib-image-output) node needs to be installed also!

![image](https://user-images.githubusercontent.com/14224149/75100715-d4817080-55d1-11ea-9c64-2dae43df8fe8.png)

```
[{"id":"fff72ad.6f8c7d8","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":460,"wires":[["ee55dd94.02b2e"]]},{"id":"28523886.f82428","type":"debug","z":"d9a54719.b13a88","name":"Normal recognitions","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"recognition","targetType":"msg","x":1560,"y":580,"wires":[]},{"id":"ee55dd94.02b2e","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"https://www.mercedes-benz.com/en/classic/history/h-number-plate-2020/_jcr_content/root/paragraph/paragraph-right/paragraphimage/image/MQ6-8-image-20191205151927/02-mercedes-benz-classic-h-number-plate-2020-2560x1440.jpeg","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":460,"wires":[["231d72b4.5233ae"]]},{"id":"10f189ae.db2d76","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":660,"wires":[["24464bb6.a07b64"]]},{"id":"24464bb6.a07b64","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"https://image.freepik.com/free-photo/empty-parking-lot_1127-3298.jpg","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":660,"wires":[["231d72b4.5233ae"]]},{"id":"5ee1acd5.4e0784","type":"comment","z":"d9a54719.b13a88","name":"No cars","info":"","x":890,"y":620,"wires":[]},{"id":"231d72b4.5233ae","type":"plate-recognizer","z":"d9a54719.b13a88","name":"","inputField":"payload","inputFieldType":"msg","outputField":"recognition","outputFieldType":"msg","url":"https://api.platerecognizer.com/v1/plate-reader/","ignoreDuring":false,"makeAndModel":false,"statusText":"scores","cameraId":"","regionFilter":false,"timestamp":false,"regionList":"","regionListType":"json","x":1320,"y":660,"wires":[["28523886.f82428","37b1b695.62fa4a"],["100a66ab.4607f9"]]},{"id":"37b1b695.62fa4a","type":"image","z":"d9a54719.b13a88","name":"Show analyzed image","width":"400","data":"payload","dataType":"msg","thumbnail":false,"active":true,"x":1740,"y":640,"wires":[]},{"id":"869cd924.d991d8","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":960,"wires":[["f6b6ac8f.40806"]]},{"id":"f6b6ac8f.40806","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"https://static.nieuwsblad.be/Assets/Images_Upload/2015/05/24/patton_drivers_2015_1.jpg?maxheight=460&maxwidth=638&scale=both","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":960,"wires":[["231d72b4.5233ae"]]},{"id":"6ac3b436.692fac","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":760,"wires":[["d2c4249d.901fd8"]]},{"id":"d2c4249d.901fd8","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"http://newscrane.com/wp-content/uploads/2019/09/Car-insurance-Newscrane-02.jpg","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":760,"wires":[["231d72b4.5233ae"]]},{"id":"9a21e444.b22fc8","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":560,"wires":[["8e81e18d.39882"]]},{"id":"8e81e18d.39882","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"http://www.piepenbroek.nl/foto2010/baltisch/IMG_1499.JPG","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":560,"wires":[["231d72b4.5233ae"]]},{"id":"5899f099.1b645","type":"comment","z":"d9a54719.b13a88","name":"Two cars","info":"","x":900,"y":520,"wires":[]},{"id":"1db8bd98.23d672","type":"comment","z":"d9a54719.b13a88","name":"One car","info":"","x":890,"y":420,"wires":[]},{"id":"100a66ab.4607f9","type":"debug","z":"d9a54719.b13a88","name":"Errors","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"recognition","targetType":"msg","x":1510,"y":680,"wires":[]},{"id":"cdfaa780.c2fa18","type":"comment","z":"d9a54719.b13a88","name":"Truck with labels","info":"","x":920,"y":920,"wires":[]},{"id":"c1d2f038.82ecc","type":"comment","z":"d9a54719.b13a88","name":"Car at angle","info":"","x":910,"y":720,"wires":[]},{"id":"8c4f0d01.426a2","type":"inject","z":"d9a54719.b13a88","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":920,"y":860,"wires":[["7b580c6e.105764"]]},{"id":"7b580c6e.105764","type":"http request","z":"d9a54719.b13a88","name":"","method":"GET","ret":"bin","paytoqs":false,"url":"https://askautoexperts.com/wp-content/uploads/1931-Dodge-1024x768.jpg","tls":"","persist":false,"proxy":"","authType":"","x":1100,"y":860,"wires":[["231d72b4.5233ae"]]},{"id":"da494e5a.e21ff","type":"comment","z":"d9a54719.b13a88","name":"Another car at angle","info":"","x":930,"y":820,"wires":[]}]
```

## Recognition status
The output message will contain the recognition status (and statusText):

![status](https://user-images.githubusercontent.com/14224149/75118670-4f9f6100-567c-11ea-92fa-98d2801f3dc3.png)


+ When the service has finished the recognition without problems, the output message will be send on the *first output* with status 2xx.
+ When the service isn't able to process the recognition, the output message will be send to the *second output* with status 4xx.  The status code will explain what went wrong:
   + 403: Forbidden due to incorrect API token.
   + 413: The payload is too large and exceeds their [upload limits](https://app.platerecognizer.com/upload-limit/).
   + 429: Too many requests have been send in a given amount of time. Upgrade your license for higher number of calls per second.

## Plate statistics
Since the number of recognitions per month is limited (e.g. 2500 for a free account), it is very useful to determine from time to time how many recognitions are left.  This way you can avoid situations where you are not aware that you have run out of recognitions...

A second node (*"Plate statistics"*) has been provided to get the statistics of your account (with *'URL'* and *'API token'* settings identical as described above):

![Statistics flow](https://user-images.githubusercontent.com/14224149/75119790-6cd92d00-5686-11ea-806d-d755b27eb8d6.png)

```
[{"id":"9b354f46.f081d","type":"plate-statistics","z":"d9a54719.b13a88","name":"","outputField":"payload","outputFieldType":"msg","url":"https://api.platerecognizer.com/v1/statistics/","x":980,"y":480,"wires":[["54063a77.493ae4"]]},{"id":"5653b69.13c4648","type":"inject","z":"d9a54719.b13a88","name":"Get statistics","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":770,"y":480,"wires":[["9b354f46.f081d"]]},{"id":"54063a77.493ae4","type":"debug","z":"d9a54719.b13a88","name":"Plate statistics","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":1190,"y":480,"wires":[]}]
```
The resulting statistics (in json format) contain the maximum number of statistics, and also the used number of statistics of the current month:

![Statistics output](https://user-images.githubusercontent.com/14224149/75119816-a3af4300-5686-11ea-9a7d-fb9a8292b823.png)
