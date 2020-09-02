How this site was created:

 1. Create a static site (no sales, forms, databases, etc.)
 1. Purchase a domain name/mail boxes from Gandi
 1. Upload static HTML, CSS, JavaScript to GitHub
 1. Upload big media files to the Internet Archive
 1. Configure DNS and GitHub Pages to work together

Services exploited:

 * Gandi handles name servers for a small fee
 * GitHub accepts traffic to your site with proper DNS settings
 * GitHub Pages serves static files from your domain name for free
 * Internet Archive serves big files for free (forever)

Advantages:

 * Analytics: analytics.google.com if you must
 * Backup and revision history: $0
 * Domain + email cost: $0-20 per year
 * Fast, durable servers: $0
 * High availability: $0
 * Hosting cost: $0


----


Create a static site
--------------------

It can be as sophisticated as you like as long as it has no forms and
does not require an app or database server. If what you have works in
a browser when not connected to the internet, then this method will
work for you. There is a lot you can do with static HTML and
client-side JavaScript. See
https://developer.mozilla.org/en-US/docs/Web/JavaScript


Purchase a domain name
----------------------

I got my domain from gandi.net (based in France) which does not share
your information or cooperate with authorities to seize your
domain if they want to for some reason. It also has a nice DNS
Settings interface.


DNS settings
------------

Change Gandi domain to LiveDNS. See
https://docs.gandi.net/en/domain_names/common_operations/changing_nameservers.html

**A record** entries with LiveDNS:

```
@ 1800 IN A 185.199.108.153 
@ 1800 IN A 185.199.109.153 
@ 1800 IN A 185.199.110.153 
@ 1800 IN A 185.199.111.153
```

**CNAME record**

Remove all but the "Web forwarding" CNAME entry. It can take up to 24
hours for this change to enable the "Enforce HTTPS" option on your
GitHub Pages site.

```
www 10800 IN CNAME webredir.vip.gandi.net.
```

Verifying can take up to 24 hours:

```
$ dig +noall +answer siznax.net
```


Upload your source code to GitHub Pages
---------------------------------------

See https://pages.github.com/

1. Create a GitHub account
1. Create a new GitHub pages repository by uploading your static files
1. See github.com/siznax/www for a working example

GitHub has a nice desktop app to make things clicky if you prefer:
https://desktop.github.com/


Make your GitHub site and Domain registrar work together
--------------------------------------------------------

1. Update DNS settings as described above
   https://help.github.com/articles/setting-up-an-apex-domain/
1. Remove and re-add your custom domain
   https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site


Optionally enable HTTPS
-----------------------

This is optional but can help protect your visitors from "spying for
free at all times" if you do _not_ also use Google Analytics (which
offers spying for free at all times). See
https://observer.com/2011/12/in-which-eben-moglen-like-legit-yells-at-me-for-being-on-facebook-2/

At first, the option to "Enforce HTTPS" on my site settings in GitHub
was not available because I had more than one subdomain (other than
"www") in my DNS record. Gandi enabled several subdomains with CNAME
entries by default, like "blog", "imap", "pop", etc. Remove them as
described above.

See also:

1. https://help.github.com/articles/securing-your-github-pages-site-with-https/
1. https://help.github.com/articles/troubleshooting-custom-domains/


Upload/serve big files to/from the Internet Archive
---------------------------------------------------

* Create an archive.org account at https://archive.org/account/signup
* Create a new "item" and upload your files. You can upload directly
  to the site at https://archive.org/create/ or you can use the
  awesome python library at 
  https://archive.org/services/docs/api/internetarchive/
* Point to your source code to your Archive assets using the
  "/download/" API.

Upload example:

```
$ workon www  # virtualenv
[www]$ pip install --update internetarchive
[www]$ ia upload image.jpeg my_item
```

Source code example:

```
<img src="https://archive.org/download/my_item/image.jpeg">
```

Hope that helps!


@siznax
