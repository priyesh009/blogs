![](RackMultipart20211101-4-1bp2u7w_html_cfd7229cc3d146b7.png)

**Introduction:**

Can a computer analyze an image and detect the text in it as a human does?

Understanding the text that appears on images is important for improving experiences, such as a more relevant photo search. Understanding text in images along with the context in which it appears also helps some systems to proactively identify inappropriate or harmful content .

Being able to detect text in images is, in fact, one of the most anticipated features that have got added into Rekognition. Customers have been pressing about recognizing text embedded in images, such as street signs and license plates captured by traffic cameras, news, and captions on TV screens, or stylized quotes overlaid on phone-captured family pictures.

Amazon Web Services offers a product called Rekognition . The purpose of AWS Rekognition is to analyze images and predict what objects are in the image, if there are any faces or any transcribe text. But to use Rekognition you need to know little, if anything, about machine learning at all! Amazon Rekognition includes a simple, easy-to-use API that can quickly analyze any stored image or video

You simply point Rekognition to any image or video file that&#39;s stored in Amazon S3 and tell it what computer vision task you want and get the results back.

**Application of Amazon Rekognition** :-

Amazon Rekognition Text in Image enables you to recognize and extract textual content from images. Text in Image supports most fonts, including highly stylized ones. It detects text and numbers in different orientations, such as those commonly found in banners and posters. In image sharing and social media applications, you can use it to enable visual search based on an index of images that contain the same keywords. In media and entertainment applications, you can catalog videos based on relevant text on screen, such as ads, news, sport scores, and captions. Finally, in public safety applications, you can identify vehicles based on license plate numbers from images taken by street cameras.

In image sharing and social media applications, you can now enable visual search based on an index of images that contain the same keywords. In media and entertainment applications, you can catalogue videos based on relevant text on screen, such as ads, news, sport scores, and captions. Additionally, in security and safety applications, you can identify vehicles based on license plate numbers from images taken by street cameras

**Detecting text:**

Amazon Rekognition text detection can detect text in images and videos. It can then convert the detected text into machine-readable text. You can use the machine-readable text detection in images to implement solutions such as:

- Visual search. An example is retrieving and displaying images that contain the same text.
- Content insights. An example is providing insights into themes that occur in text that&#39;s recognized in extracted video frames. Your application can search recognized text for relevant content—such as news, sport scores, athlete numbers, and captions.
- Navigation. An example is developing a speech-enabled mobile app for visually impaired people that recognizes the names of restaurants, shops, or street signs.
- Public safety and transportation support. An example is detecting car license plate numbers from traffic camera images.
- Filtering. An example is filtering out personally identifiable information from images.

**For text detection in videos, you can implement solutions such as** :

- Searching videos for clips where specific text keywords, such as guest&#39;s name on a graphic in a news show
- Compliance and moderation - detecting accidental text, profanity or spam
- Finding all text overlays on the video timeline for further processing, such as replacing with text in another language for content internationalization
- Finding text locations, so that other graphics can be aligned accordingly

Below are some of the leading organizations worldwide who are using Amazon Rekognition to add image and video analysis to their applications.

| ![](RackMultipart20211101-4-1bp2u7w_html_d8b9f985e3435312.png) ![](RackMultipart20211101-4-1bp2u7w_html_81d1b7d73cdd1247.png) ![](RackMultipart20211101-4-1bp2u7w_html_3486e01d1c915702.png)
**HERE Technologies Sportograf Influential** |
| --- |

**Following are some important operations** :

**DetectText**

This method detects text in .jpeg or .png format images .Detects text in the input image and converts it into machine-readable text.

Pass the input image as base64-encoded image bytes or as a reference to an image in an Amazon S3 bucket. If you use the AWS CLI to call Amazon Rekognition operations, you must pass it as a reference to an image in an Amazon S3 bucket.

