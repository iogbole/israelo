---
layout: post
title:  "The Raspberry Pi Project: Overview"
author: israel
categories: [ 'Cloud Native', 'Edge Computing', 'Pi' ]
tags: [containers, raspberry, cloud-native, pi, IoT, edge ]
image: https://user-images.githubusercontent.com/2548160/107218082-0b30e080-6a07-11eb-80e7-c62e4f1197d2.jpg
date:   2021-02-08 15:01:35 +0300
excerpt: "The Raspberry Pi Project: Overview"

permalink: /draft/pi

---

Raspberry Pi is a series of small single-board computers developed in the United Kingdom by the Raspberry Pi Foundation in association with Broadcom.

The Raspberry Pi 4 Model B is the latest version (as at the of writing this blog) of the low-cost Raspberry Pi computer. The Pi isn't like your typical device; in its cheapest form it doesn't have a case, and is simply a credit-card sized electronic board -- of the type you might find inside a PC or laptop, but much smaller.

I host a few <a href="https://woocommerce.com/" target="_blank"> woo-commerce </a> (side-hustle) projects and websites for fiends and family with a hosting company. I read about the Raspberry Pi 4 boards and I was super impressed by the power of this small computer board. I did the maths, it worked out cheeper to host my sites (including the Wordpress woocommerce site) from home using a cluster of Pis - so I decided to build a Raspberry Pi cluster for my projects -  including a Network Attached Storage (NAS) server.  That said, the cost saving element of the story is only an excuse to justify getting the Pi toys, I derive a lot of fun tinkering with programmable boards.

I will use the Ras will document my experience and the wrong/right decisions I made whilst tinkering with the Raspberry Pi. 

In the end, I ended up building a Pi Project that does the following:

1. <b> K3s Kubernetes Cluster </b>

    I containerised all the sites so I can use Kubernetes to orchestrate the deployments. The Rancher K3s Kubernetes cluster is managed via Ansible playbooks. I used K3s because it is lightweight and super efficient on the Pi. 

2. <b> Network Attached Storage </b>

   The NAS server serves two purposes:
    a) It manages the NFS share and raid replication that is used for the wordpress MySQL (Kubernetes) persistent storage.

    b) It is used to sync photos, files, videos etc from our phones and laptops. We also stream contents from the NAS server to TV. It's more or less private cloud for the family.  

   I used <a href="https://nextcloud.com/"> NextCloud</a> open source software to manage the NAS server. It has a really good app for Android, iPhones.

The result of my setup is shown the the picture below:

A cased up Pi booting from an SSD drive (using a SATA III USB 3 converter).

<p class="aligncenter">
<img alt ="pi" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107225755-1be65400-6a11-11eb-81b5-d67a245eb34f.jpg"/> 
<br>
</p>

Stack `em up!

<p class="aligncenter">
<img alt ="stack" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107226410-00c81400-6a12-11eb-9dbc-d35b0d69dd17.jpg"/> 
<br>
</p>

I stacked cased Pis up using glue dots, and a Rubber ring to hold the SSD drives together. I will explain why I did not use the regular Pi cluster casings in consequent blog posts.

<p class="aligncenter">
<img alt="stack2" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107226521-26edb400-6a12-11eb-8b3b-20421fde95ff.jpg"/> 
<br>
</p>

## Components 


Please leave a comment below if you have any questions.

Good luck and stay hungry!

<p class="aligncenter">
<img class="lazyimg" src="https://user-images.githubusercontent.com/2548160/104319297-94bcc380-54d8-11eb-83b1-1b992f4bdaff.JPG"/> 
<br>
<font size="-3">Img source : beariscool</font>

</p>
