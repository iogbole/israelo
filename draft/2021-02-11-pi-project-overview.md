---
layout: post
title:  "Raspberry Pi Kubernetes Cluster: Getting Started"
author: israel
categories: [ 'Cloud Native', 'Edge Computing', 'Pi' ]
tags: [containers, raspberry, cloud-native, pi, IoT, edge ]
image: https://user-images.githubusercontent.com/2548160/107218082-0b30e080-6a07-11eb-80e7-c62e4f1197d2.jpg
date:   2021-02-08 15:01:35 +0300
excerpt: "I had a few websites running on a managed hosting platform. This blog series describes how I am now running those public-facing websites (including a NAS server) from home using a Kubernetes Cluster running on the Raspberry Pi..."

permalink: /draft/pi

---

Raspberry Pi is a series of small single-board computers developed in the United Kingdom by the Raspberry Pi Foundation in association with Broadcom.

The Raspberry Pi, 4 Model B, is the latest version (as at the time of writing) of the low-cost Raspberry Pi computer. The Pi isn't like your typical device; in its cheapest form it doesn't have a case, and is simply a credit-card sized electronic board, similar to the type you might find inside a PC or laptop, but much smaller.

I had a few websites and <a href="https://woocommerce.com/" target="_blank"> woo-commerce </a> (side-hustle) projects hosted by a managed web hosting company. I read about the Raspberry Pi 4 boards, and I was super impressed by this small computer board's power. I did the maths; it worked out cheaper to host my sites (including the WordPress woo-commerce site) from home using Raspberry Pi - so I decided to build a Kubernetes cluster using Raspberry Pis to host my projects. That said, the cost-saving element of the story is only an excuse to justify getting myself some Pi toys. I derive a lot of fun tinkering with programmable boards.

Furthermore, some of my sites require databases, so there was a need to set up persistent storage for the database pods. This is where the idea of (Pi) NAS server came into the picture. As well as all the benefits that a regular NAS server provides, I configured mine to host and manage the Kubernetes NFS volume - this lets me mount <a href="https://kubernetes.io/docs/concepts/storage/volumes/" target="_blank"> NFS volumes </a> on multiple pods.

I will use the Raspberry Pi blog series to document and share my experience, and the wrong/right decisions I made whilst tinkering with the Raspberry Pi.

<p class="aligncenter">
<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/-cOix8JhjmQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

In the end, I ended up building a Pi Project that does the following:

1. <b> K3s Kubernetes Cluster </b>

    I containerised all the sites so I can use Kubernetes to orchestrate the deployments. The Rancher K3s Kubernetes cluster is managed via Ansible playbooks. I used K3s because it's lightweight and super-efficient on the Pi. 

2. <b> Network Attached Storage </b>

   The NAS server serves two purposes:

    a) It manages the NFS share and raid replication that is used for the WordPress MySQL (Kubernetes) persistent storage.

    b) It is used to sync photos, files, videos etc. from our phones and laptops. We also stream contents from the NAS server to TV. It's a private cloud for the family.  

   I used <a href="https://nextcloud.com/"> NextCloud</a> open source software to manage the NAS server. It has a really good app for Android, iPhones.

The result of my setup is shown in the pictures below:

A cased up Pi booting from an SSD drive (using a SATA III USB 3 converter).

<p class="aligncenter">
<img alt ="pi" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107225755-1be65400-6a11-11eb-81b5-d67a245eb34f.jpg"/> 
<br>
</p>

Stack 'em up!

<p class="aligncenter">
<img alt ="stack" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107226410-00c81400-6a12-11eb-9dbc-d35b0d69dd17.jpg"/> 
<br>
</p>

I stacked the cased Raspberry Pis up using glue dots (they hold very well), and a rubber ring to hold the SSD drives together. I will explain why I did not use the regular Pi cluster casings in consequent blog posts.

<p class="aligncenter">
<img alt="stack2" class="lazyimg" src="https://user-images.githubusercontent.com/2548160/107226521-26edb400-6a12-11eb-8b3b-20421fde95ff.jpg"/> 
<br>
</p>

