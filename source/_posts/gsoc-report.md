---
title: Google Summer of Code 2021 Final Evaluation
date: 2021/8/23 14:44:00
---

This is my final evaluation of Google Summer of Code 2021.

I worked in Casbin this summer, and my job is to write code for [Casdoor](https://github.com/casbin/casdoor) and [Casnode](https://github.com/casbin/casnode). Click the link to read [My Proposal of Google Summer of Code 2021](https://archives.kininaru.dev/gsoc/2021/proposal.pdf).

The first part of this report is a brief summary of my work. For detailed contributions, please read the summary of pull requests below.

# Summary

Here are some big features I implemented for Casdoor and Casnode. In addition, I also fixed bugs for both of them.

## Casdoor

Casdoor is a Single-Sign-On platform based on OAuth 2.0 / OIDC. It can also serve as a service provider (you can use Casdoor to send Emails, upload files, set an avatar). My work during Google Summer of Code is to implement some of these functions, and add new features to it.

**Support avatar tailoring uploading: ** This is the first contribution I made to Casbin community. I spent almost a week learning the file structure of Casdoor, Golang and React, then I talked to my mentor about the details and the front end UI. It was really excited to see the PR was merged in the end. After this, I became familiar with the process of making a contribution to the community, so things became easy after that.

**go-sms-sender and the verification code: ** There are many SMS providers in mainland China. Each of them has a different API and SDK. So it is not easy to adapt all of them. To keep the code of Casdoor clean and easy to understand, I developed [go-sms-sender](https://github.com/casdoor/go-sms-sender). It is designed to use all kinds of SMS providers easily. Only few lines of code is needed to send a SMS via the sender. With go-sms-sender, I also implemented the function of sending verification code via Email and SMS for Casdoor.

**Turing test: ** Users are required to pass a Turing test before sending a verification code to their Emails or Phones.

**Casdoor SDKs: ** Casdoor is written in Golang, but it supports many languages. Casdoor uses HTTP requests to communicate with those SDKs. I participated in the development of Casdoor Go SDK, Casdoor JS SDK (supported popup sign in and sign up), Casdoor Java SDK, and reviewed the code of Casdoor PHP SDK.

## Casnode

Casnode is the official forum of Casbin. Casnode is suitable for open source organizations to use because it supports the two-way synchronization to Google Groups, and can sync its topics and replies to all mailing lists.

**Night mode: ** I supported the night mode for Casnode. Now, users can click the switch button to switch between night mode and light mode.

**Refactored the OSS support: ** Casnode uses Aliyun STS as a storage provider before. This not that safe because users can upload any file to anywhere in the storage server. I refactored the storage system with another Go module called qor, which supports variables of storage providers, and upload these files in the backend, which is safer than before.

**Server side rendering: ** Casnode is written in React, which means that bots from search engines can not get the forum details because they can not execute JS code. I used Chromedp in Golang backend, and added a filter for bots. If a HTTP request has a `bot` keyword in its `User-Agent`, it will be recognized by the bot filter, and will receive a rendered page with forum details. This way, you can get the information of the forum from search engines.

**Mailing list support: ** Casnode sends Emails to a mailing list when new topics or new replies are made. It can sync all new topics and replies to all mailing lists, and supports the two-way synchronization to Google Groups. This made Casnode suitable for open source organizations to use.

**Embeded plugin: ** I implemented the embeded plugin for Casnode. With the plugin, only few lines of HTML code is needed to embed Casnode as a reply plugin to any webpage. For example, [Casbin OA](https://oa.casbin.com/) uses Casnode  Embedded Plugin as a reply plugin.