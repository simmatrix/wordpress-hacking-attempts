# Wordpress Hacking Attemps
A compilation of wordpress hacking attempts and the solutions to it

## 1. Auto-redirect to Viagra Site

When people visit my company's site http://netccentric.com/contact-us/, it auto-redirects the visitors to a site selling Viagra supplements. A very degrading act done by some unscrupulous guys.

![Step01](https://raw.githubusercontent.com/simmatrix/wordpress-hacking-attempts/master/images/01.png)

Next is to search through the server log file `/var/log/httpd/<your-site-name>` with the distinct keyword `gofitobishba`. It is very clear that everything started at April 03, 2018.

```
118.189.155.99 - - [03/Apr/2018:18:13:06 +0800] "GET /?gofitobishba=e1e05da3 HTTP/1.1" 302 - "http://netccentric.com/career-available-positions/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"

182.55.174.239 - - [03/Apr/2018:22:11:28 +0800] "GET /?gofitobishba=9675eb0e HTTP/1.1" 302 - "http://netccentric.com/career-meet-the-people/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"

121.7.60.84 - - [03/Apr/2018:22:26:59 +0800] "GET /?gofitobishba=e1e05da3 HTTP/1.1" 302 - "http://netccentric.com/about/" "Mozilla/5.0 (Linux; Android 7.1.1; SM-N950F Build/NMF26X) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.109 Mobile Safari/537.36"
...
```

I searched through `wp-contents` and found no clues, no file changes on April 03, 2018. Next is to search through `wp-includes`, then I found out one of the file `default-constants.php` has been modified on April 03, 2018. 

In order to identify the file difference, first is to check your current Wordpress version (which you can find in your Wordpress Admin Dashboard, within the "At a Glance" box), then download the respective Wordpress version from the archive at https://wordpress.org/download/release-archive/

A quick file comparison with online tool https://www.diffchecker.com/diff clear shows the file difference as illustrated below. 

![Step02](https://raw.githubusercontent.com/simmatrix/wordpress-hacking-attempts/master/images/02.png)

It shows that someone had added the line `@include_once ( ABSPATH . 'wp-admin/includes/' . 'static.php' );` into this file, which lead to the file `../wp-admin/includes/static.php`, a file that doesn't come together with the original Wordpress bundle. It is the file added by some unscrupulous guys, along with some images. 

![Step03](https://raw.githubusercontent.com/simmatrix/wordpress-hacking-attempts/master/images/03.png)

Note that the images are not actually images, when you open the image in your text editor, they are actually some base64-encoded stuff, you can copy and paste the text in some online decoder like https://www.base64decode.org/ and see the hidden content. For example, one of the image file, when decoded, looks like this.

![Step04](https://raw.githubusercontent.com/simmatrix/wordpress-hacking-attempts/master/images/04.png)

And the `static.php` file looks like this. Decode yourself if you're curious to know what content it is. :)

![Step05](https://raw.githubusercontent.com/simmatrix/wordpress-hacking-attempts/master/images/05.png)

So, remove all these files, and your site should be back to normal. Remember to upgrade your Wordpress site to the latest version whenever possible to prevent any vulnerabilities from affecting your sites.