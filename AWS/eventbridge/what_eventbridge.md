**Event Bridge – what**

**What is Eventbridge?**

![](RackMultipart20211101-4-w37y1r_html_bc211b920de7b911.png)

EventBridge helps developers build universal, reliable and fully event-driven applications. EventBridge is a serverless pub / sub service which makes it possible to seamlessly connect different event sources with AWS cloud services via event buses. Event publishers can be any (e.g. legacy) application, SaaS providers or internal AWS services. Event consumers can range from EC2 instances, to other event buses to Lambda functions. Given this simplicity, yet flexibility it&#39;s easy to see why other developers call this announcement one of the most important services for serverless application development.

Amazon EventBridge is a serverless event bus that makes it easy to connect applications together using data from your own applications, integrated Software-as-a-Service (SaaS) applications, and AWS services. EventBridge delivers a stream of real-time data from event sources, such as Zendesk, Datadog, or Pagerduty, and routes that data to targets like AWS Lambda.

EventBridge makes it easy to build event-driven applications because it takes care of event ingestion and delivery, security, authorization, and error handling for you. As your applications become more interconnected through events, you need to spend more effort to find events and understand their structure in order to write code to react to those events.

You can set up routing rules to determine where to send your data to build application architectures that react in real time to all of your data sources.

The Amazon EventBridge schema registry stores event structure - or schema - in a shared central location and maps those schemas to code for Java, Python, and Typescript so it&#39;s easy to use events as objects in your code. You can connect to and interact with the schema registry from the AWS Management Console, APIs, or the SDK Toolkits for Jetbrains (Intellij, PyCharm, Webstorm, Rider) and VS Code.

This improves developer agility as well as application resiliency. EventBridge makes it easy to build event-driven applications because it takes care of event ingestion and delivery, security, authorization, and error-handling for you.

EventBridge leverages the CloudWatch Events API, so CloudWatch Events users can access their existing default bus, rules, and events in the new EventBridge console, as well as in the CloudWatch Events console.

**What are events?**

Events in Amazon EventBridge are represented as JSON objects. All events have a similar structure, and the same top-level fields.

Below is the example of the event.They all have the same top-level fields – the ones appearing in the example above – which are never absent.

The contents of the  **detail**  top-level field are different depending on which service generated the event and what the event is. The combination of the  **source**  and  **detail-type**  fields serves to identify the fields and values found in the  **detail**  field.

| {&quot;version&quot;: &quot;0&quot;,&quot;id&quot;: &quot;6a7e8feb-b491-4cf7-a9f1-bf3703467718&quot;,&quot;detail-type&quot;: &quot;EC2 Instance State-change Notification&quot;,&quot;source&quot;: &quot;aws.ec2&quot;,&quot;account&quot;: &quot;111122223333&quot;,&quot;time&quot;: &quot;2017-12-22T18:43:48Z&quot;,&quot;region&quot;: &quot;us-west-1&quot;,&quot;resources&quot;: [&quot;arn:aws:ec2:us-west-1:123456789012:instance/i-1234567890abcdef0&quot;],&quot;detail&quot;: {&quot;instance-id&quot;: &quot; i-1234567890abcdef0&quot;,&quot;state&quot;: &quot;terminated&quot;}} |
| --- |

Each event field is described below.

**version**

By default, this is set to 0 (zero) in all events.

**id**

A unique value is generated for every event. This can be helpful in tracing events as they move through rules to targets, and are processed.

**detail-type**

Identifies, in combination with the source field, the fields and values that appear in the detail field.

All events that are delivered via CloudTrail have AWS API Call via CloudTrail as the value for detail-type. For more information, see Events Delivered Via CloudTrail.

**source**

Identifies the service that sourced the event. All events sourced from within AWS begin with &quot;aws.&quot; Customer-generated events can have any value here, as long as it doesn&#39;t begin with &quot;aws.&quot; We recommend the use of Java package-name style reverse domain-name strings.

To find the correct value for source for an AWS service, see the table in AWS Service Namespaces. For example, the source value for Amazon CloudFront is aws.cloudfront.

**account**

The 12-digit number identifying an AWS account.

**time**

The event timestamp, which can be specified by the service originating the event. If the event spans a time interval, the service might choose to report the start time, so this value can be noticeably before the time the event is actually received.

**region**

Identifies the AWS region where the event originated.

**resources**

This JSON array contains ARNs that identify resources that are involved in the event. Inclusion of these ARNs is at the discretion of the service. For example, Amazon EC2 instance state-changes include Amazon EC2 instance ARNs, Auto Scaling events include ARNs for both instances and Auto Scaling groups, but API calls with AWS CloudTrail do not include resource ARNs.

**detail**

A JSON object, whose content is at the discretion of the service originating the event. The detail content in the example above is very simple, just two fields. AWS API call events have detail objects with around 50 fields nested several levels deep.

**What is event rules?**

EventBridge rules use event patterns to match on AWS events on an event bus. When a pattern matches, the rule routes that event to a target.

**Event Pattern:-**

