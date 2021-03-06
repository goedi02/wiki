+++
title = "Same-Site Cookies"
description = ""
date = "2020-10-01"
category = [
    "Defense",
]
menu = "main"
+++

Same-Site cookies are one of the most impactful modern security mechanisms for fixing security issues that involve cross-site requests. This mechanism allows applications to make browsers only include cookies in requests that are issued same-site [^1]. This type of cookies has three modes: `None`, `Lax`, and `Strict`.

## Same-Site Cookie Modes

`None` disables all protections and restores the old behavior of cookies. This mode is not recommended. 

{{< hint warning >}}
The `None` attribute must come with the `Secure` flag [^same-site-none].
{{< /hint >}}


`Strict` causes the browser to not include cookies in any cross-site requests. This means `<script src="example.com/resource">`, `<img src="example.com/resource">`, `fetch()`, and `XHR` will all make requests without the same-site `Strict` cookies attached. Even if the user clicks on a link to `example.com/resource` then their cookies will not be included. 

The only difference between `Lax` and `Strict` is that `Lax` mode allows cookies to be added to requests triggered by top-level navigations. This makes `Lax` cookies much easier to deploy since they won't break incoming links to your application. Unfortunately, an attacker can trigger a top-level navigation via `window.open` that allows the attacker to maintain a reference to the `window` object. 

## Considerations

`Strict` cookies provide the strongest security guarantees, but it can be very difficult to deploy `Strict` same-site cookies in an existing application. 

Same-Site cookies are not bulletproof [^2] nor they can fix everything. To complete the defense strategy against XS-Leaks users should consider implementing other protections that complement same-site cookies. For example, [COOP]({{< ref "coop.md" >}}) can prevent an attacker from controlling pages using a `window` reference even if `Lax` Same-Site Cookies are used.

## Deployment

Anyone interested in deploying this mechanism in web applications should take a careful look at this [web.dev](https://web.dev/samesite-cookie-recipes/) article.

## References

[^1]: SameSite cookies explained, [link](https://web.dev/samesite-cookies-explained/)
[^2]: Bypass SameSite Cookies Default to Lax and get CSRF, [link](https://medium.com/@renwa/bypass-samesite-cookies-default-to-lax-and-get-csrf-343ba09b9f2b)
[^same-site-none]: SameSite cookies explained, [link](https://web.dev/samesite-cookies-explained/#samesitenone-must-be-secure)
