---
author: mathieu.poirier
comments: true
date: 2014-08-14 07:46:40
description: "Short introduction to the Arm\xC2\xAE CoreSight\xE2\x84\xA2 technology
  and goes over the upstreaming effort currently ongoing at Linaro to provide a uniform
  architecture across all platforms."
excerpt: "This blog post gives a short introduction to the Arm\xAE CoreSight\u2122
  technology and goes over the upstreaming effort currently ongoing at Linaro to provide
  a uniform architecture across all platforms. "
layout: post
link: /blog/core-dump/coresight-initial-steps-supporting-hw-assisted-tracing-linux-arm-socs/
slug: coresight-initial-steps-supporting-hw-assisted-tracing-linux-arm-socs
tags:
- Core Dump
- arm
- CoreSight     
- socs
categories:
  - blog
title: 'CoreSight: Initial steps in supporting HW assisted tracing on Linux for Arm
  SoCs'
wordpress_id: 6299
---

# CoreSight: Initial steps in supporting HW assisted tracing on Linux for Arm SoCs


One of the challenges when developing products based on Arm SoC (System on Chip) solutions is debugging the complex interactions of a software stack that runs across multiple HW components (GPU, CPU, DSPs, etc). To help alleviate the complexity introduced by the interconnection of those components, a wealth of debugging tools and hardware assisted tracing solutions are available, largely falling under the Arm® CoreSight™ technology.

In this post concentrate on hardware assisted tracing (simply referred to as CoreSight from hereon) where IP blocks are added to a design based on the components found in the SoC and expected tracing needs, providing an on-system solution that is flexible and easily adaptable. The blocks themselves are inter-connected and categorized as data sources, links and sinks, each being enabled and configured at run time based on specific scenarios. A typical CoreSight system is represented in the below diagram (source: Arm Ltd):

{% include image.html name="CoreSight-HW-assisted-Linux-ARM-SoCs.jpg" alt="CoreSight-HW-assisted-Linux-Arm-SoCs" %}

Current support in the Linux kernel for hardware-assisted tracing on Arm SoCs with CoreSight technology is fairly limited. Today’s solutions are generally static, tailored to a single architecture, hardly extensible and not maintained. The result is a fragmented landscape of proprietary solutions targeting very specific goals. These solutions are by their nature not fit for upstream acceptance. In user space, the situation is just as difficult since there is no tool available to collect and package the configuration data, let alone decode the compressed streams

Over the last several months Linaro has been working hard on Coresight support in the Linux kernel.  Starting from the initial submission from codeAurora back in [December of 2012](http://lists.infradead.org/pipermail/linux-arm-kernel/2012-December/138646.html), we  addressed all comments that came back from the community. Two more rounds ([V1](about:blank) and [V2](http://thread.gmane.org/gmane.linux.kernel/1734361)) were then posted for review, each time making modifications to provide a more generic solution that is more suitable for integration into the Linux kernel.  A third set, to be released in the [coming days](https://git.linaro.org/kernel/coresight.git/refs/), allows multiple sinks to be enabled simultaneously, whether they belong to the same Coresight source or not. This feature marks a sharp improvement over the initial implementation, increasing the debugging capabilities of the solution.

Despite being satisfied with the ongoing upstreaming process, the CoreSight team at Linaro (and the community at large) is still faced with serious challenges, most notably in the area of packaging source configuration information (commonly called “metadata”) and decoding trace streams generated by trace sources.  Although we haven’t identified any generic solutions, significant progress on trace decoding has been achieved lately.  Working in partnership with our friends at Arm Ltd, trace streams generated on the TC2 platforms have been decoded. The process, documented [here](https://wiki.linaro.org/WorklingGroups/Kernel/Coresight/traceDecodingWithDS5) and available to all, can seem convoluted at first, but steps are explained thoroughly and example files provided so that interested parties can start from a working example. Last but not least, we have started to look at how support for STMs (System Trace Macrocells) can be integrated to the existing CoreSight framework and how it can be utilised in conjunction with existing kernel subsystems, most notably in the area of debugging.  The main challenge will be to find a way to define and configure channels between kernel and user space, something we intend to take to the community by hosting a talk at the [Linux Plumbers Conference](https://www.linuxplumbersconf.org/) in Dusseldorf (Germany) in October.

Arm® CoreSight™  is an [Arm trademark](http://www.arm.com/about/trademarks/arm-trademark-list/CoreSight-trademark.php).