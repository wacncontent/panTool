<properties 
	pageTitle="Frequently asked questions" 
	description="Frequently asked questions (FAQs)" 
	services="media-services" 
	documentationCenter="" 
	authors="Juliako" 
	manager="dwrede" 
	editor=""/>

<tags
	ms.service="media-services"
	ms.date="12/05/2015"
	wacn.date=""/>


#Frequently asked questions  

##Overview

Q: How do you scale indexing?

A: The reserved units are the same for Encoding and Indexing tasks. Follow instructions on [How to Scale Encoding Reserved Units](/documentation/articles/media-services-how-to-scale). **Note** that Indexer performance is not affected by Reserved Unit Type.

Q: I uploaded, encoded, and published a video. What would be the reason the video does not play when I try to stream it? 

A: One of the most common reasons is you do not have at least one reserved streaming unit allocated on the streaming endpoint from which you are trying to playback.  Follow instructions on [How to Scale Streaming Reserved Units](/documentation/articles/media-services-how-to-scale).

Q: Can I do compositing on a live stream? 

A: Compositing on live streams is currently not offered in Azure Media Services, so you would need to pre-compose on your computer.

Q: Can I use Azure CDN with Live Streaming? 

A: Media Services supports integration with Azure CDN (for more information, see [How to Manage Streaming Endpoints in a Media Services Account](/documentation/articles/media-services-manage-origins#enable_cdn)).  You can use Live streaming with CDN. Azure Media Services provides Smooth Streaming, HLS and MPEG-DASH outputs. All these formats use HTTP for transferring data and get benefits of HTTP caching. In live streaming actual video/audio data is divided to fragments and this individual fragments get cached in CDN. Only data needs to be refreshed is the manifest data. CDN periodically refreshes manifest data.

Q: Does Azure Media services support storing images?

A: If you are just looking to store JPEG or PNG images, you should keep those in Azure Blob Storage. There is no benefit to putting them in your Media Services account unless you want to keep them associated with your Video or Audio Assets. Or if you might have a need to use the images as overlays in the video encoder. Media Services encoder supports overlaying images on top of videos, and that is what it lists JPEG and PNG as supported input formats. For more information, see [Creating Overlays](https://msdn.microsoft.com/zh-cn/library/azure/dn640496.aspx).

Q: How can I copy assets from one Media Services account to another. 

A: To copy assets from one Media Services account to another, use [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) extension method available in the [Azure Media Services .NET SDK Extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions/) repository. For more information, see [this](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) forum thread. 

Q: How can I rotate a video during the encoding process.

A: The [Media Encoder Standard](/documentation/articles/media-services-dotnet-encode-with-media-encoder-standard) supports rotation by angles of 90/180/270. The default behavior is "Auto", where it tries to detect the rotation metadata in the incoming MP4/MOV file and compensate for it. Include the following **Sources** element to one of the json presets defined [here](http://msdn.microsoft.com/zh-cn/library/azure/mt269960.aspx):
	
	"Version": 1.0,
	"Sources": [
	{
	  "Streams": [],
	  "Filters": {
	    "Rotation": "90"
	  }
	}
	],
	"Codecs": [
	
	...
