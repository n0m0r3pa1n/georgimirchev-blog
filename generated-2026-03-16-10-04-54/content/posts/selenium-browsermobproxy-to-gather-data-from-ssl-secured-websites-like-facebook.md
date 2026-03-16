---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:20"
_publicize_job_id: "43819835290"
author: gmirchev90
categories:
  - web
date: "2020-05-04T11:00:43+00:00"
guid: http://georgimirchev.com/?p=116
parent_post_id: null
post_id: "116"
summary: Recently, Facebook decided to turn off access to public events. So now, if you are not an administrator of the page or if you don’t have an access to the page, you don’t have access to its events, even though they are public. Here is the [bug](https://web.archive.org/web/20191023023240/https://developers.facebook.com/bugs/1697903980256107/) for it.
tags:
  - brotli
  - browsermob
  - browsermobproxy
  - facebook
  - selenium
  - ssl
timeline_notification: "1588590044"
title: Selenium + BrowserMobProxy to gather data from SSL secured websites like Facebook
url: /2020/05/04/selenium-browsermobproxy-to-gather-data-from-ssl-secured-websites-like-facebook/

---
Recently, Facebook decided to turn off access to public events. So now, if you are not an administrator of the page or if you don’t have an access to the page, you don’t have access to its events, even though they are public. Here is the [bug](https://web.archive.org/web/20191023023240/https://developers.facebook.com/bugs/1697903980256107/) for it.

You probably don’t know but Facebook are slow as sh\*t when it comes to responding to issues or bugs and now, when they are under pressure by the Senate to do some changes, they are even worse. They tell you that you need an approved app to get the events list but if you try to submit your app for approval it says: “We don’t accept app submissions at this moment”. That’s why I decided it may be quicker for me to implement another solution that will parse public events without the need of an API to return them. Let’s check it out.

## Observations on how Facebook works

Let’s check how Facebook works. Let’s open a webpage with events. For example [this one](https://www.facebook.com/crosstravel.bg/events/).

### GraphQL

Soon you will find that some requests that end in `graphqlbatch` are the ones that actually fetch the data. We know that [GraphQL](https://graphql.org/learn/) is the language used by Facebook to query their server. And all of the data we are interested in is there. Like event name, date, location etc.

### Solutions

So, there are not many solutions if we want to manually parse public events from Facebook.

- We can parse manually the returned web page and use xpath to select the date we need from the HTML elements OR
- We can intercept all network calls done from the web page and get the data from them. So the idea is to get these `graphqlbatch` requests that happen in the browser inside our Selenium code. This is the approach that I have chosen.

## Problems with intercepting network requests

Running code with Selenium is easy. You create a webdriver, tell it to open a website and everything magically works. But there are some preparation problems that will occur.

### Proxy for Selenium

In order to intercept the network requests we will need a proxy for Selenium through which all requests will pass. Such proxy is [BrowserMobProxy.](https://github.com/lightbody/browsermob-proxy)

### SSL connections

We need to figure out SSL connections – when starting Selenium on SSL secured websites like Facebook, Google or others there is a big chance that we either won’t receive a response (response.content.text will be null) or the browser will tell us that “The page is not secure” and we won’t be able to access it. In order to fix that we need to install the BrowserMobProxy SSL certificate in our browser. The certificate can be [found here](https://github.com/lightbody/browsermob-proxy#ssl-support).

### Single Selenium browser profile

Starting Selenium each time we want to parse an event will always create a new browser profile under which the code is executed. This is problematic because we want to login once to Facebook and not have to login each time. Actually, login is not the big problem. Public events can be parsed without the user being logged in. The bigger problem is that we need to install an SSL certificate that will be deleted as soon as the current test session ends which is not what we want. We need to run tests under one browser profile each time they are started.

### Brotli compression in BrowserMobProxy

After we setup everything with the profile, we will see that actually the response.content.text variable says it is base64 encoded string but after a lot of tries this is actually not true. Facebook uses Brotli compression format for the response data and because the BrowserMobProxy does not support Brotli, I had to implement it myself which was not so hard. You can check the [PR](https://web.archive.org/web/20191023023240/https://github.com/lightbody/browsermob-proxy/pull/742) [here](https://github.com/lightbody/browsermob-proxy/pull/742).

## Solutions to intercepting network requests

### Proxy

Use [BrowserMobProxy](https://github.com/lightbody/browsermob-proxy). It is quite simple using it:

```
BrowserMobProxy proxy = new BrowserMobProxyServer();
proxy.start();

Proxy seleniumProxy = ClientUtil.createSeleniumProxy(proxy);
ChromeOptions options = new ChromeOptions();
options.setCapability(CapabilityType.PROXY, seleniumProxy);

WebDriver driver = new ChromeDriver(options);

```

And now the data from the browser passes through the proxy.

### SSL connections using BrowserMobProxy & Selenium

This was a tricky part which took me a lot of time to figure out.

- First, you need to do some code changes

```
BrowserMobProxy proxy = new BrowserMobProxyServer();
// SSL related
proxy.setTrustAllServers(true);
proxy.start();

Proxy seleniumProxy = ClientUtil.createSeleniumProxy(proxy);

ChromeOptions options = new ChromeOptions();
options.setCapability(CapabilityType.PROXY, seleniumProxy);
// SSL related
options.setCapability(CapabilityType.ACCEPT_SSL_CERTS, true);
options.setCapability(CapabilityType.ACCEPT_INSECURE_CERTS, true);
options.addArguments("--ignore-certificate-errors");

WebDriver driver = new ChromeDriver(options);

```

You can see that we added several params like ACCEPT\_SSL\_CERTS etc. This was required so our proxy can gather all of the data that comes to it.

- Second, you need to install the certificate from [HERE](https://github.com/lightbody/browsermob-proxy#ssl-support) in your browser in order to escape the “This page is not secure” page. Here is [link for Firefox](https://support.mozilla.org/questions/1059377) and a [link for Chrome](https://support.securly.com/hc/en-us/articles/206081828-How-to-manually-install-the-Securly-SSL-certificate-in-Chrome).

### Single browser profile

- Chrome

In order to not install the SSL certificate each time you start your tests, you need to use a single browser profile. This is easily achievable for Chrome

```
options.addArguments("--user-data-dir=/home/nmp/selenium");

```

You just setup a directory where the Chrome will store the user profile and now each time you run your tests you don’t need to import the SSL certificate. Once is enough.

- Firefox

For Firefox things are a bit trickier. You need to start firefox with the `-p` flag in order to create a profile. [Here is a great tutorial](http://toolsqa.com/selenium-webdriver/custom-firefox-profile/) how to do that!

### Brotli compression

In order to get the uncompressed data from Facebook you need to decompress it using the Brotli algorithm. I made a PR to the BrowserMobProxy which can be seen [here](https://web.archive.org/web/20191023023240/https://github.com/lightbody/browsermob-proxy/pull/742). In order to run Selenium + BrowserMobProxy you will need to manually import the project that I forked inside Intellij or whichever IDE you use.

## Final Result

```
import com.annimon.stream.Stream;
import net.lightbody.bmp.BrowserMobProxy;
import net.lightbody.bmp.BrowserMobProxyServer;
import net.lightbody.bmp.client.ClientUtil;
import net.lightbody.bmp.core.har.Har;
import net.lightbody.bmp.proxy.CaptureType;
import org.openqa.selenium.Proxy;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.CapabilityType;

import java.util.HashSet;

public class TestChrome {
    public static void main(String[] args) {
        BrowserMobProxy proxy = new BrowserMobProxyServer();
        proxy.setTrustAllServers(true);
        proxy.start();

        Proxy seleniumProxy = ClientUtil.createSeleniumProxy(proxy);

        ChromeOptions options = new ChromeOptions();
        options.setCapability(CapabilityType.PROXY, seleniumProxy);
        options.setCapability(CapabilityType.ACCEPT_SSL_CERTS, true);
        options.setCapability(CapabilityType.ACCEPT_INSECURE_CERTS, true);

        options.addArguments("--user-data-dir=/home/nmp/selenium");
        options.addArguments("--ignore-certificate-errors");

        WebDriver driver = new ChromeDriver(options);

        proxy.enableHarCaptureTypes(CaptureType.getAllContentCaptureTypes());
        proxy.enableHarCaptureTypes(CaptureType.REQUEST_CONTENT, CaptureType.RESPONSE_CONTENT);
        HashSet enable = new HashSet();
        enable.add(CaptureType.REQUEST_HEADERS);
        enable.add(CaptureType.REQUEST_CONTENT);
        enable.add(CaptureType.RESPONSE_HEADERS);
        enable.add(CaptureType.RESPONSE_CONTENT);
        proxy.enableHarCaptureTypes(enable);

        proxy.newHar("facebook.com");

        driver.get("https://www.facebook.com/events/232613057481886/");

        Har har = proxy.getHar();

        Stream.of(har.getLog().getEntries())
                .filter(entry -> entry.getRequest().getUrl().contains("graphql"))
                .map(entry -> entry.getResponse().getContent().getText())
                .forEach(System.out::println);
    }
}
﻿
```

We use the Har file which means “Http Archive” to get the network data. This includes requests, responses, headers and cache and some more additional information. In order to print the body, we access the response.content.text field. I won’t print the actual result here as it includes a lot of personal data, but yeah, it works.
