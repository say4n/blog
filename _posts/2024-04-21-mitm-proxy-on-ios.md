---
layout: post
title: Setting up `mitmproxy` on iOS
---

## install the `mitmproxy` server

Install the `mitmproxy` server on your platform of choice.
The instructions on [mitmproxy.org](https://docs.mitmproxy.org/stable/overview-installation/) are fairly straightforward to follow!
After installing, run the `mitmproxy` command to start the proxy server.

Note the IP address/hostname (let's call this `proxyhost`) of the computer that is running the proxy server, by default the server listens on port **8080**.
Essentially, we should have the proxy server reachable at `proxyhost:8080` now for any device on the same network as the proxy server.

## configure your iOS device to use the proxy

Head over to Settings > WiFi and then tap on the (i) icon beside the network your are connected to.

**See**: [Screenshot 0](/images/mitm-proxy-on-ios/0.png)

Scroll down to the bottom of the page and then tap on the option that says "Configure Proxy".

**See**: [Screenshot 1](/images/mitm-proxy-on-ios/1.png)

Use the proxy server IP and port from earlier (`proxyhost:8080`) and hit save.

**See**: [Screenshot 2](/images/mitm-proxy-on-ios/2.png)

Open Safar on the iOS device and head over to [mitm.it](mitm.it).
You will be greeted with a page that lists CA certificates for various platforms, tap on the one that is for iOS.

**See**: [Screenshot 3](/images/mitm-proxy-on-ios/3.png)

Tap on allow to download `mitmproxy`'s CA cert to the iOS device's certificate store.

**See**: [Screenshot 4](/images/mitm-proxy-on-ios/4.png)

Choose the device that you want to install the CA certificate too. Most likely you would want to choose the iPhone from the list.

**See**: [Screenshot 5](/images/mitm-proxy-on-ios/5.png)

The CA certificate for `mitmproxy` is now downloaded, we need to head on over to the Settings apps now.

**See**: [Screenshot 6](/images/mitm-proxy-on-ios/6.png)

When you open the Settings app on iOS device, you'll be greeted with a list item titled "Profile Downloaded". Tap on that!

**See**: [Screenshot 7](/images/mitm-proxy-on-ios/7.png)

Then tap on the "Install" button on the top right corner.

**See**: [Screenshot 8](/images/mitm-proxy-on-ios/8.png)

After that head on to Settings > General > About and scroll down to the bottom of the page.
You will see a menu item titled "Certificate Trust Settings".
Tap on it, in the following page enable the certifcate for `mitmproxy`.

**See**: [Screenshot 9](/images/mitm-proxy-on-ios/9.png)

## profit

That's it, you will now be able to intercept SSL traffic from your iOS device on the proxy server. 

## cleaning up

I would recommend disabling or removing the `mitmproxy` CA certificate when not in use, you can simply toggle it in Settings > General > About > Certificate Trust Settings.

You would also need to disable the proxy server configuration for the network whenever you turn off the proxy server.
You can do that by heading over to Settings > WiFi and then tap on the (i) icon beside the network your are connected to.
Scroll down to the bottom of the page and then tap on the option that says "Configure Proxy" and then tap on the "Off" option from the list.

## caveats

You will not be able to interecept SSL traffic for any application that uses certificate pinning.
This is because these applications bypass the system certificate store and have their own trust store for certificates.
See [https://docs.mitmproxy.org/stable/concepts-certificates/#certificate-pinning](https://docs.mitmproxy.org/stable/concepts-certificates/#certificate-pinning) on working around certificate pinning.
