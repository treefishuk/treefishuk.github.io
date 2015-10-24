---
layout: post
title: Top DNS Tools
--- 


If your website works with a www but not without one, your e-mails are not getting delivered or your website is not available at all... chances are you have a DNS problem...

###What is DNS anyway?

DNS stands for Domain name system. Each domain name such as mywebsite.com has a 'Zone File' which contains a range of records pertaining to that domain. It has a record that says where your website is, a record that says what do do with e-mails and a whole load of other useful records which keep everything relating to a domain name working.

A simple way to think of DNS is it's like a helpful local. The local knows where the pub, post office and gym are. When asked the local will point whoever is asking for directions in the right direction to get to where they want to go. Of course if that local gets knocked on the head by a stray cricket ball and suffers some temporary amnesia, he wont be very helpful at pointing people in the right direction! People either get lost and never find the place they were looking for, or it takes them a lot longer to find the place then it would have had they have been given accurate directions.

###DNS Tools

If you are technically minded there are all sorts of commands you can put into a terminal console such as 'dig' and 'ping' to find out information on a domain or IP address. But the information returned isn't very extensive and it's not very pretty. The good news is there are a fair few tools available to help you discover any DNS issues in a more user friendly way...

###MXToolbox

MXToolbox is one of my favourite tools. The name is a little misleading though, MX records are a type of record in a DNS Zone file that relate to e-mail. So on first glance you would think MXToolbox just checks MX records. However it actually looks up the whole range of records. One of the most useful features on the site is the 'Domain Health' page where you put in a domain name and a Pass/Warning/Error traffic light system highlights any issues found and a description of the problems discovered. All completely free to use.

There is also a subscription service you can sign up to that monitors your domains and notifies you if any problems appear. There are three tiers and the first tier is absolutely free, but has substantially less features then the two paid tiers.

###DNS Stuff

DNS stuff is is limited to the amount of searches you can do on a domain without signing up for a login. But signing up for a login is absolutely free so don't let that put you off! DNS stuff has a 'Professional Toolkit' which does exactly what it says. It's a full set of tools that can analyse a whole range of DNS records and beyond. There are a whole range of boxes where you can input a domain name and find out interesting things about your site, but the best one is the one with a label that simply says 'Do I have DNS problems?'. The report that is generated for this is very detailed, it has a traffic light Pass/Warning/Error system indicating how serious issues are if any, and provides checks for a few things that MXToolbox does not.

There is also a paid subscription service you can sign up to that monitors a domain against Spam databases so you can get notified if your domain gets listed as Spam, as well as a domain monitoring service that reports any DNS issues that occur.

###intodns

IntoDNS is very simple to use. Unlike MXToolbox and DNS Stuff where there are various different tools and checks you can run, IntoDNS is just one test. The test is very similar to the 'Do I have DNS problems?' tool in DNS Stuff. There are a few different things however that get highlighted using this tool.

###Pingdom

Pingdom is one of the most popular tools for checking DNS issues. However it is my least favourite. On a recent occasion where a website had a DNS issue Pingdom completely failed and didn't show any results. All the other tools still gave a complete run down of what was working and what was not, but not Pingdom. My other issue with Pingdom is that the test results are quite basic. Not a lot of detail is provided unless you go the the advanced tab, at which point there is too much information, and the data is no where near as easy to read as the results from IntoDNS or DNS stuff.

###Final Thoughts

For general DNS health I tend to use DNS Stuff and IntoDNS. However, if the problem is specifically e-mail related then I often use MXToolbox. Pingdom didn't really work for me in terms of DNS testing. However, the other tools that Pingdom has on offer are very useful for checking things such as website loading speed, so don't write it off completely. Just because I didn't like it doesn't mean you wont love it!

DNS issues can be scary when you come across them. Sometimes warnings are only warnings and may not be issues at all. So try not to freak out, if you can still see your website in a browser chances are the issues are not as serious as it may seem.

Finally, DNS changes are slow. DNS changes can take 72 hours (if not more) to take effect, so when it comes to DNS there is no such thing as an instant fix. Sometimes in trying to fix something you may actually break it, but not know for a few days until the changes take effect. This is why you should get an expert to look into any DNS issues you come across, and why it is important to check DNS regularly using the tools mentioned after making any changes.