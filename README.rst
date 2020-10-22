LAB: F5 Essential App Protect - Multi-Region with CloudFront (ISC)
==================================================================

.. contents:: Table of Contents

Background & Scenario
#####################

You oversee application delpoyment within your organization. One of these apps is BuyTime Auction: a modern microservices-based written in React.JS and running on NGINX. It was built by an outsourced team in record speed, which raised some questions on whether "proper" security testing has been done. You don't have time, resources, or in-house expertise to put the app through rigorous test.

Your team piloted F5 Essential App Protect and believes it provides the checkbox-simple security that you need. First, you like the built-in protection across a number of vectors, which is a useful "isurance policy" for unknowns that potentially exist any time you leverage many frameworks in the modern app dev approach.

However, where you really think F5 Essential App Protect is going to be a game-changer for this project, is in the following 2 features: 
- the fact it can enable multi-region protection for this Auction app, since it has multiple endpoints around the world to serve its global audience, and
- caching capability offered by intergrated AWS CloudFront feature of this innovative cloud-based WAF.

You are now tasked with enabling both of these capabilities. You're excited and ready to go!

Pre-Requisites
###############

- Main browser: any modern browser (Chrome recommended) for working with the UI (and this lab)

In order to use F5 Essential App Protect you need access to F5 Cloud Services and be logged in with a valid user account. If you need to sign up, or if you already have one, use your Main browser to log into the `F5 Cloud Services portal <http://bit.ly/f5csreg>`_.

.. figure:: https://github.com/f5devcentral/f5-cloudservice-eap-lb-lab/raw/master/_figures/0_1.png

You can use use AWS CloudFront directly from F5 Essential App Protect, no additional accounts required. 

Running the Lab
###############

1. Getting started with the Lab Environment
************************************************************************

`a)` In your email, accept the invite that was sent you for this lab. Note the unique ID for the Organization (ISC-Lab-$$$) that you were asked to join, where $$$ will be your own personalized ISC Lab Organization ID (Org ID). Take note of this Org ID, you will need it later. 

.. figure:: _figures/invite.png

`b)` Inside your main browser F5 Cloud Services Portal and click on the username icon in the top right corner and switch account to the ISC-Lab-$$$ created personally just for you with your Org ID.

.. figure:: _figures/switch_account.png

`c)` Go to the Essential App Protect tab and find your application. We have pre-created one for you. The application name is isc-lab-$$$.securelab.online where $$$ is your unique id.

.. figure:: _figures/open_the_app.png

2. Run a SQL Injection Attack
************************************************************************

Alright. Now you want to "kick the tires" on the app, to see just how poorly it may have been coded. You're thinking why not try a SQL Injection attack first, which inserts a SQL query via the input data field in the web application. Such attacks could potentially read sensitive data, modify and destroy it. More detailed information can be found `here <https://bit.ly/2ZUv0Xl>`_.

Let's now follow the steps below to send a SQL Injection attack via browser to our "BuyTime Auction" app. 

`a)` Copy your FQDN from the F5 Cloud Services Portal and paste it to your browser. In the **LOG IN** window fill in username value as follows (including single quotes) **' OR 1=1 --'** and use any password as the value. *NOTE the space after --, it's needed for the attack*. Click **LOGIN**.

.. figure:: _figures/sql_attack_not_blocked.png

As you can see this attack bypassed the login and is showing the contents of the catalog that should be restricted only to valid users. Not good! 

But, no worries! This app has already been configured with F5 Essential App Protect, and you know that all you need to do is to turn on the Blocking mode on. Let's do this now.

`b)` Go back to the F5 Cloud Services Portal, the **High-risk Attack Mitigation** tab and toggle **Blocking Mode** on.

.. figure:: _figures/sql_attack_turn_on.png

`c)` And now simulate the attack again by repeating the step **a)** above. This time it will be blocked by Essential App Protect.

.. figure:: _figures/sql_attack_blocked.png

You can find detailed event log in the events stream in the F5 Cloud Services Portal, the **VIEW EVENTS** card. 

.. figure:: _figures/sql_attack_events_stream.png

3. Add an Additional Region Endpoint
************************************************************************

For now, our app only has one endpoint located in US East (N. Virginia) and deployed on Amazon AWS. Your regions can be different. But our application is serving a global audience, so let's add the second endpoint located in Europe for European users. *NOTE: Your regions may be different, this is just an example*

`a)` Go to the F5 Cloud Services Portal, the **PROTECT APPLICATION** card. There, in the **Description** field of the **General** tab, you can find the information required for the second region.

.. figure:: _figures/info_in_description.png

`b)` Select **Manage regions**.

.. figure:: _figures/manage_regions.png

`c)` Hit **Add** to add the new region:

.. figure:: _figures/add_region.png

`d)` Fill in the region details with the information found in the **Description** field above and **Save** the settings.

.. figure:: _figures/add_region_details.png

The application will be deployed to the second region. It will take several minutes to complete.

.. figure:: _figures/add_region_deploying.png

When the app is deployed, you will see the **Active** state indicator.

.. figure:: _figures/add_region_active.png

Now let's open the app in the browser and we will see that the region changed to the closed one.

.. figure:: _figures/region_europe.png

4. CloudFront Caching
************************************************************************

Lets open the Developer tools by pressing Ctrl+Shift+I or From "Browser settings" => "More tools" => "Developer tools". Open the Network tab and disable caching and preserve logs.

.. figure:: _figures/dev_tools.png

Now we need to open two browser windows. At the first one we open the website using a domain name and at the second one we use the IP address from the description field in step 3.a. Try to press page refresh couple times and check the page load time. In the first window with domain name it's faster because the traffic flows through the CloudFront CDN.

.. figure:: _figures/side_by_side.png

What's Next?
###############

Good job! If you've gotten this far, you've successfully added another regional endpoint and turned on CloudFront from F5 Essential App Protect. Have you looked at any of the othe labs available, or looked at the F5 Essential App ProtectAnsible project that automates many of its routine tasks?  Here are some things for you to look at:

* EAP Lab
* EAP / DNS Lab
* Ansible repository

Thanks for taking the time to do this lab, let us know any issues in the Issues section of this repo. 
