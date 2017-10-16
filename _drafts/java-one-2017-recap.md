---
layout: post
title: JavaOne 2017 Recap
description: "In the first week of October 2017 I got a chance to join the CUBA Platform team and participate in the JavaOne conference, taking place in San Francisco. In this blog post I'll make a little recap of what I have learned during these days."
modified: 2017-04-16
tags: [cuba platform, java, java-one]
image:
  feature: javaone-2017-recap/feature.jpg
---

In the first week of October 2017 I got a chance to join the CUBA Platform team and participate in the JavaOne conference, taking place in San Francisco. In this blog post I'll make a little recap of what I have learned during these days.

<!-- more -->

## Talks All Over the Place

First of all I would like to note that is was impossible to attend even one-third of the talks, as they were split by over than 10 parallel tracks. Choosing between the sessions also wasn't an easy task, as it's not easy to choose between useful and interesting. However, I managed to attend quite a lot of them. Must say that sometimes (not often, but still...) my expectations from the talks I attended were overestimated, so I will not mention them in this article, keeping space only for the best ones.

#### Java Keynote: All about openness

Starting with the first day, there was [Java Keynote](https://www.oracle.com/javaone/on-demand.html?bcid=5596229112001), led by Mark Cavage ([@mcavage](https://twitter.com/mcavage)) and Mark Reinhold ([@mreinhold](https://twitter.com/mreinhold)).

The Keynote was all about the openness of the Java platform. They talked about their recent announcement of moving JavaEE to the Eclipse foundation and what it means in terms of openness. Then they announced that they wanted to change their release cycles dramatically from feature-based releases with timeframes of 3-4 years to times-based releases every 6 months.

Next they talked about the Jigsaw project as a part of JDK9. Since this is the major feature of Java 9, there has been written a lot before about it. Being able to define modules in Java seems to be a reasonable approach and is also the missing piece that other languages have had for ages. Since the major libraries also need to support the Java 9 module system, time will tell how this works out.

The last notable thing is called Project Amber. This is an effort that has been around for quite some time. It deals with increasing the features of the Java language itself in order to make it a little better step by step.

Within Project Amber there are subprojects that deal with certain parts. One is to introduce pattern matching into Java, which is very cool. Next thing is the automatic type inference within a function. This would allow to use the keyword <code>var</code> instead of defining the type. Not to confuse with dynamic typing. It is just that the Java compiler will infer the type of the variable from the right side of the assignment.

So, these are the great feature enhancements. But they are not part of Java 9. Instead, they will be part of ext releases of the JDK. However, combined with the new release schedule, it should out to users much faster. The <code>var</code> thing e.g. will be part of the next (18.03) release in March 2018.


#### A Competitive Food Retail Architecture with Microservices - REWE

Next up was an interesting and practical talk from two german software developers working for a major retail company called REWE: Sebastian Gauder ([@rattakresch](https://twitter.com/rattakresch)) and Ansgar Brauner ([@a_brauner](https://twitter.com/a_brauner)).

It was a "monolith to microservices" story as it has been told a few times, but accompanied with a practical showcase it is always interesting.

One of the main things was "Asnychronous > Synchronous". So wherever it may be possible, async communication should be preferred, because it will highly decouple the elements in the architecture.

With this there comes the second notable takeaway: "Having data is much better than fetching data".
This basically means that it is totally ok to duplicate data in the MS based architecture. It is not perfect either, but making distributed sychronous network calls is even worse.

They use Apache Kafka for message based communication. Additionally, the receivers of the message only take data of the message it requires. This is somewhat related to the idea of bounded context in the domain-driven design.

Interestingly they said that they decided to not go the event sourcing route, but instead hold the latest state in the messages.

The last quite interesting thing in the microservices architecture is the question on how the UI integration works. There are different approaches to this. REWE decided to create a thing, they called "UI-gateway", which aggregates the UI (HTML+CSS+JavaScript) from Microservices and pushes them to the web client. This means that the microservices have to agree on a particular UI technology (and a version of it too) like React v.15.0. That has some obvious drawbacks, but it was interesting to see that sometimes as an architectural trade-off a decision like this is being made.

The slides for this interesting talk can be found at [Speakerdeck](https://speakerdeck.com/abrauner/javaone-2017-a-competitive-food-retail-architecture-with-microservices).

#### You Deserve Great Tools: Commit-to-Production Automation at LinkedIn

Another interesting talk was from the the creator of the widely used Mockito library. He talked about how they applied "continuous delivery" in the development of the open source library. They automated everything so that every pull request creates a release. This enforces developers that every pull request has to contain everything:

- code
- tests
- documentation

After that, we made the switch and talked about the continuous-delivery process they approached at LinkedIn.
Mainly they used a so-called 3x3 approach: 3 releases per day & max 3 hours to release.

Before that, they were doing 1x release per month. Switching to 3x3 removes the "feature-rush" problem. A feature rush is usually the situation that a few days before the release the amount of commits go up drastically, to get it into the release.

Besides the often heard problems and solutions to CD, a very neat thing that resonated with me is their approch to flaky tests:

In order to identify flaky tests, LinkedIn runs their functional test-suite 1000x at night. Then they count the flaky ones. In the morning, they remove the flaky ones from the suite and fix them before putting them back in.


#### Developer Keynote - Promises and DevOps

The next day, there was another keynote where besides the obvious Oracle ceremony it included a talk from  Patick Debois, the person who coined the term DevOps.

His talk was dealing with the fact that a lot of companies use services wherever they can. He called it "ServiceFull". Besides the obvious benefits of such approach, like concentrating on the core business, lower costs etc., there are some heavy drawbacks on this approach as well.

To explain this fact, he went into [Promise theory](http://a.co/imya0c4) in order to discusss the idea of a promise, which stands as a fundamental building block. You shoud only create promises for your service only on what you can control.

Spinning back to the ServiceFull idea, he said that one should try to eliminate a single point of failures even when using services (using OpenFaaS instead of Lambda). It's all about the choice, in order to keep your promise.

Taking DevOps into the picture, he said that the problem with use of services is that the idea of DevOps does not hold when using external services very much. This is because in DevOps it is all about getting the different people back together at a single table to communicate across boundaries. But this doesn't really work when outsourcing everything to services. Actually, then there are even bigger boundaries. But this is exactly what DevOps tries to prevent.

He pointed out some solutions in these ServiceFull spaces:

External services need to try to open their boundaries so that you can hold your promise. It is about how do they communicate with problems etc. Some possible options to opening up are:

- post-mortems
- changelog
- open sourcing
- direct access to engineers

He closes with the idea that using DevOps in the ServiceFull space means DevOps across service / company boundaries.


#### How Languages Influence Each Other: Reflections on 14 Years of Apache Groovy

Guillaume Laforge [@glaforge](https://twitter.com/glaforge), one of the core maintainers, gave an interesting talk about Groovy. It was mainly going through the different syntactical features of the languages and talk about what other languages influenced Groovy or what other languages were influenced by Groovy.

Besides that, there were some little tricks I wasn't really aware of, so I'll just mention them here:

- named parameters can be used not only for constructors but for regular method calls as well:

{% highlight groovy %}

def rectangle(Map m, Color r) {
  println "$m.width:$m.height $r"
}

rectangle red, width: 100, height: 200

{% endhighlight %}

- <code>@Immutable</code> Annotation to create immutable classes
- type aliases through naming imports: <code>import com.company.project.ClassName as CN</code>


#### Testing Containers with TestContainers: There and Back Again

Another very interesting talk was about how to use Docker for integration testing.
There is a library called [TestContainers](https://www.testcontainers.org/) which allows the user to create Docker containers in their JUnit based integration tests.

This is super useful, if you have other services as dependencies of your application. Those are normally hard to create manually, but through Docker it becomes a breeze. An example of this would be a Kafka message broker. Your application uses Kafka for async communication, and in your test you want to check if your system successfully creates messages if a customer is created e.g.

Normally because starting and stopping a Kafka cluster is pretty hard, there will be something like a single shared resource that is used throughout the test-suite. But this has some limitations. What about testing failure scenarios of the messaging system? What about concurrent running tests?

Testcontainers solves this problem with the help of Docker. It acts as a JUnit Rule that creates some containers through a nice DSL.


## Chatting with the vendors

Besides the talks that were quite informative, I had the chance to have a deep chat with some of the software vendors that had their booths in this pretty big exhibition. Most of the time I spend with [CUBA Platform](https://www.cuba-platform.com/) developers (for obvious reasons). We talked about different things regarding the framework.

The first notable thing was the whole messaging infrastructure they will provide in CUBA 6.7. It's about backend messages, as well as UI events, which allow greater flexibility and looser coupling between the moving parts of the application.

Next they talked about plans to change the way UI controllers are supposed to work. Currently it is required to extend some base class (like <code>AbstractEditor</code>). Then there are certain lifecycle hook methods that can be overridden to put custom logic in place.

It is inspired by [Vaadin folks](https://vaadin.com/) who had a booth at JavaOne as well. They showed some early preview of the upcoming [Vaadin 10](https://vaadin.com/blog/vaadin-flow-the-next-piece-of-vaadin-10-is-now-in-developer-preview) and their way on how to glue together your controller logic with the actual UI like this:


{% highlight java %}

@Tag("my-label") // declare which HTML element to use, here a custom element <my-label>
public class Label extends Component {

 public void setText(String text) {
   getElement().setText(text);
 }
 public String getText() {
   return getElement().getText();
 }
}

{% endhighlight %}

CUBA thinks on switching from the base class approach to a more annotation driven approach where the developer just creates POJOs, and the annotation will create the binding to some XML e.g.

It was kind of interesting to see how these frameworks influence on each other and how they cherry-pick ideas in order to fulfill their customer needs best.

Another interesting vendor was [Datadog](https://www.datadoghq.com/). It was all about monitoring and analytics of running application. It was pretty impressive how easy it is to setup a monitoring system and what insides can be fetched form a running system.

Besides the raw data, it is only useful if the data can be consumed in a way that makes it easy to get some business value out. Seems that Datadog is doing a pretty good job on it.


There were a lot of other interesting talks and vendors that I did not take the time to participate in. However, this was my little impression of JavaOne 2017. It was a very subjective description, nevertheless I hope you enjoyed the recap.
