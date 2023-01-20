---
title: Google Summer of Code 2021 Final Evaluation
date: 2021/8/23 14:44:00
---

This is my final evaluation of Google Summer of Code 2021.

I worked in Casbin this summer, and my job is to write code for [Casdoor](https://github.com/casbin/casdoor) and [Casnode](https://github.com/casbin/casnode). Click the link to read [My Proposal of Google Summer of Code 2021](https://takemeto.icu/archive/gsoc/2021/proposal.pdf).

The first part of this report is a brief summary of my work. For detailed contributions, please read the summary of pull requests below.

# Summary

Here are some big features I implemented for Casdoor and Casnode. In addition, I also fixed bugs for both of them.

## Casdoor

Casdoor is a Single-Sign-On platform based on OAuth 2.0 / OIDC. It can also serve as a service provider (you can use Casdoor to send Emails, upload files, set an avatar). My work during Google Summer of Code is to implement some of these functions, and add new features to it.

**Support avatar tailoring uploading:** This is the first contribution I made to Casbin community. I spent almost a week learning the file structure of Casdoor, Golang and React, then I talked to my mentor about the details and the front end UI. It was really excited to see the PR was merged in the end. After this, I became familiar with the process of making a contribution to the community, so things became easy after that.

**go-sms-sender and the verification code:** There are many SMS providers in mainland China. Each of them has a different API and SDK. So it is not easy to adapt all of them. To keep the code of Casdoor clean and easy to understand, I developed [go-sms-sender](https://github.com/casdoor/go-sms-sender). It is designed to use all kinds of SMS providers easily. Only few lines of code is needed to send a SMS via the sender. With go-sms-sender, I also implemented the function of sending verification code via Email and SMS for Casdoor.

**Turing test:** Users are required to pass a Turing test before sending a verification code to their Emails or Phones.

**Casdoor SDKs:** Casdoor is written in Golang, but it supports many languages. Casdoor uses HTTP requests to communicate with those SDKs. I participated in the development of Casdoor Go SDK, Casdoor JS SDK (supported popup sign in and sign up), Casdoor Java SDK, and reviewed the code of Casdoor PHP SDK.

## Casnode

Casnode is the official forum of Casbin. Casnode is suitable for open source organizations to use because it supports the two-way synchronization to Google Groups, and can sync its topics and replies to all mailing lists.

**Night mode:** I supported the night mode for Casnode. Now, users can click the switch button to switch between night mode and light mode.

**Refactored the OSS support:** Casnode uses Aliyun STS as a storage provider before. This not that safe because users can upload any file to anywhere in the storage server. I refactored the storage system with another Go module called qor, which supports variables of storage providers, and upload these files in the backend, which is safer than before.

**Server side rendering:** Casnode is written in React, which means that bots from search engines can not get the forum details because they can not execute JS code. I used Chromedp in Golang backend, and added a filter for bots. If a HTTP request has a `bot` keyword in its `User-Agent`, it will be recognized by the bot filter, and will receive a rendered page with forum details. This way, you can get the information of the forum from search engines.

**Mailing list support:** Casnode sends Emails to a mailing list when new topics or new replies are made. It can sync all new topics and replies to all mailing lists, and supports the two-way synchronization to Google Groups. This made Casnode suitable for open source organizations to use.

**Embeded plugin:** I implemented the embeded plugin for Casnode. With the plugin, only few lines of HTML code is needed to embed Casnode as a reply plugin to any webpage. For example, [Casbin OA](https://oa.casbin.com/) uses Casnode  Embedded Plugin as a reply plugin.

# Contributions

Here are my contributions to Casbin community.

## Open Issues

- Open Issue [Balance didn't refresh after receiving "today's checkin bonus"](https://github.com/casbin/casnode/issues/178)
- Open Issue [A suggestion for README.md](https://github.com/casbin/casdoor/issues/20)
- Open Issue [Forum website login error](https://github.com/casbin/casnode/issues/159)
- Open Issue [The installation commands in the docs do not work properly](https://github.com/casbin/casbin.js/issues/34)
- Open Issue [Add a self check at the beginning of the program](https://github.com/casbin/casdoor/issues/170)
- Open Issue [Add a self check when the program starts](https://github.com/casbin/casnode/issues/323)

## Open Pull Requests

- Open PR [fix: notification bug](https://github.com/casbin/casnode/pull/185)
- Open PR [fix: bonus bug and Chinese translation missing](https://github.com/casbin/casnode/pull/183)
- Open PR [feat: add go backend API docs](https://github.com/casbin/casdoor/pull/34)
- Open PR [feat: Go backend API docs](https://github.com/casbin/casdoor/pull/33)
- Open PR [Fix: Avatar uploading failed](https://github.com/casbin/casdoor/pull/32)
- Open PR [fix: converted sts to qor](https://github.com/casbin/casnode/pull/180)
- Open PR [feat: convert Aliyun STS to qor/oss](https://github.com/casbin/casnode/pull/179)
- Open PR [fix: member page links bug](https://github.com/casbin/casnode/pull/177)
- Open PR [fix: rich text editor img bug](https://github.com/casbin/casnode/pull/175)
- Open PR [fix: rich list bug](https://github.com/casbin/casnode/pull/173)
- Open PR [feat: add sensitive words block](https://github.com/casbin/casnode/pull/169)
- Open PR [fix: node bugs](https://github.com/casbin/casnode/pull/166)
- Open PR [feat: added imgs and css for forum night mode](https://github.com/casbin/static/pull/5)
- Open PR [feat: added night mode](https://github.com/casbin/casnode/pull/164)
- Open PR [add: night.js for casbin-forum](https://github.com/casbin/static/pull/4)
- Open PR [feat: added avatar tailoring and uploading](https://github.com/casbin/casdoor/pull/23)
- Open PR [Support avatar tailoring and updating](https://github.com/casbin/casdoor/pull/22)
- Open PR [added explainations to Dev and Prod usage in README](https://github.com/casbin/casdoor/pull/21)
- Open PR [added details about EnforceEx()](https://github.com/casbin/casbin-website/pull/178)
- Open PR [Fix: https://github.com/casbin/casbin-website/issues/82](https://github.com/casbin/casbin-website/pull/176)
- Open PR [Fix: Error document](https://github.com/casbin/casbin-website/pull/175)
- Open PR [added Casbin RBAC and RBAC96](https://github.com/casbin/casbin-website/pull/173)
- Open PR [added Casbin RBAC and RBAC96](https://github.com/casbin/casbin-website/pull/172)
- Open PR [added a tutorial for APIs](https://github.com/casbin/casbin-website/pull/170)
- Open PR [feat: add info about MySQL and Postgres in readme](https://github.com/casbin/casdoor/pull/51)
- Open PR [feat: add pg for db adapter](https://github.com/casbin/casdoor/pull/50)
- Open PR [feat: recognize google group email correctly](https://github.com/casbin/google-groups-crawler/pull/3)
- Open PR [feat: sync google group conversations to Casnode](https://github.com/casbin/casnode/pull/224)
- Open PR [fix: typo in readme](https://github.com/casbin/google-groups-crawler/pull/2)
- Open PR [feat: init commit](https://github.com/casbin/google-groups-crawler/pull/1)
- Open PR [feat: added Mailing List](https://github.com/casbin/casnode/pull/223)
- Open PR [feat: node header image](https://github.com/casbin/casnode/pull/222)
- Open PR [fix: node background image uploading failed](https://github.com/casbin/casnode/pull/221)
- Open PR [fix: add min width to desktop pages](https://github.com/casbin/static/pull/8)
- Open PR [feat: added cdp for ssr](https://github.com/casbin/casnode/pull/217)
- Open PR [fix: avatar display error in topic page](https://github.com/casbin/casnode/pull/216)
- Open PR [fix: picture too wide in notification page](https://github.com/casbin/static/pull/7)
- Open PR [fix: reply box height and avatar too small](https://github.com/casbin/casnode/pull/215)
- Open PR [add reset username and fixed HTML tag display error](https://github.com/casbin/casnode/pull/214)
- Open PR [fix: img doesn't show in richtext editor replies](https://github.com/casbin/casnode/pull/212)
- Open PR [fix: page didn't jump on mobile UI and reply links](https://github.com/casbin/casnode/pull/211)
- Open PR [fix: reload replies from backend incorrectly](https://github.com/casbin/casnode/pull/210)
- Open PR [refactor: rewrite topic page logic](https://github.com/casbin/casnode/pull/207)
- Open PR [fix: reply page bug and admin new node page crash](https://github.com/casbin/casnode/pull/206)
- Open PR [Initialized docusaurus files and added CI](https://github.com/casnode/casnode-website/pull/1)
- Open PR [made last page the default page of reply box, and made auto jump smooth](https://github.com/casbin/casnode/pull/205)
- Open PR [feat: auto jump when reply is not in the first page](https://github.com/casbin/casnode/pull/203)
- Open PR [fix: login check bug](https://github.com/casbin/casnode/pull/201)
- Open PR [fix: mobile select2 and casnode reply img error](https://github.com/casbin/static/pull/6)
- Open PR [fix: made casnode adapt the layout according to the page size](https://github.com/casbin/casnode/pull/193)
- Open PR [feat: convert casbin forum to casnode](https://github.com/casbin/casnode/pull/190)
- Open PR [fix: rich text editor doesn't refresh after replying](https://github.com/casbin/casnode/pull/187)
- Open PR [fix: text colors are not in effect](https://github.com/casbin/casnode/pull/186)
- Open PR [feat: add provider intro](https://github.com/casdoor/casdoor-website/pull/9)
- Open PR [feat: add basic intro to Casdoor](https://github.com/casdoor/casdoor-website/pull/7)
- Open PR [fix: removed outdated lines in README](https://github.com/casbin/casdoor/pull/73)
- Open PR [feat: use retro style when user did not set gravatar](https://github.com/casbin/casnode/pull/250)
- Open PR [fix: add Re to mailing list replies](https://github.com/casbin/casnode/pull/249)
- Open PR [fix: print log when the cookie expires](https://github.com/casbin/casnode/pull/244)
- Open PR [feat: update crawler to the latest version](https://github.com/casbin/casnode/pull/243)
- Open PR [fix: http client aren't closed](https://github.com/casbin/google-groups-crawler/pull/16)
- Open PR [refactor: rewrite all logic](https://github.com/casbin/google-groups-crawler/pull/15)
- Open PR [fix: Conversation message split](https://github.com/casbin/google-groups-crawler/pull/14)
- Open PR [fix: Conversation message error](https://github.com/casbin/google-groups-crawler/pull/13)
- Open PR [refactor: rewrite crawler logic to get conversations](https://github.com/casbin/google-groups-crawler/pull/12)
- Open PR [fix: code won't exit when an Internet error happened](https://github.com/casbin/google-groups-crawler/pull/11)
- Open PR [feat: add cookies when getting messages](https://github.com/casbin/google-groups-crawler/pull/10)
- Open PR [fix: invalid date for Google group messages](https://github.com/casbin/casnode/pull/237)
- Open PR [fix: time syntax error in Google groups conversations](https://github.com/casbin/google-groups-crawler/pull/9)
- Open PR [feat: get conversation start time](https://github.com/casbin/google-groups-crawler/pull/8)
- Open PR [feat: turing test before sending code](https://github.com/casbin/casdoor/pull/70)
- Open PR [feat: intergrate verification code countdown as a component](https://github.com/casbin/casdoor/pull/66)
- Open PR [feat: check email and phone number when signing up](https://github.com/casbin/casdoor/pull/63)
- Open PR [feat: upload avatar to oss when sync from google group](https://github.com/casbin/casnode/pull/229)
- Open PR [feat: Maven project init and add getUsers() getOrganizations() API](https://github.com/casdoor/casdoor-java-sdk/pull/1)
- Open PR [fix: add judgement to avoid crashing](https://github.com/casbin/google-groups-crawler/pull/7)
- Open PR [fix: remove all dom APIs from the project](https://github.com/casbin/casdoor/pull/57)
- Open PR [Fixed reset phone email data error and removed dom API from ResetModal.js](https://github.com/casbin/casdoor/pull/56)
- Open PR [feat: add reset Email and phone by using verification code.](https://github.com/casbin/casdoor/pull/55)
- Open PR [Add Tencent Cloud and Aliyun as providers](https://github.com/casdoor/go-sms-sender/pull/1)
- Open PR [feat: set password for users](https://github.com/casbin/casdoor/pull/54)
- Open PR [feat: update crawler to v0.0.6 to remove quote from replies](https://github.com/casbin/casnode/pull/226)
- Open PR [feat: removed gmail quote from replies](https://github.com/casbin/google-groups-crawler/pull/6)
- Open PR [feat: auto create user during synchronization from Google Groups](https://github.com/casbin/casnode/pull/225)
- Open PR [fix: added some judgement to avoid crashing](https://github.com/casbin/google-groups-crawler/pull/5)
- Open PR [feat: add author name to email mapping](https://github.com/casbin/google-groups-crawler/pull/4)
- Open PR [feat: add architecture overview, main-package, routers, controllers](https://github.com/casnode/casnode-website/pull/11)
- Open PR [Add low version SQL support to search](https://github.com/casbin/casnode/pull/295)
- Open PR [Removed keywords in html Tags when searching](https://github.com/casbin/casnode/pull/287)
- Open PR [Add nil judgements to balance and notification backend to avoid crashing and fixed avatar error in search results](https://github.com/casbin/casnode/pull/279)
- Open PR [feat: add get started page](https://github.com/casnode/casnode-website/pull/7)
- Open PR [feat: in-site search for topics](https://github.com/casbin/casnode/pull/273)
- Open PR [fix: update crawler to v0.1.2 to avoid crashing](https://github.com/casbin/casnode/pull/268)
- Open PR [fix: signup bug](https://github.com/casbin/casnode/pull/267)
- Open PR [fix: http response nil](https://github.com/casbin/google-groups-crawler/pull/17)
- Open PR [feat: convert to casdoor go sdk](https://github.com/casbin/casnode/pull/263)
- Open PR [fix: remove panic in internet errors to avoid crashing](https://github.com/casdoor/casdoor-go-sdk/pull/2)
- Open PR [fix: redirect urls null in app-built-in](https://github.com/casbin/casdoor/pull/85)
- Open PR [feat: add update delete User](https://github.com/casdoor/casdoor-go-sdk/pull/1)
- Open PR [fix: wrong usement in getUsername](https://github.com/casbin/casdoor/pull/84)
- Open PR [feat: authorize via clientId and clientSecret](https://github.com/casbin/casdoor/pull/76)
- Open PR [Basic Code](https://github.com/casnode/casnode-embed-js/pull/2)
- Open PR [fix: nginx reload command](https://github.com/casdoor/casdoor-website/pull/37)
- Open PR [feat: expose email and sms APIs as services to SDK](https://github.com/casbin/casdoor/pull/202)
- Open PR [feat: integrate Storage config into providers](https://github.com/casbin/casdoor/pull/198)
- Open PR [feat: integrate OSS into providers](https://github.com/casbin/casdoor/pull/196)
- Open PR [feat: use Casdoor SDK to signup](https://github.com/casbin/casnode/pull/329)
- Open PR [Customized OAuth provider login URL](https://github.com/casbin/casdoor/pull/185)
- Open PR [Added Casdoor JS SDK](https://github.com/casbin/casnode/pull/321)
- Open PR [feat: add react lazyload](https://github.com/casbin/casnode/pull/319)
- Open PR [feat: auto login session will expire after 24h](https://github.com/casbin/casdoor/pull/153)
- Open PR [feat: support SAML okta login](https://github.com/casbin/casdoor/pull/149)
- Open PR [feat: add auto release CI](https://github.com/node-casbin/redis-adapter/pull/6)
- Open PR [added a new Button component.](https://github.com/casbin/static/pull/18)
- Open PR [feat: add Button component](https://github.com/casbin/casnode/pull/316)
- Open PR [feat: add semantic release](https://github.com/casdoor/casdoor-js-sdk/pull/7)
- Open PR [fix: code mirror editor display bugs](https://github.com/casbin/casnode/pull/363)
- Open PR [feat: Hide Nodes and Topics](https://github.com/casbin/casnode/pull/358)
- Open PR [feat: support popup window](https://github.com/casdoor/casdoor-js-sdk/pull/6)
- Open PR [feat: use casdoor-js-sdk to signin and signup](https://github.com/casbin/casbin-oa/pull/14)
- Open PR [fix: add nodeId in getTopicByUrlAndTitle](https://github.com/casbin/casnode/pull/357)
- Open PR [feat: add Casnode embed plguin](https://github.com/casbin/casbin-oa/pull/13)
- Open PR [feat: change provider name](https://github.com/casdoor/go-sms-sender/pull/3)
- Open PR [feat: login in embed reply](https://github.com/casbin/casnode/pull/354)
- Open PR [feat: add VolcEngine as a SMS provider](https://github.com/casbin/casdoor/pull/252)
- Open PR [feat: update go-sms-sender to v0.0.2 to support VolcEngine](https://github.com/casbin/casdoor/pull/249)
- Open PR [feat: add router and APIs for embed plugin](https://github.com/casbin/casnode/pull/351)
- Open PR [Embed plugin](https://github.com/casbin/casnode/pull/350)
- Open PR [Add API to get topic by url and title](https://github.com/casbin/casnode/pull/347)
- Open PR [feat: modified user, and return user directly](https://github.com/casdoor/casdoor-go-sdk/pull/9)
- Open PR [feat: add user initScore in app.conf](https://github.com/casbin/casdoor/pull/220)
- Open PR [feat: add user initScore in app.conf](https://github.com/casbin/casnode/pull/342)
- Open PR [refactor: use ResponseError to response error](https://github.com/casbin/casdoor/pull/218)
- Open PR [fix: use ReponseError to response error](https://github.com/casbin/casdoor/pull/213)

## Review Pull Requests

- Review PR [fix: all codemirror focus area](https://github.com/casbin/casnode/pull/247#pullrequestreview-671704807)
- Review PR [fix: Perfect the function of add node](https://github.com/casbin/casnode/pull/246#pullrequestreview-671422215)
- Review PR [feat: add "forget password" [front & backend]](https://github.com/casbin/casdoor/pull/72#pullrequestreview-670363168)
- Review PR [feat: Add new member in UI #194](https://github.com/casbin/casnode/pull/242#pullrequestreview-669771999)
- Review PR [fix: codemirror textarea focus area](https://github.com/casbin/casnode/pull/241#pullrequestreview-669764803)
- Review PR [feat:Add advertisement banners to the forum](https://github.com/casbin/casnode/pull/238#pullrequestreview-668703725)
- Review PR [feat: add password Item forcibly](https://github.com/casbin/casdoor/pull/132#pullrequestreview-693382559)
- Review PR [add check node info on add new topic and reply](https://github.com/casbin/casnode/pull/313#pullrequestreview-692457034)
- Review PR [fix: improve i18n and add some translation](https://github.com/casbin/casdoor/pull/117#pullrequestreview-689435765)
- Review PR [fix: Return to the previous page after language selection](https://github.com/casbin/casnode/pull/305#pullrequestreview-689177408)
- Review PR [fix: sync google group attachment files to links](https://github.com/casbin/casnode/pull/303#pullrequestreview-687907113)
- Review PR [fix: sync attachment files](https://github.com/casbin/google-groups-crawler/pull/18#pullrequestreview-687886977)
- Review PR [feat: add member and poster swagger api docs](https://github.com/casbin/casnode/pull/298#pullrequestreview-687827018)
- Review PR [fix: Merge the same translation words #108](https://github.com/casbin/casdoor/pull/111#pullrequestreview-686355298)
- Review PR [feat: add topic and account swagger api docs](https://github.com/casbin/casnode/pull/285#pullrequestreview-686349241)
- Review PR [feat: migrate beego from v1.x to v2.x](https://github.com/casbin/casnode/pull/284#pullrequestreview-686070533)
- Review PR [fix: user balance not updated after login #276](https://github.com/casbin/casnode/pull/283#pullrequestreview-685314829)
- Review PR [feat: Allow the user to sign up with OAuth](https://github.com/casbin/casdoor/pull/107#pullrequestreview-684038545)
- Review PR [fix: The administrator cannot enter the administrator background #275](https://github.com/casbin/casnode/pull/277#pullrequestreview-682790770)
- Review PR [fix: Fix the debug warnings in browser console #269](https://github.com/casbin/casnode/pull/274#pullrequestreview-682413949)
- Review PR [feat: Allow the user to sign up with OAuth](https://github.com/casbin/casdoor/pull/102#pullrequestreview-682411165)
- Review PR [fix: Fix the bug that the ad doesn't show up for anonymous access #270](https://github.com/casbin/casnode/pull/272#pullrequestreview-682331150)
- Review PR [feat: allow user signup with oauth](https://github.com/casbin/casdoor/pull/92#pullrequestreview-678096847)
- Review PR [feat: Add auto-generated topic tags](https://github.com/casbin/casnode/pull/259#pullrequestreview-676278571)
- Review PR [feat: users can add tags to posts when they publish them #197](https://github.com/casbin/casnode/pull/253#pullrequestreview-674420635)
- Review PR [feat: add "forget password" [front & backend]](https://github.com/casbin/casdoor/pull/75#pullrequestreview-673767651)
- Review PR [refactor: change jsdelivr to cdn.casbin.org](https://github.com/casbin/casnode/pull/340#pullrequestreview-718769483)
- Review PR [fix: topic author error and memberbox fresh](https://github.com/casbin/casnode/pull/336#pullrequestreview-716738317)
- Review PR [feat: implement the basic framework of casbin-oa app](https://github.com/casdoor/casbin-oa-android/pull/9#pullrequestreview-716734713)
- Review PR [refactor: change cdn](https://github.com/casbin/casnode/pull/335#pullrequestreview-715497704)
- Review PR [feat: general implement](https://github.com/casdoor/casdoor-js-sdk/pull/2#pullrequestreview-714789168)
- Review PR [feat: improve ApplicationEditPage](https://github.com/casbin/casdoor/pull/189#pullrequestreview-713620711)
- Review PR [feat: fix header bugs, improve UI, optimize touchscreen experience](https://github.com/casbin/casdoor/pull/187#pullrequestreview-713054019)
- Review PR [feat: You can get Prs from github automatically](https://github.com/casbin/casbin-oa/pull/6#pullrequestreview-703997675)
- Review PR [feat: support LDAP](https://github.com/casbin/casdoor/pull/160#pullrequestreview-703919548)
- Review PR [feat: Add log table and record all user behaviors into the table, add UI to view logs](https://github.com/casbin/casdoor/pull/150#pullrequestreview-702138627)
- Review PR [feat: support 2fa](https://github.com/casbin/casdoor/pull/148#pullrequestreview-700657858)
- Review PR [feat: support login UI customization](https://github.com/casbin/casdoor/pull/146#pullrequestreview-700574269)
- Review PR [feat: add installation by BT panel](https://github.com/casnode/casnode-website/pull/17#pullrequestreview-699333174)
- Review PR [Add the code.](https://github.com/casdoor/casdoor-php-sdk/pull/2#pullrequestreview-699328336)
- Review PR [feat: Install Casnode by BT panel](https://github.com/casbin/casnode/pull/317#pullrequestreview-698680205)
- Review PR [fix: Correct a small bug](https://github.com/casbin/casbin-oa/pull/4#pullrequestreview-698567609)
- Review PR [feat: Integrate Crowdin](https://github.com/casbin/casdoor/pull/143#pullrequestreview-698564255)
- Review PR [feat: add frontconf management](https://github.com/casbin/casnode/pull/348#pullrequestreview-728195139)
- Review PR [Add VolcEngine SMS service support](https://github.com/casdoor/go-sms-sender/pull/2#pullrequestreview-727139514)
- Review PR [fix:  table errors in frontend](https://github.com/casbin/casdoor/pull/239#pullrequestreview-725974995)
- Review PR [feat: support local storage](https://github.com/casbin/casdoor/pull/224#pullrequestreview-724808035)
- Review PR [feat: mount app.conf to container](https://github.com/casbin/casdoor/pull/217#pullrequestreview-720401001)