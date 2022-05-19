# **Documentation of the plugin OpenCV - Cameras**

## **Installation**
1. Get the plugin on the UE4 marketplace then simply install it via the Epic launcher for the engine version you want.
2. Once this step is done, you can open your UE4 projet, go to the plugins settings panel, then enable the "OpenCV - Cameras" plugin.

![](./Screenshots/Installation.jpg)


## **How to use**
### **CameraCapture component**
1. In an Actor, add the CameraCapture component.

![](./Screenshots/HowToUse_1.jpg)

2. Customize your CameraCapture component with the variables in the "CameraCapture" categorie.

![](./Screenshots/HowToUse_2.jpg)

3. Start and stop the capture with the StartCapture and StopCapture nodes.

![](./Screenshots/HowToUse_3.jpg)

4. Click on your CameraCapture component then, in the "Events" categorie, create the "OnNewFrameAvailable" event by clicking on the "+" green button.

![](./Screenshots/HowToUse_4.jpg)

This event will be called each time a new frame is available, so use it to update a material with the Texture2D or whatever you want. You can found an example map in the content plugin folder, feel free to test it!

And do not forget to add required Android permissions for the cameras.
![](./Screenshots/GrantPermissions.jpg)

### **OpenCV (C++ only)**

If you want to use OpenCV in your C++ code, first, you need to add the plugin dependencies in your [Project_Name].build.cs:
```c#
using System;						
using System.IO;					
using System.Collections.Generic; 	
using UnrealBuildTool;

public class MobileSensorManager : ModuleRules
{
	public MobileSensorManager(ReadOnlyTargetRules Target) : base(Target)
	{
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;
	
		PrivateDependencyModuleNames.AddRange(new string[] {});
		PublicDependencyModuleNames.AddRange(new string[] { 
			"Core", 
			"CoreUObject", 
			"Engine", 
			"InputCore",
            "OpenCVCameras" // <- Here.
        });
    }
}
```

Then you need to import the libraries in your C++ files, like this:

```c++
// Prevent some conflict warnings between UE and OpenCV.
#pragma push_macro("check")
#undef check
#define int64 OpenCV_int64
#define uint64 OpenCV_uint64
#pragma warning(push)
#pragma warning(disable : 4946)

// Import the modules that you need here.
#include "opencv2/core.hpp"
#include "opencv2/imgproc.hpp"

// Revert changes made above.
#pragma warning(pop)
#undef int64
#undef uint64
#pragma pop_macro("check")
```

Then you can use OpenCV normally, like:

```c++
// Example code.
cv::Mat Frame;
cv::Vec3b Pixel = Frame.at<cv::Vec3b>(Y, X);
```


Check out the Driver.h file (located in the [plugin folder]/Source/OpenCVCameras/Public/OpenCV/ folder) to know how this plugin use OpenCV.