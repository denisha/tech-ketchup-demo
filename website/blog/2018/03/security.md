# Security

## Browsers about to flag all insecure pages

Browsers are changing how they are handling and showing insecure websites. Chrome and Firefox has already been updated to flag any form password fields on an insecure page to the user (to the purported [detriment of at least one company](https://www.bleepingcomputer.com/news/security/developer-complains-firefox-labels-his-site-as-insecure-hilarity-ensues/)). But there are [ways around these checks](https://www.troyhunt.com/bypassing-browser-security-warnings-with-pseudo-password-fields/). Soon though any page that does not serve over HTTPS will be flagged, regardless. This is an important milestone in the history of browser software, bringing with it [it's own mix of lack of understanding masquerading as conspiracy](http://this.how/googleAndHttp/), but should really [be supported](https://www.gdwnet.com/2018/02/25/why-do-i-support-https-everywhere/).

### HSTS, CSP and Expect-CT

These three headers have become important in the secure-website checklist. [HSTS](https://www.troyhunt.com/understanding-http-strict-transport/) provides a mechanism to preload your website as serving over HTTPS, so that no insecure handshake has to happen to determine that fact. This removes a possible gap for a MITM attack. [CSP](https://www.netsparker.com/blog/web-security/content-security-policy/) controls the sources of your content, be that images, scripts or anything else. The front-end team has already seen how this can prevent malicious script injection. [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/) is still in report-only beta, but is slated to become completely enforceable within 2018. GK already have our [own reporting service](https://opsec.globalkinetic.com) setup for this.

### Testing sites

There are a few services out there to test and report on your site's hardiness.

* [securityheaders.io](https://securityheaders.io) is one that will report on your response headers.
* [SSL Labs](https://www.ssllabs.com/ssltest/) provides a more complete service, checking the configured cipher suites and checking your web server itself for known day-zero and unpatched vulnerabilities, like BEAST and heartbleed.
* [passmarked.com](https://passmarked.com/) is another service that provides invaluable information wrt to the state of your site.

## Let's Encrypt

If you do run a personal website, blog, wordpress site or similar, it is a good idea to start looking into making it secure. Companies prefer to buy certificates from CAs, which show their name (EV, OV types). Most of us, however, can get away with a DV certificate. This is where [Let's Encrypt](https://letsencrypt.org/) comes in. It's automated so once you're setup there shouldn't be a need to manually renew expired certs. And it's free. However, in some cases [it is not that trivial or that free](https://www.troyhunt.com/everything-you-need-to-know-about-loading-a-free-lets-encrypt-certificate-into-an-azure-website/).

## Facebook

Facebook is [ceiling cat](https://threatpost.com/facebook-woes-continue-as-ftc-opens-data-privacy-probe/130788/)  

<img src="ceiling_cat.jpg" alt="meow"/>
