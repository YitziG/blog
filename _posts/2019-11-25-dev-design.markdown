---
title: Principles of Developer Focused Design
description:  
date: '2019-11-24T14:22:33.586Z'
permalink: dev-design
image: /img/download.png

---
#### Developers are human (usually)

To understand how to "encourage" developer adoption of an SDK, API, or any other developer-focused product, we need to understand what attracts them and what pushes them away.

> The only assumption you can make regarding your developer audience is that they are human beings, even that is just a heuristic.

Developers, like most humans, like things fast and easy. They want to:

1.  Find the info they need quickly.
2.  Be able to read and understand it quickly.
3.  Get it working rapidly with a minimum of hiccups.
4.  Feel that the technology they are integrating into their app is current and robust.

To that end I'd recommend:

**A. Setting up a developer site at an independent subdomain.**

 - Mixing developer docs with marketing content and other content typically present on a primary site is not a good look and makes it harder for developers to find what they need. 
 - Developers want to feel that they are learning and integrating relevant tech. The more they hang around on a marketing site to get the info they need, the more they feel like the tech is an afterthought.

**B. Not using Wordpress/Zendesk.**

*   Besides being cluttered and buggy, they can turn off developers because of their association with inferior and old tech.
*   I think a static site is perfect for documentation. Static websites are easy and cheap to build and maintain. Most are uncluttered by default, and they are blazing fast.
*   As an example, take a look at [Polly.js's documentation](https://netflix.github.io/pollyjs/#/quick-start).
*   [yitzi.dev](http://yitzi.dev) is another great static documentation site demonstrating a clean non-cluttered UI.

> If members have to scroll past large banners or find useful information among static content, that's bad. Minimize the static material to show the latest activity. Deliver the maximum value for the minimum amount of effort.
> -[Richard Millington](https://www.feverbee.com/community-design-principles)

**C. As much as possible, code samples and documentation should be current and consistent.**

*   In any case, it should be clear which one is the single "source of truth."
*   Another advantage of using a static site for documentation is that updating a version number in a data file can trigger a rebuild of the entire website in seconds, causing an update of that variable across the site.

**D. Instructions and Examples should be simple and relatable.**

*   Tell a story.
*   Developers won't be upset if you explain things clearly, but they will get angry if you make them feel dumb.
*   Help the developer understand the use case and motivation for the different features of your software.
*   See here for an [**article I wrote**](https://medium.com/@YitziG/how-i-became-an-indie-developer-b35ed0954b1a?source=friends_link&sk=f63a0b5dd367d14d253ac500df898b52) giving motivation for mobile attribution integration.

**E. Provide an IRC channel where developers can get help from other developers in real-time.**

*   There will always be issues, and developers need to feel like there is somewhere they can turn.
*   Discord is excellent, and Slack is OK, I guess.
*   At the start, there will need to be an investment of employee time. If done right as the community gets involved in the channel, company employees will be able to step back.
*   Before setting up your channel, see if there is already a developer community IRC where this channel would be appropriate. Going to your community is a lot easier and more productive than trying to drag them to you.
*   Every single page of the developer website should have a prominent link to the channel. It should never be more than one click away.

Developing software is frequently frustrating and almost always takes more time than the developer imagined it would. This is true for code newbies as well as for the most talented developers who are in love with coding. If you make life easier for developers, they will love you.

Simple, really.

Thanks for reading!
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg0Mzc2Nzk4NV19
-->
