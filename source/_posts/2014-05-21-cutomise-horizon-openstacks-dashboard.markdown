---
layout: post
title: "Cutomise Horizon: OpenStack's Dashboard"
date: 2014-05-21 17:07:46 +0530
comments: true
categories: 
---

Horizon is a web based user interface to manage Openstack services like Nova, Neutron, Glance, etc. It was Canonical who went ahead and implemented this from Openstack's Dashboard. It is a Django-based project which builds on a Django site which uses `dashboard` to communicate with the Openstack services. My goal here was to customize it to put in my own logo.

<!--more-->

## Follow the Docs

I have never worked on Django framework so I intuitively started looking at the docs to give me pointers to where to look for the css which linked to the logo image file. I first looked [here](http://docs.openstack.org/havana/config-reference/content/dashboard-custom-brand.html), but realized that this was the original **Dashboard** and not the Canonical's Horizon that was working with. Well, it's a shame that I had to put in the raged comment there. But, it is really very easy to get that page confused with Horizon. 

A little more looking at I found [this](http://docs.openstack.org/developer/horizon/topics/customizing.html). It clearly says Horizon, so looks like we are going somewhere now. But I just could not find the file:`openstack_dashboard/local/local_settings.py` in the very first instruction which can change the page title.

## Why are you doing this to me?

After being disheartened by not able to find the file, I read on. Oh! there is a file location specified for something else, maybe it's there. The `local_setting.py` is in `/usr/share/openstack-dashboard/openstack_dashboard/local/local_settings.py`. Just include a line to get a custom page title `SITE_BRANDING="This is my Openstack"` in that file.

Now, I wanted to replace the OpenStack logo which appears when I login and after the login in my dashboard with my own logo. The next section in the [document](http://docs.openstack.org/developer/horizon/topics/customizing.html) gives me instructions for exactly that. I just had to replace the image file at `/usr/share/openstack-dashboard/openstack_dashboard/static/dashboard/img/logo.png.` But, I dont see what I'm supposed to see. The old image is still there.

## Inspect element

At this point of time, I'm questioning the effectiveness of the documentation. I close the document. And open up Chrome and use the developer tools on the dashboard. I find some weird hexa after the url `url('/static/dashboard/img/logo-splash.png?bd0a834bae0e')`. And I soon realize that this css is in `/usr/share/openstack-dashboard/static/dashboard/css`. And it has been relatively referencing to the `img` directory. That is not where I put my logo.

What's done is done. So, I replace `logo-splash.png` and `logo.png` in the `/usr/share/openstack-dashboard/static/dashboard/img` directory. And finally I have my logo show up at the login screen. But, the old Openstack logo shows up after you login.

I dive right into the css remove the funny hexa on the url. And edit a little css to get my logo to look nice here. Looks like Horizon uses a css compressor to make your pages load faster. I have never used one and do not know how they work. So to get your logo at the dashboard edit the css file `/usr/share/openstack-dashboard/static/dashboard/css/291184af72d2.css` (it might be different name for you)

``` css
h1.brand a {
  background: url('/static/dashboard/img/logo.png') top center no-repeat;
  display: block;
  float: left;
  width: 166px;
  height: 123px;
  text-indent: -9999px;
  margin-left: 30px;
  margin-top: 15px;
  margin-bottom: 25px;
}
```

Hope this helps you brand your OpenStack install. And please dive right into the code if you ever feel the documentation is not getting you anywhere.