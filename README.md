# Import a Machine Learning Model (Core ML) and Classify Images Using Different Models for iOS app

## Using a Core ML

Modern machine-learning generally relies on very large multi-dimensional arrays, arranged in complex graphs. These 
models are the result of their own development process. Core ML (https://developer.apple.com/documentation/coreml) 
does not have APIs for developing or training models, only for consuming them. 

Apple has defined a file format, .mlmodel, that stores the technique description and metadata, the graph structure, and 
the values in a single file. To use that model in a program, it must be compiled with a special tool and the 
compilation-products included in your program’s `Resources` directory or made available for download by the app.

Models can vary greatly in size. This app demonstrates two models, both intended to describe the contents of an image, 
that trade off size for accuracy. One model, SqueezeNet, is approximately 5MB. The other, VGG16, is 550MB. In this app 
you can use either model to classify either stored photos or camera images. 

VGG16 is _not_ included in this Github repository, so you _must_ follow the instructions in this file to download and 
compile it for use in the app.

## Downloading and compiling a Core ML

This application uses two image-classification models: SqueezeNet and VGG16. The compiled SqueezeNet model is already 
included in the Github directory. VGG16 must be downloaded, compiled, and included in the project before you can use it.

To download the 550MB VGG16, from a terminal, run:

    wget https://docs-assets.developer.apple.com/coreml/models/VGG16.mlmodel

The Core ML compiler is included with the Xcode developer tools   (Xcode 9 and higher). To compile an .mlmodel, run:

    xcrun coremlcompiler compile {model.mlmodel} {outputFolder}

The `outputFolder` should be the Resources folder of your iOS solution. The output of the Core ML compiler is a 
folder with a .mlmodelc extension. The contents will vary depending on the type of machine-learning algorithm used. 

Once you’ve compiled the model, you must include it in your solution. Open the sample application and in the Solution 
Pad, right-click on the Resources folder. Select “Add an existing folder…”:

![Resources directory in Solution Pad](docs/figure1.png)

And select the output folder from the Machine Learning compiler. Choose “Include All” and “OK” to add the files to your solution. 
Once you have done so, the Resources folder for the sample app should look like:

![Resources once VGG16 added](docs/figure2.png)

## Loading a Machine Learning model in your app

The function to load a Core ML is [MLModel.FromUrl](https://developer.xamarin.com/api/member/CoreML.MLModel.FromUrl/p/Foundation.NSUrl/Foundation.NSError@/), so it _is_ possible to load a smaller model from the Web, but generally
 you will load it with code similar to:

	MLModel LoadModel(string modelName)  
	{  
	    NSBundle bundle = NSBundle.MainBundle;  
		var assetPath = bundle.GetUrlForResource(modelName, "mlmodelc");  
        NSError err;
        var mdl = MLModel.FromUrl(assetPath, out err);  
		if (err != null)  
		{  
		    ErrorOccurred(this, new EventArgsT\<string\>(err.ToString()));  
		}  
		return mdl;  
	}  


# Classifying Images Using Core ML

This sample app uses the two different image-classification models to attempt to identify the contents of an image, 
either from the Photos store or taken with the camera. The VGG16 model generally produces better results, but at a cost 
of 2 orders of magnitude in the size of the model! 

The models can take several seconds to classify an image. Classification is done asynchronously and a 
`PredictionsUpdated` event raised when new results are available. The 5 most-likely results are displayed, along with 
the model’s accuracy estimate:

![app screenshot showing results](docs/figure3.png)

## Getting Started

This example project runs only in iOS 11 or above.


## Deploy This C# Sample Project To iOS

Please follow the procedure as described in `"Develop and Deploy iOS app to iPhone or iPad device using Visual Studio 2017 Community.pdf"` to deploy this sample project to iOS.
