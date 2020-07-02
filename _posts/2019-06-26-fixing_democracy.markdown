---
layout: post
title: Fixing Democracy, Developer Style
categories: open-source code law web-scraping
permalink: democracy
published: true

---

#### Submitting forms and scraping data for the public good

![](https://cdn-images-1.medium.com/max/1024/0*G1qzri4PoIbSsEh8)Photo by <a href="https://unsplash.com/@ushakov_kyryll?utm_source=medium&amp;utm_medium=referral">kyryll ushakov</a> on <a href="https://unsplash.com?utm_source=medium&amp;utm_medium=referral">Unsplash</a>

The US legal system is stacked against the poor.

The outcome of any court case is mainly dependent on the amount of money the parties involved can spend on their representation. Wealthy criminals who have harmed others with impunity get let off with a slap on the wrist or less. At the same time, the less financially fortunate can end up with outrageous sentences for the pettiest violations.

> One of the main drivers of this unjust situation is the cost of legal research. Two companies hold a duopoly on the U.S. market for legal information.

Law firms seeking access to precedent-setting legal opinions will need to pay Westlaw or Lexis upwards of $100 per month per license! See “[The Lexis/Westlaw Duopoly and the Proprietization of Legal Research](http://emoglen.law.columbia.edu/twiki/bin/view/LawNetSoc/ElliottPaper1)” — Columbia.

To fight this travesty [AnyLaw](http://anylaw.com) ([as seen on forbes.com](https://www.forbes.com/sites/maryjuetten/2019/01/22/free-legal-research-for-all-anylaw)) has built a robust, fully-featured, and most importantly, FREE legal search engine!

{% include youtubePlayer.html id="7y9QP-Sh4fU" %}

Cool, I know!

### How we do it

Because this was to be a free solution, we obviously could not have the overhead of licensing the published opinions annually.

Thankfully, US law requires every state and federal court to maintain a website and publish their legal opinions online.

The obvious solution, therefore, is to scrape and download the data from the individual court sites.

Unfortunately, there is no standard. Sites can be:

- static
- rendered server-side
- dynamic JavaScript sites

Courts are allowed to do as they please in terms of building their site.

> It pleases them to do some very interesting things.

For example have a look at the website of [Virginia’s Supreme Court](http://www.courts.state.va.us/scndex.htm):

![](https://cdn-images-1.medium.com/max/1024/1*SpsopEWjZp7ZV43-u9QHZQ.png)Yikes!!

Yup, that is every Supreme Court decision since 1995 on a single page. 😕

TLDR; Each court website needs a bot 😧

#### Case study: Texas

Texas is a great example because of the complexity of their site. This is what our bot is confronted with when it navigates to the [court system URL](http://www.search.txcourts.gov/CaseSearch.aspx?d=1&coa=cossup):

![](https://cdn-images-1.medium.com/max/1002/1*Voqr5WMG2rWWWykEw-M9Fw.png)a non-exceptional form

The first thing our bot needs to do is to get a list of all Supreme Court opinions. Since this bot needs to interact with the site, we chose to use [HtmlUnit](http://htmlunit.sourceforge.net/) by [Gargoyle Software Inc.](http://www.gargoylesoftware.com/)

Step 1. Install the HtmlUnit dependency. We place the following code snippet into our apps pom.xml. If you are not using Maven, visit the website for additional options.

{% gist c89b7492c6af69fe341c978aa88e6c08 %}

Step 2. Create a method that fills out and submits the form. This is what such a method looks like:

{% gist d59d48773c7ad50f9b477671553a68e6 %}

What’s happening here?  
Let’s go through this line by line.

#### Getting the page

```java
HtmlPage page = webClient.getPage(pageUrl);
```

`HtmlPage page` represents the initial page. `HtmlPage` comes from the HtmlUnit library. I use an instance of `WebClient`, another HtmlUnit class, to get the `HtmlPage` by calling `getPage()` on it and providing the URL. The `WebClient` instance is initialized with a simple `WebClient webClient = New WebClient()`.

#### Getting the form

```java
final HtmlForm form = page.getFormByName("aspnetForm");
```

The `getFormByName()` method provides us with another `HtmlUnit` object, this one representing the form. It is easy to identify the name of any elements that we need using the browsers dev tools as seen below.

{% include youtubePlayer.html id="40hyOkqwKYw" %}

#### Getting and interacting with the input fields

Since we only want court “opinions” we get the checkbox labeled “opinions”:

```java
final HtmlCheckBoxInput checkBoxInput = form.getInputByName("ctl00$ContentPlaceHolder1$chkListDocTypes$0");
```

We “check” it programatically with the line:

```java
checkBoxInput.setChecked(true);
```

We also need to set a “from” date. We get the “from” field with:

```java
final HtmlTextInput textField = form.getInputByName("ctl00$ContentPlaceHolder1$dtDocumentFrom$dateInput");
```

and “type” in a properly formatted string with:

```java
textField.type(getFromDateString());
```

If you are curious about how we get the date string, here is what that looks like:

{% gist https://gist.github.com/YitziG/9fe7112fa9b7cdea7072f6c5d8a56692 %}

This bot runs frequently, so it is sufficient to fetch the past two weeks of opinions only.

Lastly, we need the submit button:

```java
final HtmlSubmitInput button = form.getInputByName("ctl00$ContentPlaceHolder1$btnSearchText");
```

and finally, `button.click()` will submit the form and return the page generated as a result of the submission.

![](https://cdn-images-1.medium.com/max/1024/1*bz7q0kJdctRlI2I_E5Se9g.png)

Our bot is now seeing a neat list of the most recent Supreme Court opinions. The bot can efficiently run through this list downloading and tagging each document.

#### The results:

After a single run the bot has:

- downloaded 541 files from Texas courts!

![](https://cdn-images-1.medium.com/max/523/1*nDIXiR95NKp2T3iNm87VlQ.png)My other computer’s a Mac

- Inserted the download info into our database. This info includes the source URL as well as the current location on our server.

![](https://cdn-images-1.medium.com/max/1024/1*TNmvRE-nZB5LNFMqYlyWoQ.png)Yes, I know this is NJ. Sue me.

- Inserted metadata about the case with a foreign key to the download entry. This metadata includes the date of the decision, case title, and issuing court.

![](https://cdn-images-1.medium.com/max/942/1*-skhnf_Wf0kEPnadEBqGlg.png)

### Grand Finale!!

![](https://cdn-images-1.medium.com/max/1024/1*QQXEgdM3zLbuzOhKYM1dzw.png)

Finally, we can make use of this data by sharing it with the world! All this messy, unorganized data is now beautifully organized, searchable, and tagged. [Have a look](http://anylaw.com) and you’ll discover some other cool stuff that we’ve done with this data.

Thanks for reading!

I’m Yitzi 👋

[Yitzi Ginzberg (@codegician) | Twitter](https://twitter.com/codegician)
