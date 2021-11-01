![](RackMultipart20211101-4-5tipb8_html_cfd7229cc3d146b7.png)

**Introduction:-**

AWS Rekognition was released in November 2016 as the first cloud image analysis service by Amazon. The purpose of AWS Rekognition is to analyze images and predict what objects are in the image, if there are any faces or any transcribed text. But to use Rekognition you need to know little, if anything, about machine learning at all! Amazon Rekognition includes a simple, easy-to-use API that can quickly analyze any stored image or video.

Amazon Rekognition is a service that makes it easy to add powerful visual analysis to your applications. Rekognition Image lets you easily build powerful applications to search, verify, and organize millions of images.

You simply point Rekognition to any image or video file that&#39;s stored in Amazon S3 and tell it what computer vision task you want and get the results back.

**Overview of face detection and face comparison :-**

1. **Face Detection :-**

Amazon Rekognition can detect faces in images and videos. It provides a set of face detection, analysis, and recognition features for image and video analysis.

Face analysis generates rich metadata about detected faces in the form of gender, age range, emotions(for all 7 supported emotions: &#39;Happy&#39;, &#39;Sad&#39;, &#39;Angry&#39;, &#39;Surprised&#39;, &#39;Disgusted&#39;, &#39;Calm&#39; and &#39;Confused&#39;), attributes such as &#39;Smile&#39;, &#39;Eyeglasses&#39; ,&#39; EyesOpen&#39; and even &#39;Beard&#39;, face pose, face image quality and face landmarks.

It is useful for customers who need to search and categorize large photo collections. For example, using face analysis, customers can easily find all photos that contain smiling people, or all photos with men who have beards and are wearing sunglasses&quot;.

The face detection system can provide an estimate of the confidence level of the prediction in the form of a probability or confidence score. For example, face detection system may predict that an image region is a face at a confidence score of 90%, and another image region is a face at a confidence score of 60%. The region with the higher confidence score should be more likely to contain a face. If a face detection system does not properly detect a face, or provides a low confidence prediction of an actual face, this is known as a missed detection or false negative.

1. **Face Comparison:-**

A face comparison system is designed to answer the question: does the face in an image match the face in another image? A face comparison system takes an image of a face and makes a prediction about whether the face matches other faces in a provided database. Face comparison systems are designed to compare and predict potential matches of faces regardless of their expression, facial hair, and age.

These systems make predictions of whether a face exists in an image or matches a face in another image, with a corresponding level of confidence in the prediction. Users of these systems should consider the confidence score/similarity threshold provided by the system when designing their application and making decisions based on the output of the system.

The Amazon Rekognition face analysis models are available for both Amazon Rekognition Image and Video.

Below are some of the leading organizations worldwide who are using Amazon Rekognition to add image and video analysis to their applications.

| ![](RackMultipart20211101-4-5tipb8_html_18b815cf9b5104f1.png) ![](RackMultipart20211101-4-5tipb8_html_dc022bf8ad5c7962.png) ![](RackMultipart20211101-4-5tipb8_html_6d462036542a6d7d.png) **NFL Media Marinus Analytics Aella Credit** |
| --- |

**Following are some important methods/operations used for detecting face** :

1. **DetectFaces :-**

This method detects faces within an image that is provided as input.

DetectFaces detects the 100 largest faces in the image. For each face detected, the operation returns face details. These details include a bounding box of the face, a confidence value (that the bounding box contains a face), and a fixed set of attributes such as facial landmarks (for example, coordinates of eye and mouth), presence of beard, sunglasses, and so on.

The face-detection algorithm is most effective on frontal faces. For non-frontal or obscured faces, the algorithm might not detect the faces or might detect faces with lower confidence.

You pass the input image either as base64-encoded image bytes or as a reference to an image in an Amazon S3 bucket. If you use the AWS CLI to call Amazon Rekognition operations, passing image bytes is not supported. The image must be either a PNG or JPEG formatted file.

1. **CompareFaces:-**

To compare a face in the source image with each face in the target image, we use the CompareFaces operation.

Rekognition can compare faces between two pictures, and establish with some confidence the similarity between them. Using Boto 3&#39;s compare\_faces method, we provide a source and a target image for analysis.

You pass the input and target images either as base64-encoded image bytes or as references to images in an Amazon S3 bucket. If you use the AWS CLI to call Amazon Rekognition operations, passing image bytes isn&#39;t supported. The image must be formatted as a PNG or JPEG file.

In response, the operation returns an array of face matches ordered by similarity score in descending order. For each face match, the response provides a bounding box of the face, facial landmarks, pose details (pitch, role, and yaw), quality (brightness and sharpness), and confidence value (indicating the level of confidence that the bounding box contains a face). The response also provides a similarity score, which indicates how closely the faces match.

1.
# **Facial Recognition**

Usually it is recommended to access Rekognition and other AWS products through a client library. Many different AWS client libraries support different languages. Here we&#39;ll be using Python and the boto3 package .

- Boto is the Amazon Web Services (AWS) SDK for Python. It enables Python developers to create, configure, and manage AWS services, such as EC2 and S3.

**Let&#39;s Get Started :**

Before using Rekognition, we would create a user which we will use .

You need to have images stored in a location that is accessible by Rekognition, generally in the cloud . We will create a s3 bucket, our bucket name is &#39;rekoblogbucket&#39;.

You can consider the below code for inserting image in the S3 bucket using python.

| import boto3

 s3\_client = boto3.client(&#39;s3&#39;,aws\_access\_key\_id = access\_key\_id,aws\_secret\_access\_key = secret\_access\_key)

 s3\_client.upload\_file(&#39; test\_source\_image.jpg&#39;, &#39;rekoblogbucket&#39;, &#39; test\_source\_image.jpg&#39;) |
| --- |

**Detecting face in an image:**

Below is the image which we will be using for face detection:

![](RackMultipart20211101-4-5tipb8_html_a29b5501d643ee66.jpg)

The first step would be to create a client for using AWS Rekoginition.

| boto3.setup\_default\_session(region\_name=&quot;us-east-1&quot;)

 client = boto3.client(&#39;rekognition&#39;,aws\_access\_key\_id = access\_key\_id,aws\_secret\_access\_key = secret\_access\_key ) |
| --- |

Next we will call the detect\_faces method and pass a dictionary to the Image keyword argument. The dictionary will represent the location of the image in our S3 bucket .The Attributes keyword argument is a list of different features to detect, such as gender and emotion. In the below provided code snippet, we&#39;ll pass a single value &#39; ALL&#39; to get all of the attributes.

| response=client.detect\_faces(Image={&#39;S3Object&#39;:{&#39;Bucket&#39;:&#39;rekoblogbucket&#39;,&#39;Name&#39;:&#39; test\_source\_image.jpg&#39;}},Attributes=[&#39;ALL&#39;]) |
| --- |

The faces are stored in the FaceDetails key. There is only one face in this image. The keys of the face are the attributes.

| for faceDetail in response[&#39;FaceDetails&#39;]:
 print(&#39;Age of the detected face is between &#39; +
 str(faceDetail[&#39;AgeRange&#39;]))
 print(&#39;Gender &#39; + str(faceDetail[&#39;Gender&#39;]))
 print(&#39;EyesOpen &#39; + str(faceDetail[&#39;EyesOpen&#39;]))
 print(&#39;MouthOpen &#39; + str(faceDetail[&#39;MouthOpen&#39;]))
 print(&#39;Emotions &#39; + str(faceDetail[&#39;Emotions&#39;])) |
| --- |

**Output:-**

| Age of the detected face is between {&#39;Low&#39;: 38, &#39;High&#39;: 56}

 Gender {&#39;Value&#39;: &#39;Male&#39;, &#39;Confidence&#39;: 99.64933013916016}

 EyesOpen {&#39;Value&#39;: True, &#39;Confidence&#39;: 99.56517028808594}

 MouthOpen {&#39;Value&#39;: False, &#39;Confidence&#39;: 90.71836853027344}

 Emotions [
 {&#39;Type&#39;: &#39;CALM&#39;, &#39;Confidence&#39;: 57.12054443359375},
 {&#39;Type&#39;: &#39;HAPPY&#39;, &#39;Confidence&#39;: 39.588871002197266},
 {&#39;Type&#39;: &#39;SURPRISED&#39;, &#39;Confidence&#39;: 1.262111783027649},
 {&#39;Type&#39;: &#39;CONFUSED&#39;, &#39;Confidence&#39;: 0.9066788554191589},
 {&#39;Type&#39;: &#39;DISGUSTED&#39;, &#39;Confidence&#39;: 0.48998212814331055},
 {&#39;Type&#39;: &#39;ANGRY&#39;, &#39;Confidence&#39;: 0.25930991768836975},
 {&#39;Type&#39;: &#39;SAD&#39;, &#39;Confidence&#39;: 0.2578431963920593},
 {&#39;Type&#39;: &#39;FEAR&#39;, &#39;Confidence&#39;: 0.11466632038354874}] |
| --- |

Here in the attributes, we can see that the Rekognition predicted that the Age Range of the provided image is between 38-56, the Gender as Male with more than 99% confidence, and that the person&#39;s eyes were open but her mouth is not. It also predicted the person&#39;s emotions and with most likely emotion as CALM.

**Comparing faces:**

Below are two images which we will be using for comparing faces:

![](RackMultipart20211101-4-5tipb8_html_71fb166b485c7457.jpg) ![](RackMultipart20211101-4-5tipb8_html_58af40d4379c2ea0.jpg)

We have already mentioned how to upload an image to the S3 bucket , so you can follow the step and upload a second image which we will use for comparing faces.

| response2 = client.compare\_faces(
 SourceImage={
&#39;S3Object&#39;: {
&#39;Bucket&#39;: &quot;rekoblogbucket&quot;,
&#39;Name&#39;: &quot;test\_source\_image.jpg&quot;
 }
 },
 TargetImage={
&#39;S3Object&#39;: {
&#39;Bucket&#39;: &quot;rekoblogbucket&quot;,
&#39;Name&#39;: &quot;test\_target\_image.jpg&quot;
 }
 }
 )

print (response2[&#39;FaceMatches&#39;][0][&#39;Similarity&#39;]) |
| --- |

**Output:-**

| 99.99931335449219 |
| --- |

Returning a similarity score of 99.99%, Rekognition is positive that these two faces are the same.

**Note:** The face detection models used by Amazon Rekognition Image and Amazon Rekognition Video don&#39;t support the detection of faces in cartoon/animated characters or non-human entities. If you want to detect cartoon characters in images or videos, we recommend using Amazon Rekognition Custom Labels.

**Conclusion** :-

Amazon Rekognition makes it easy to add image and video analysis to your applications. You just need to provide an image or video to the Rekognition API, and the service to detect objects, faces, text and more in both still images and videos. Amazon Rekognition is a service that offers handy features that can be easily used.Amazon Rekognition solves the problem of identifying or verifying people in photographs and videos. It is a process comprised of detection, alignment, feature extraction, and a recognition task

And for using all of these functionalities you just need to know how to use the API for the client libraries. For this blog we used Python. There are other client libraries for popular languages. Thanks for reading!!!