## Components

These are some of the components that I used: 

1. One 8GB RAM and two 4GB RAM Raspberry Pi 4 boards

    The 8GB RAM board is used as a NAS server, NFS persistent storage, and a Kubernetes worker node.

    The first 4GB RAM board is used as the Kubernetes master node. I also configured it as a worker node; however, I ensured to use <a href="https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity" target="_blank">NodeAffinity </a> to selectively run lightweight workloads on this nodes. 

    The second 4GB RAM  board is a Kubernetes worker node - again, I used NodeAffinity to schedule heavier workloads on this node.

    All three nodes run the Raspberry Pi OS.

    The 8GB RAM node is still heavily underutilised - it's roughly under 3GB RAM utilisation on average. So it's probably not worth the investment, the 4GB is just what is needed for my kind of workloads.

    I bought all three  Raspberry Pi boards from  <a href="https://uk.rs-online.com/web/c/raspberry-pi-arduino-development-tools/raspberry-pi-shop/raspberry-pi/" target="_blank"> RS-Components. </a>

2. Three SSD drives:

    I used a 500GB repurposed SSD for the NAS server, and two 250GB SSD drives for the master and worker nodes. However, I bought these <a href="https://www.amazon.co.uk/gp/product/B077XVTTJC/ref=ppx_yo_dt_b_asin_title_o09_s00?ie=UTF8&psc=1" target="_blank"> SATA III 2.5 inch enclosures. </a>

    A combination of SSD and USB 3 gives you speed, which is most needed by the Kubernetes master node. 

3. Acrylic case with a fan and 4pcs Heat sinks - The famous Pi cluster case didn't work for me, because of the SSDs. I had to return it. 
   I also needed the flexibility to be able to take the boards apart if needed in the future. The fan keeps the board cool, but it can be a little bit noisy when used with the 5V GPIO pin.  Amazon <a href="https://www.amazon.co.uk/gp/product/B07TVLTMX3/ref=ppx_yo_dt_b_asin_title_o06_s00?ie=UTF8&psc=1" target="_blank">link </a>
 
4. USB Power - You need a 5V and 3.1A constant power supply for each board. I bought an extension cable that can also be used as a three-pin plug.  Amazon <a href="https://www.amazon.co.uk/gp/product/B083184N9N/ref=ppx_yo_dt_b_asin_title_o05_s01?ie=UTF8&psc=1" target="_blank">link </a>

5. 3 park of 1.5m USB C cables.  Amazon <a href="https://www.amazon.co.uk/gp/product/B07CJJHVKX/ref=ppx_yo_dt_b_asin_title_o04_s00?ie=UTF8&psc=1" target="_blank">link </a>

4. Silicon Power 32GB 3D NAND High-Speed MicroSD Card. You will need at least one SSD card to boot the Raspberry Pis. 
Amazon <a href="https://www.amazon.co.uk/gp/product/B07RMXNLF4/ref=ppx_yo_dt_b_asin_title_o07_s00?ie=UTF8&psc=1" target="_blank">link </a>

5. SD Card Reader. Amazon <a href="https://www.amazon.co.uk/gp/product/B07KVZJH2D/ref=ppx_yo_dt_b_asin_title_o05_s01?ie=UTF8&psc=1" target="_blank">link </a> 

In the next blog post, I wil describe how to configure your the Raspberry Pi. 

Stay hungry! 

Please leave a comment below if you have any questions.


- name: appd-log-analytics
        image: appdynamics/analytics-agent:20.6.0-win
        imagePullPolicy: Always
        volumeMounts:
        - mountPath: "c:/logdir"
          name: log-volume
        ports:
        - containerPort: 9090
          protocol: TCP
        #windows analytics agent requires a minimum of 1GB heap space to start. 
        resources: 
          requests: 
            memory: "1Gi"
          limits: 
            memory: "1.5Gi"
        envFrom:
        - configMapRef:
            name: analytics-agent-config