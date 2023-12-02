---
author: "Saief Zneti"
title: "Setting Up My Custom Email Address"
description: "How I moved my main email to my custom domain"
date: 2023-11-24T07:47:15+01:00
draft: false
cover:
    image: "/images/setting-up-my-custom-email-address/poster.jpg"
    alt: "Custom Emails"
    # caption: "<text>"
    relative: false # To use relative path for cover image, used in hugo Page-bundles 
---

For the last 10 years or so, I had a couple of main email accounts that I used personally and professionally. I moved from provider to provider using their free plan such as Gmail, Yahoo and Outlook.

One major issue always existed. It never felt that any of these accounts was mine. Let me explain:

- I was sharing the domain with millions of other users. It doesn't come as a surprise that the email that I wanted was never available to me...
- We're always at the mercy of the email provider since they're offering a free service.
- Aggressive push for Ads with Emails this last few years.

That's why I decided to start looking for a better email hosting service. this post is detailing my journey.

## Context & Requirements: Getting my Own domain

First things first, It was important to get my own domain to configure it with my email provider later on. A few options came to mind:

- Namecheap
- Name.com
- Cloudflare
- Porkbun

Namecheap and Name.com were the most popular of the bunch. However, their yearly renewal costs are quite high so I ruled them out. Porkbun and Cloudflare have a good reputation for having very low renewal costs with no hidden fees later on.

I chose Cloudflare because they offer a very good API, a very good integration with the Dynamic DNS Client I'm using for my home server (Which I will discuss further in another post) in addition to caching capabilities through proxying and very good monitoring.

It was important also that the domain chosen was self-explanatory. Thankfully my first name `saief`
was available in the `.me` domain to give me the full domain `saief.me`. And while this might not seem very smart for obvious privacy concerns. In my case, I was solely using this domain to host my personal stuff, so it was acceptable.

A very good advantage of having your own domain is the ability to add as many emails as you want as long as your email hosting provider allows it. From a technical standpoint adding emails doesn't cost anything so I had to look for an email provider that allows **unlimited Email Addresses**

## Choosing the right Email Provider

After setting up my domain I had to look for an email provider that would host the SMTP server handling my email addresses. And while self-hosting was an option. In the case of email servers it was not ideal for a couple of reasons:

- The IP used should preferably be static. And unfortunately, I do not have a static one.
- Many ISPs forbid hosting email servers on their network.
- Email hosting is a pain to maintain and much effort should be put to maintain a good reputation and to avoid being *blacklisted* by other mail providers.

I decided to go with a more standard email provider. and I checked a couple of options first:

- Outlook
- Google Workspace
- Zoho
- ProtonMail

Some of these providers were on the higher cost end (like Google and Outlook) and some were on the lower end. But they all shared the same problem **They offer too many services with their subscription**. I do not want to pay for a subscription including a calendar or a VPN. I just want an email server with the ability to host my own. Nothing more nothing less.

Another issue is that the number of emails was very limited with many of these providers.

## Considering MXRoute

One particular email provider that was highly praised was MXRoute. Upon taking a closer look, Their policy seemed quite strict at first:

- They do not allow marketing emails in any way, shape or form.
- No Refunds!

Their reasoning however was quite convincing. MXRoute is competing with many big players in the emailing industry. To maintain its reputation and ensure that user emails get sent to their destination in the best conditions. Rules should be enforced.

MXRoute is more targeted at technical users who are not afraid to fiddle with DNS Configurations. Their User interface isn't the most elegant but it gets the job done. And they don't offer any other services besides the ones related to emails.

MXRoute also has very low fees with the ability to go for a lifetime subscription. And they offer an unlimited number of inboxes and email addresses with limited storage. So I decided to give them a shot.

## Getting my email up and running

Upon starting your subscription with MXRoute. an email will be sent to you containing all the information necessary to properly configure your DNS Server:

- a couple of MX Records
- a couple of TXT Records (for SPF and DMARC)

![DNS Records](/images/setting-up-my-custom-email-address/dns_record.png)

In addition to that, an admin dashboard is provided to manage all your email accounts.

<!-- ![Admin Dashboard](/images/setting-up-my-custom-email/admin_dashboard.png) -->

![Admin Dashboard Zoomed](/images/setting-up-my-custom-email-address/admin_dashboard_zoomed.png)

### Setting Up SSL

To maintain a secure connection between the email SMTP server and my email clients (Which I will detail later on) I'm using SSL across my different devices. It was important to enable SSL both over POP3 and IMAP.

Thankfully, MxRoute allows the creation and auto-renewal of [Let's Encrypt](https://letsencrypt.org/) SSL certificates through their Admin Dashboard. How cool is that!

In my DNS Settings above, I added a CNAME record pointing `mail.saief.me` to the SMTP server used by MXRoute. So I only needed to create a certificate for this domain so I can later on be used by my email client. As seen below:

![SSL Certificate](/images/setting-up-my-custom-email-address/ssl_certificate.png)

## Setting up my email chain

To create an account for MXRoute and Cloudflare, an Email needs to be used. However, MxRoute and Cloudflare are needed to get our first email address set up and running...

For these reasons, I created an *Email Chain*: My Custom email is the primary email used. In addition to that, Another email is used to set up the services needed to get my Custom email up and running. A third email is used as a recovery for the second email. The higher we go in the email chain, the more security options are used and the harder it is to log in.

![Email Chain](/images/setting-up-my-custom-email-address/email_chain.png)

the `Gmail` Email is only used for Cloudflare, MXRoute and recovery. The `Yahoo` email is only used for recovery. `zneti@saief.me` is used for most of my personal services.

## Email Client: IMAP or POP3?

![Email Chain](/images/setting-up-my-custom-email-address/pop3_vs_imap.webp)

Two protocols can be used to retrieve emails in my devices. POP3 and IMAP:

- IMAP relies mostly on the **Server**: It synchronizes continuously with the SMTP server. Any reception, deletion or modification is propagated to all the clients. This protocol will use the storage of the SMTP server.

- POP3 relies mostly on the **Clients**: When the client starts fetching emails it fetches emails into the client. These emails are then deleted from the main server and stay locally on the device itself. Any reception, deletion or modification only affects the currently connected client.

One advantage of using POP3 over IMAP is the ability to use a centralized device to pull the emails, then synchronize with the rest of the devices if needed so won't use storage on the SMTP server which is quite limited. This can be set up through `GMAIL` for example by adding our email as a custom email. Since Gmail offers 15GB of free storage.

This [Great Youtube Video](https://www.youtube.com/watch?v=SBaARws0hy4) summarizes how the two protocols work.

IMAP was better for my case since it doesn't rely on any 3rd party services and works across all my devices while keeping them in total sync. So I chose to go with it.

## Email Clients

Email Clients are used to fetch the emails on my different devices. MXRoute offers Roundcube as an IMAP email client integrated, by default, with its admin dashboard. However, in my case, I was looking for a local client that I can install on my machines which can cache some of my emails if needed.

- For My PC, I went with the free and open-source client [Thunderbird](https://www.thunderbird.net)
- Since I have a Samsung device, I went with the [Samsung Email](https://play.google.com/store/apps/details?id=com.samsung.android.email.provider&hl=en&gl=US) App which integrates quite well with the global OS UI.

## Conclusion

To conclude this article, Setting up my custom email was very interesting and rewarding at the same time. It was a great opportunity to learn how email servers work, how to set up SMTP and the different email clients and protocols that can be used. But most importantly, now I have one short, easy to remember and fully customizable email address. So hit me up on `zneti@saief.me` with any suggestions you might have ☺︎.