Event patterns have the same structure as the Events they match. They look much like the events they are filtering. Rules use event patterns to select events and route them to targets. A pattern either matches an event or it doesn&#39;t. The following is an example of a simple AWS Event which you might encounter on EventBridge.

| {&quot;version&quot;: &quot;0&quot;,&quot;id&quot;: &quot;6a7e8feb-b491-4cf7-a9f1-bf3703467718&quot;,&quot;detail-type&quot;: &quot;EC2 Instance State-change Notification&quot;,&quot;source&quot;: &quot;aws.ec2&quot;,&quot;account&quot;: &quot;111122223333&quot;,&quot;time&quot;: &quot;2017-12-22T18:43:48Z&quot;,&quot;region&quot;: &quot;us-west-1&quot;,&quot;resources&quot;: [&quot;arn:aws:ec2:us-west-1:123456789012:instance/i-1234567890abcdef0&quot;],&quot;detail&quot;: {&quot;instance-id&quot;: &quot;i-1234567890abcdef0&quot;,&quot;state&quot;: &quot;terminated&quot;}} |
| --- |

Event patterns have the same structure as the events they match. For example, the following event pattern allows you to subscribe to only events from Amazon EC2.

| {&quot;source&quot;: [&quot;aws.ec2&quot;]} |
| --- |

The pattern simply quotes the fields you want to match and provides the values you are looking for.

The sample event above, like most events, has a nested structure. Suppose you want to process all instance-termination events. Create an event pattern like the following.

| {&quot;source&quot;: [&quot;aws.ec2&quot;],&quot;detail-type&quot;: [&quot;EC2 Instance State-change Notification&quot;],&quot;detail&quot;: {&quot;state&quot;: [&quot;terminated&quot;]}} |
| --- |

Specify Fields to Match

Only specify fields that you care about. In the previous example, you only provide values for three fields: The top-level fields &quot;source&quot; and &quot;detail-type&quot;, and the &quot;state&quot; field inside the &quot;detail&quot; object field. EventBridge ignores all the other fields in the event while applying the filter.

Match Values

Match values are always in arrays. Note that the value to match is in a JSON array, surrounded by &quot;[&quot; and &quot;]&quot;. This is so you can provide multiple values. For example, if you were interested in events from Amazon EC2 or Fargate, you could specify the following.

| {&quot;source&quot;: [&quot;aws.ec2&quot;, &quot;aws.fargate&quot;]} |
| --- |

This matches on events where the value for the &quot;source&quot; field is either &quot;aws.ec2&quot; or &quot;aws.fargate&quot;.

Match on All JSON Data Types

You can match on all of the JSON data types. Consider the following example Amazon EC2 Auto Scaling event.

| {&quot;version&quot;: &quot;0&quot;,&quot;id&quot;: &quot;3e3c153a-8339-4e30-8c35-687ebef853fe&quot;,&quot;detail-type&quot;: &quot;EC2 Instance Launch Successful&quot;,&quot;source&quot;: &quot;aws.autoscaling&quot;,&quot;account&quot;: &quot;123456789012&quot;,&quot;time&quot;: &quot;2015-11-11T21:31:47Z&quot;,&quot;region&quot;: &quot;us-east-1&quot;,&quot;resources&quot;: [],&quot;detail&quot;: {&quot;eventVersion&quot;: &quot;&quot;,&quot;responseElements&quot;: null}}
 |
| --- |

For the above example, you can match on the &quot;responseElements&quot; field as follows.

| {&quot;source&quot;: [&quot;aws.autoscaling&quot;],&quot;detail-type&quot;: [&quot;EC2 Instance Launch Successful&quot;],&quot;detail&quot;: {&quot;responseElements&quot;: [null]}}
 |
| --- |

This works for numbers too. Consider the following Amazon Macie Classic event (truncated for brevity).

