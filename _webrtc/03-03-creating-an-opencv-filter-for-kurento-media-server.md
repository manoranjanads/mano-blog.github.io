---
title: "How to Create an OpenCV Filter for Kurento Media Server"
permalink: /webrtc/kurento/creating-an-opencv-filter-for-kurento-media-server.html
excerpt: "WebRTC based Machine Vision"
header:
  overlay_image: /assets/images/webrtc/03-03-creating-an-opencv-filter-for-kurento-media-server/03-03-creating-opencv-filter-for kurento-media-server.jpg
  teaser: /assets/images/webrtc/03-03-creating-an-opencv-filter-for-kurento-media-server/03-03-creating-opencv-filter-for kurento-media-server.jpg
  overlay_filter: 0.5
last_modified_at: 2018-11-08T14:07:49-04:00
redirect_from:
  - /theme-setup/
toc: true
author: Mano
---
[Meetrix.IO](https://meetrix.io) provides installation, configuration and customization services for Kurento.
Get fully configured Kurento setup on your own server (starting from `$250`).
Please shoot an email to `hello@meetrix.io` for more information
{: .notice--success}

Using kurento modules, we can perform image processing and machine vision tasks on media streams.
In Kurento module architecture, we can access each frame of the video stream and perform operations on them.
In this guide, we are going to display a simple 'hello world' text on each webRTC video frame using using OpenCV
These instructions were tested on Ubuntu 16.04

All the source cods for this tutorial can be found on [Meetrix.IO Github Page](https://github.com/meetrix/kurento-custom-opencv-module-tutorial.git)


Installing Kurento-Media-server-Dev
---

You can install Kurento Media Server Dev with following command.
Complete guide on installing kurento can be found [here](https://meetrix.io/blog/webrtc/kurento.html)
```bash
DISTRO="xenial"
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5AFA7A83
```

and then

```
sudo tee "/etc/apt/sources.list.d/kurento.list" >/dev/null <<EOF
deb [arch=amd64] http://ubuntu.openvidu.io/6.8.1 $DISTRO kms6
EOF

sudo apt-get update
sudo apt-get install kurento-media-server-dev
```

Creating the Boilerplate for the Plugin
---

Following command will create the plugin structure.

```bash
kurento-module-scaffold.sh <module_name> <output_directory> opencv_filter
```

I'm going to create a plugin named `MeetrixKurentoHelloWorld` in the current directory. So here is my command.

```
kurento-module-scaffold.sh MeetrixKurentoHelloWorld ./ opencv_filter
```

This will create a directory named `meetrix-kurento-hello-world` inside the current directory.
Go in to the directory and create a directory named `build`.
run `cmake ./` inside `build` directory
```bash
cd ./meetrix-kurento-hello-world
mkdir build
cd build
cmake ./
```

This will generate the boilerplates inside `server` directory of the plugin directory.

Implementing the Image Processing Code
---

Inside `server/implementation/objects/` directory you will see four files.
open`server/implementation/objects/<YOUR_MODULE_NAME>OpenCVImpl.cpp` file and
locate following method.

```
void MeetrixKurentoHelloWorldOpenCVImpl::process (cv::Mat &mat)
 {
   // FIXME: Implement this
   throw KurentoException (NOT_IMPLEMENTED, "MeetrixKurentoHelloWorldOpenCVImpl::process: Not implemented");
 }
 ```
 
 replace the following code inside the method.
 
 ```
 void MeetrixKurentoHelloWorldOpenCVImpl::process (cv::Mat &mat)
  {
    cv::Point textOrg(100, 100);
    putText( mat, "Hello from Meetrix.IO", textOrg, 1, 2, cv::Scalar(0, 0, 0) );
  }
```
This code snippet will display `Hello from Meetrix.IO` text on the video.

Building and Installing the Module
---
Run the following command inside the `build` directory.
```
cmake .. -DCMAKE_INSTALL_PREFIX=/usr && make && sudo make install
```


If you get an error similar to this

```
/usr/bin/ld: libkmsmeetrixkurentohelloworldinterface.a(MeetrixKurentoHelloWorldInternal.cpp.o): relocation R_X86_64_32 against `.rodata' can not be used when making a shared object; recompile with -fPIC
libkmsmeetrixkurentohelloworldinterface.a: error adding symbols: Bad value
```

open `CMakeLists.txt` file in module directory and locate following line.

```cpp
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -DHAVE_CONFIG_H -Wall -Werror -std=c++11")
```
then replace it with following line (we are just adding `-fPIC` flag here)
```cpp
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fPIC -DHAVE_CONFIG_H -Wall -Werror -std=c++11")

```


Then run the above build command again (inside the build directory).
If everything goes well, the module will be installed to Kurento Media Server.

Verifying
---
To verify whether the module has been installed correctly, run
```
kurento-media-server	-v
```
This will output something similar to following

```
Kurento Media Server version: 6.7.1
Found modules:
	'chroma' version 6.7.1~7.g8cb7bc0
	'core' version 6.7.1
	'elements' version 6.7.1
	'filters' version 6.7.1
	'meetrixkurentohelloworld' version 0.0.1~0.gc3753ed
	'platedetector' version 6.7.1~7.gf74700b
	'pointerdetector' version 6.7.1~6.g5ed9fea
```

Check whether your module is listed there.
Then, restart Kurento Medea Server.

```bash
service kurento-media-server restart
```

Buildinging the JS Client
---
Inside the `build` directory, run
```bash
cmake .. -DGENERATE_JS_CLIENT_PROJECT=TRUE
```

This sill generate the javascript client insede the `build/js` directory.
If you open the `build/js/package.json`, you will see your module name.

```bash
"name": "kurento-module-meetrixkurentohelloworld"
```

then if you open the `build/js/lib/index.js ` you will see the exports.

```js
exports.MeetrixKurentoHelloWorld = MeetrixKurentoHelloWorld;
```

also if you open the `build/js/lib/MeetrixKurentoHelloWorld.js `,
you will see the following function.

```js
function MeetrixKurentoHelloWorld(){
  MeetrixKurentoHelloWorld.super_.call(this);
};
```

So this is the function that we are going to call. Make a note of it.

Sample Application
---
We are going to use `https://github.com/Kurento/kurento-tutorial-node/tree/master/kurento-chroma`
as the template. So, let's clone it.
```bash
git clone https://github.com/Kurento/kurento-tutorial-node.git
cd kurento-tutorial-node/kurento-chroma
npm install
```


Setingup the JS Client
---
We need to copy the `js` directory that was generated in `build` directory of the module directory, and
move the entire `js` directory to `kurento-tutorial-node/kurento-chroma/node_modules`. Also we need to rename it to the module name that we found out in `package.json` directory.

In my case I use `mv` command instead of copying.
```bash
mv meetrix-kurento-hello-world/build/js kurento-tutorial-node/kurento-chroma/node_modules/kurento-module-meetrixkurentohelloworld
```

Then we have to modify the code of `server.js` file in `kurento-chroma`.
Open the `serve.js` file and locate the following line.
```js
kurento.register('kurento-module-chroma');
```

and replace it with

```js
kurento.register('<MODULE_NAME_THAT_YOU_FOUND_IN_PACKAGE.JSON>');
```

In my case

```js
kurento.register('kurento-module-meetrixkurentohelloworld');
```

Then locate the `createMediaElements` function

```js
function createMediaElements(pipeline, ws, callback) {
    pipeline.create('WebRtcEndpoint', function(error, webRtcEndpoint) {
        if (error) {
            return callback(error);
        }

        var options = {
            window: kurento.getComplexType('chroma.WindowParam')({
                topRightCornerX: 5,
                topRightCornerY: 5,
                width: 30,
                height: 30
            })
        }
        pipeline.create('chroma.ChromaFilter', options, function(error, filter) {
            if (error) {
                return callback(error);
            }

            return callback(null, webRtcEndpoint, filter);
        });
    });
}
```

and replace it with

```js
function createMediaElements(pipeline, ws, callback) {
    pipeline.create('WebRtcEndpoint', function(error, webRtcEndpoint) {
        if (error) {
            return callback(error);
        }

        var options = {}
        pipeline.create('meetrixkurentohelloworld.MeetrixKurentoHelloWorld', options, function(error, filter) {
            if (error) {
                return callback(error);
            }

            return callback(null, webRtcEndpoint, filter);
        });
    });
}

```

We got the `meetrixkurentohelloworld.MeetrixKurentoHelloWorld` by concatenating 
`meetrixkurentohelloworld` part from `kurento-module-meetrixkurentohelloworld`, which can be found on the `package.json` of the generated JS client
 and 
 `MeetrixKurentoHelloWorld` from the function name.

(You can debug the loaded classes by printing `kurento.register.classes` in server.js as well)

There are couple of other places that we need to modify, in order to remove all parts related to `kurento-chroma`

Locate `connectMediaElements` function

```js
function connectMediaElements(webRtcEndpoint, filter, callback) {
    webRtcEndpoint.connect(filter, function(error) {
        if (error) {
            return callback(error);
        }

        filter.setBackground(url.format(asUrl) + 'img/mario.jpg', function(error) {
            if (error) {
                return callback(error);
            }

            filter.connect(webRtcEndpoint, function(error) {
                if (error) {
                    return callback(error);
                }

                return callback(null);
            });
        });
    });
}
```

and change it to

```js
function connectMediaElements(webRtcEndpoint, filter, callback) {
    webRtcEndpoint.connect(filter, function(error) {
        if (error) {
            return callback(error);
        }

        //filter.setBackground(url.format(asUrl) + 'img/mario.jpg', function(error) {
            //if (error) {
                //return callback(error);
            //}

            filter.connect(webRtcEndpoint, function(error) {
                if (error) {
                    return callback(error);
                }

                return callback(null);
            });
        });
    //});
}
```
We are just commenting out some functions of original `Kurento-Chroma` project.


Running the Example
---

Run `node server.js` and open the application usin a webrtc compatible browser: `https://<YOUR_SERVER_IP>:8443/#`

![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/webrtc/10-creating-an-opencv-filter-for-kurento-media-server/kurento-opencv-module-tutorial-demo.png){: .align-center}


Calling a Function in the Created Plugin
---
Currently the text that is shown on the video is a static text. Lets set the position of text dynamically. 
For this we can declare private variables `textToPrint, positionX, positionY`  and a method called `setText(text, x, y)`. 

First we define the method in `meetrix-kurento-hello-world/src/server/interface/meetrixkurentohelloworld.MeetrixKurentoHelloWorld.kmd.json`

```json
{
  "remoteClasses": [
    {
      "name": "MeetrixKurentoHelloWorld",
      "extends": "OpenCVFilter",
      "doc": "MeetrixKurentoHelloWorld interface. Documentation about the module",
      "constructor": {
        "doc": "Create an element",
          "params": [
            {
              "name": "mediaPipeline",
              "doc": "the parent :rom:cls:`MediaPipeline`",
              "type": "MediaPipeline",
              "final": true
            }
          ]
        },
      "methods": [
        {
          "name": "setText",
          "doc": "Sets the overlay text",
          "params": [
            {
              "name": "text",
              "doc": "Text to Print",
              "type": "String"
            },
            {
              "name": "x",
              "doc": "Position x",
              "type": "int"
            },
            {
              "name": "y",
              "doc": "Position y",
              "type": "int"
            }
          ]
        }
      ]
    }
  ]
}
```

Then define the methods as follow in C++ files

in `meetrix-kurento-hello-world/src/server/implementation/objects/MeetrixKurentoHelloWorldOpenCVImpl.hpp`

```c++
private:
  std::string textToPrint;
  int positionX;
  int positionY;

public:
  MeetrixKurentoHelloWorldOpenCVImpl ();

  virtual ~MeetrixKurentoHelloWorldOpenCVImpl () {};

  virtual void process (cv::Mat &mat);

  void setText(const std::string &text, const int x, const int y);
```


in `meetrix-kurento-hello-world/src/server/implementation/objects/MeetrixKurentoHelloWorldOpenCVImpl.cpp`

```c++
/* Autogenerated with kurento-module-creator */

#include "MeetrixKurentoHelloWorldOpenCVImpl.hpp"
#include <KurentoException.hpp>

namespace kurento
{
namespace module
{
namespace meetrixkurentohelloworld
{

MeetrixKurentoHelloWorldOpenCVImpl::MeetrixKurentoHelloWorldOpenCVImpl ()
{
    textToPrint = "Hello From Meetrix";
    positionX = 100;
    positionY = 100;
}

/*
 * This function will be called with each new frame. mat variable
 * contains the current frame. You should insert your image processing code
 * here. Any changes in mat, will be sent through the Media Pipeline.
 */
void MeetrixKurentoHelloWorldOpenCVImpl::process (cv::Mat &mat)
{
    cv::Point textOrg(positionX, positionY);
    putText( mat, textToPrint, textOrg, 1, 2, cv::Scalar(0, 0, 0) );
}

void MeetrixKurentoHelloWorldOpenCVImpl::setText(const std::string &text, const int x, const int y)
{
    textToPrint= text;
    positionX= x;
    positionY= y;
}
```

in `meetrix-kurento-hello-world/src/server/implementation/objects/MeetrixKurentoHelloWorldImpl.hpp`

```c++
/* Autogenerated with kurento-module-creator */

#include "MeetrixKurentoHelloWorldOpenCVImpl.hpp"
#include <KurentoException.hpp>

namespace kurento
{
namespace module
{
namespace meetrixkurentohelloworld
{

MeetrixKurentoHelloWorldOpenCVImpl::MeetrixKurentoHelloWorldOpenCVImpl ()
{
    textToPrint = "Hello From Meetrix";
    positionX = 100;
    positionY = 100;
}

/*
 * This function will be called with each new frame. mat variable
 * contains the current frame. You should insert your image processing code
 * here. Any changes in mat, will be sent through the Media Pipeline.
 */
void MeetrixKurentoHelloWorldOpenCVImpl::process (cv::Mat &mat)
{
    cv::Point textOrg(positionX, positionY);
    putText( mat, textToPrint, textOrg, 1, 2, cv::Scalar(0, 0, 0) );
}

void MeetrixKurentoHelloWorldOpenCVImpl::setText(const std::string &text, const int x, const int y)
{
    textToPrint= text;
    positionX= x;
    positionY= y;
}




} /* meetrixkurentohelloworld */
} /* module */
} /* kurento */

```

in `meetrix-kurento-hello-world/src/server/implementation/objects/MeetrixKurentoHelloWorldImpl.cpp`

```c++
/* Autogenerated with kurento-module-creator */

#include <gst/gst.h>
#include "MediaPipeline.hpp"
#include <MeetrixKurentoHelloWorldImplFactory.hpp>
#include "MeetrixKurentoHelloWorldImpl.hpp"
#include <jsonrpc/JsonSerializer.hpp>
#include <KurentoException.hpp>
#include "MediaPipelineImpl.hpp"

#define GST_CAT_DEFAULT kurento_meetrix_kurento_hello_world_impl
GST_DEBUG_CATEGORY_STATIC (GST_CAT_DEFAULT);
#define GST_DEFAULT_NAME "KurentoMeetrixKurentoHelloWorldImpl"

namespace kurento
{
namespace module
{
namespace meetrixkurentohelloworld
{

MeetrixKurentoHelloWorldImpl::MeetrixKurentoHelloWorldImpl (const boost::property_tree::ptree &config, std::shared_ptr<MediaPipeline> mediaPipeline) : OpenCVFilterImpl (config, std::dynamic_pointer_cast<MediaPipelineImpl> (mediaPipeline) )

{
}

MediaObjectImpl *
MeetrixKurentoHelloWorldImplFactory::createObject (const boost::property_tree::ptree &config, std::shared_ptr<MediaPipeline> mediaPipeline) const
{
  return new MeetrixKurentoHelloWorldImpl (config, mediaPipeline);
}

MeetrixKurentoHelloWorldImpl::StaticConstructor MeetrixKurentoHelloWorldImpl::staticConstructor;

MeetrixKurentoHelloWorldImpl::StaticConstructor::StaticConstructor()
{
  GST_DEBUG_CATEGORY_INIT (GST_CAT_DEFAULT, GST_DEFAULT_NAME, 0,
                           GST_DEFAULT_NAME);
}

void MeetrixKurentoHelloWorldImpl::setText(const std::string &text, const int x, const int y)
{
    MeetrixKurentoHelloWorldOpenCVImpl::setText(text, x, y);
}

} /* meetrixkurentohelloworld */
} /* module */
} /* kurento */

```


Updating Module
---
Build again
```bash
cd build/
cmake .. -DCMAKE_INSTALL_PREFIX=/usr && make && sudo make install
```
Copy to builds to kurento modules and restart kurento server
```bash
cp ./build/src/server/libkmsmeetrixkurentohelloworldmodule.so /usr/lib/x86_64-linux-gnu/kurento/modules/
sudo service kurento-media-server restart
```

Build the JS Client again and copy it to node_modules

```bash
cmake .. -DGENERATE_JS_CLIENT_PROJECT=TRUE
cp -r js ../../kurento-chroma/node_modules/kurento-module-meetrixkurentohelloworld
```

Just after creating the filter, we can write a function to change the position of text dynamically
```js
  setInterval(function(){
    const randomX = Math.floor(Math.random() * Math.floor(200));
    const randomY = Math.floor(Math.random() * Math.floor(200));
    filter.setText("Hello from Meetrix", randomX, randomY,function(){
        console.log("Set Text Called");}
    );
  }, 500);
```

Please note that this code will continue to run evern after the pipeline is terminated, make a not to take care of that
That's It !