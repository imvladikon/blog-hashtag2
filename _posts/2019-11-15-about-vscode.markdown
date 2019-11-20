---
layout: post
title:  "A little bit about vscode and about yet another plugin"
date:   2019-11-15 10:15:16 +0200
categories: 
description: 
tags: vscode plugins
---
I use a bunch of vscode plugins, there are a lot of resources about it, but I recommend [this one](https://github.com/Jakobud/vscode-cdnjs)  Great plugin, just couple press of button, and we can include some js libraries in our protype (in production system it's better use ~~spaceship~~ webpack and etc.) But one day I just typed some code, and found out that cdnjs (that plugin above uses) don't cover all of set js libraries, especially libraries that built on npm. Of course we can use npm, 

<img src="/assets/img/npm_hell2.png" alt="npm_hell" width="350"/>
<!--break-->
but I decided it's better to have the same plugin, that uses [unpkg](https://unpkg.com/)
And [here it is](https://github.com/imvladikon/vscode-unpkg) 