| {&quot;version&quot;: &quot;0&quot;,&quot;id&quot;: &quot;3e355723-fca9-4de3-9fd7-154c289d6b59&quot;,&quot;detail-type&quot;: &quot;Macie Alert&quot;,&quot;source&quot;: &quot;aws.macie&quot;,&quot;account&quot;: &quot;123456789012&quot;,&quot;time&quot;: &quot;2017-04-24T22:28:49Z&quot;,&quot;region&quot;: &quot;us-east-1&quot;,&quot;resources&quot;: [&quot;arn:aws:macie:us-east-1:123456789012:trigger/trigger\_id/alert/alert\_id&quot;,&quot;arn:aws:macie:us-east-1:123456789012:trigger/trigger\_id&quot;],&quot;detail&quot;: {&quot;notification-type&quot;: &quot;ALERT\_CREATED&quot;,&quot;name&quot;: &quot;Scanning bucket policies&quot;,&quot;tags&quot;: [&quot;Custom\_Alert&quot;,&quot;Insider&quot;],&quot;url&quot;: &quot;https://lb00.us-east-1.macie.aws.amazon.com/111122223333/posts/alert\_id&quot;,&quot;alert-arn&quot;: &quot;arn:aws:macie:us-east-1:123456789012:trigger/trigger\_id/alert/alert\_&quot;risk-score&quot;: 80,&quot;trigger&quot;: {&quot;rule-arn&quot;: &quot;arn:aws:macie:us-east-1:123456789012:trigger/trigger\_id&quot;,&quot;alert-type&quot;: &quot;basic&quot;,&quot;created-at&quot;: &quot;2017-01-02 19:54:00.644000&quot;,&quot;description&quot;: &quot;Alerting on failed enumeration of large number of bucket policie&quot;risk&quot;: 8},&quot;created-at&quot;: &quot;2017-04-18T00:21:12.059000&quot;,...
 |
| --- |

If you want to match anything that has a risk score of 80 and a trigger risk of 8, do the following.

| {&quot;source&quot;: [&quot;aws.macie&quot;],&quot;detail-type&quot;: [&quot;Macie Alert&quot;],&quot;detail&quot;: {&quot;risk-score&quot;: [80],&quot;trigger&quot;: {&quot;risk&quot;: [8]}}} |
| --- |

Simple Matching with Event Patterns

The following example event is used to show how the subsequent event patterns would match with this event JSON.

| {&quot;version&quot;: &quot;0&quot;,&quot;id&quot;: &quot;6a7e8feb-b491-4cf7-a9f1-bf3703467718&quot;,&quot;detail-type&quot;: &quot;EC2 Instance State-change Notification&quot;,&quot;source&quot;: &quot;aws.ec2&quot;,&quot;account&quot;: &quot;111122223333&quot;,&quot;time&quot;: &quot;2017-12-22T18:43:48Z&quot;,&quot;region&quot;: &quot;us-west-1&quot;,&quot;resources&quot;: [&quot;arn:aws:ec2:us-west-1:123456789012:instance/ i-1234567890abcdef0&quot;],&quot;detail&quot;: {&quot;instance-id&quot;: &quot; i-1234567890abcdef0&quot;,&quot;state&quot;: &quot;terminated&quot;}} |
| --- |

Event patterns are represented as JSON objects with a structure that is similar to that of events, for example:

| {&quot;source&quot;: [&quot;aws.ec2&quot;],&quot;detail-type&quot;: [&quot;EC2 Instance State-change Notification&quot;],&quot;detail&quot;: {&quot;state&quot;: [&quot;running&quot;]}} |
| --- |

This event pattern would not match on the example event, as the value of the &quot;state&quot; field matches on &quot;running&quot;, but the value in the example event is &quot;terminated&quot;.

It is important to remember the following about event pattern matching:

For a pattern to match an event, the event must contain all the field names listed in the pattern. The field names must appear in the event with the same nesting structure.

Other fields of the event not mentioned in the pattern are ignored; effectively, there is a &quot;\*&quot;: &quot;\*&quot; wildcard for fields not mentioned.

The matching is exact (character-by-character), without case-folding or any other string normalization.

The values being matched follow JSON rules: Strings enclosed in quotes, numbers, and the unquoted keywords true, false, and null.

Number matching is at the string representation level. For example, 300, 300.0, and 3.0e2 are not considered equal.

When you write patterns to match events, you can use the TestEventPattern API or the test-event-pattern CLI command to make sure that your pattern will match the desired events. For more information, see TestEventPattern or test-event-pattern.

The following event patterns would match the previous example event. The first pattern matches because one of the instance values specified in the pattern matches the event (and the pattern does not specify any additional fields not contained in the event). The second one matches because the &quot;terminated&quot; state is contained in the event.

| {&quot;resources&quot;: [&quot;arn:aws:ec2:us-east-1:123456789012:instance/i-12345678&quot;,&quot;arn:aws:ec2:us-east-1:123456789012:instance/i-abcdefgh&quot;]}{&quot;detail&quot;: {&quot;state&quot;: [&quot;terminated&quot;]}}
 |
| --- |

These event patterns do not match the event at the top of this page. The first pattern does not match because the pattern specifies a &quot;pending&quot; value for state, and this value does not appear in the event. The second pattern does not match because the resource value specified in the pattern does not appear in the event.

| {&quot;source&quot;: [&quot;aws.ec2&quot;],&quot;detail-type&quot;: [&quot;EC2 Instance State-change Notification&quot;],&quot;detail&quot;: {&quot;state&quot;: [&quot;pending&quot;]}}{&quot;source&quot;: [&quot;aws.ec2&quot;],&quot;detail-type&quot;: [&quot;EC2 Instance State-change Notification&quot;],&quot;resources&quot;: [&quot;arn:aws:ec2:us-east-1::image/ami-12345678&quot;]}
 |
| --- |

**Conclusion**

EventBridge is the evolution of the CloudWatch Events service. It brings new features, including the ability to integrate data from popular SaaS providers as events within AWS. In this post I have discussed the Event bridge and the modules present in the event bridge. You can have easy understanding when you need to use eventbridge and some examples for the events and event patterns. New event-based features are now released in EventBridge. By migrating from CloudWatch Events, you can take advantage of new capabilities as they are released.