It can detect up to 50 words in an image . It can also detect numbers as well as some common symbols.[@, /, $, %. -, \_, +, \*, #.]

**GetTextDetection**

Gets the text detection results of a Amazon Rekognition Video analysis started by StartTextDetection .

Text detection with Amazon Rekognition Video is an asynchronous operation. You start text detection by calling StartTextDetection which returns a job identifier (JobId ) When the text detection operation finishes, Amazon Rekognition publishes a completion status to the Amazon Simple Notification Service topic registered in the initial call to StartTextDetection .

To get the results of the text detection operation, first check that the status value published to the Amazon SNS topic is SUCCEEDED . if so, call GetTextDetection and pass the job identifier (JobId ) from the initial call of StartLabelDetection .

DetectText and GetTextDetection detects words and lines.

Consider the below example :

![](RackMultipart20211101-4-1bp2u7w_html_7bccad8b54d16ea0.png)

The blue boxes represent information about the detected text and the location of the text that&#39;s returned by the DetectText operation (on an image) or the GetTextDetection operation (on a single frame of a video). In this example, Amazon Rekognition detects &quot;IT&#39;s&quot;, &quot;MONDAY&quot;, &quot;but&quot;, &quot;keep&quot;, and &quot;Smiling&quot; as words. To be detected, text must be within +/- 90 degrees orientation of the horizontal axis.

**Detecting text in an image:**

Usually it is recommended to access Rekognition and other AWS products through a client library. Many different AWS client libraries support different languages. Here we&#39;ll be using Python and the boto3 package .

- Boto is the Amazon Web Services (AWS) SDK for Python. It enables Python developers to create, configure, and manage AWS services, such as EC2 and S3.

**Let&#39;s Get Started :**

Before using Rekognition, we would create a user which we will use .

You need to have images stored in a location that is accessible by Rekognition, generally in the cloud . We will create a s3 bucket, our bucket name is &#39;rekoblogbucket&#39;.

You can consider the below code for inserting image in the S3 bucket using python.

|
import boto3
s3\_client = boto3.client(&#39;s3&#39;,aws\_access\_key\_id = access\_key\_id,aws\_secret\_access\_key = secret\_access\_key)
s3\_client.upload\_file(&#39;rekog\_image.jpg&#39;, &#39;rekoblogbucket&#39;, &#39;rekog\_image.jpg&#39;)
 |
| --- |

Below is the image which we will be using in this blog :

![](RackMultipart20211101-4-1bp2u7w_html_a0f361cd51f17525.jpg)

The images are from Unsplash and can be found respectively at :

[https://unsplash.com/photos/dGk-qYBk4OA?utm\_source=unsplash&amp;utm\_medium=referral&amp;utm\_content=creditShareLink](https://unsplash.com/photos/dGk-qYBk4OA?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditShareLink)

**Detecting text :**

The first step would be to create a client for using AWS Rekoginition.

|
boto3.setup\_default\_session(region\_name=&quot;us-east-1&quot;)
client = boto3.client(&#39;rekognition&#39;,aws\_access\_key\_id = access\_key\_id,aws\_secret\_access\_key = secret\_access\_key )
 |
| --- |

Next we will be creating a dictionary which will represent the location of the image in S3.To detect text, call the detect\_text method and just the Image keyword argument.

| response=client.detect\_text(Image={&#39;S3Object&#39;:{&#39;Bucket&#39;:&#39;rekoblogbucket&#39;,&#39;Name&#39;:&#39;rekog\_image.jpg&#39;}})
 |
| --- |

The detected text is in the TextDetections key and each detection has a DetectedText key.

|
for detection in text[&#39;TextDetections&#39;]:print(detection[&#39;DetectedText&#39;])
 |
| --- |

**Output:-**

![](RackMultipart20211101-4-1bp2u7w_html_ed9ea3f1b5daca8e.png)

In the above output you can see that the text were detected twice .

It might look as if the text were detected twice. But notice that in the image the words &quot;DO&quot; and &quot;NOT&quot; are on the same line. The first three detections are lines of text. The remaining detections are for words . Below explanation will explain this concept in brief.

A line is a string of equally spaced words. A line isn&#39;t necessarily a complete sentence. For example, a driver&#39;s license number is detected as a line. A line ends when there is no aligned text after it. Also, a line ends when there&#39;s a large gap between words, relative to the length of the words. Depending on the gap between words, this means that Amazon Rekognition might detect multiple lines in text that are aligned in the same direction. Periods don&#39;t represent the end of a line. If a sentence spans multiple lines, the operation returns multiple lines.

Similarly, for videos, using the StartTextDetection and GetTextDetection APIs, you can detect text and get confidence scores and timestamps for each detection. In media and entertainment applications, you can create text metadata to support search for relevant content, such as news, sport scores, commercials, and captions. You can also review the detected text for policy or compliance violations e.g. an email address or phone number that has been overlaid by spammers.

**Conclusion :**

Amazon Rekognition makes it easy to add image and video analysis to your applications. You just need to provide an image or video to the Rekognition API, and the service to detect objects, faces, text and more in both still images and videos. AWS Rekognition is simple, easy, quick and cost-effective , developers only have to pay for the quantity of images that they analyze.You don&#39;t need to know anything about computer or machine learning. All you need to know is how to use the API for the client libraries.
