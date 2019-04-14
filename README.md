# Magento 2 module for WebP
This module adds WebP support to Magento 2. Currently, it ships with the following features:

- Browser-support for WebP is detected in various ways: A simple check for Chrome, an ACCEPT header sent by the browser and a JavaScript check generating a `webp` cookie for all browsers that remain.
- When `<img>` tags are found on the page, the corresponding JPG or PNG is converted into WebP and a corresponding `<picture` tag is used to replace the original `<img>` tag.
- The Fotorama gallery of the Magento core product pages is replaced with WebP images, as long as the Full Page Cache is disabled. Unfortunately, with the FPC, the whole JavaScript needs to be refactored.

### System requirements
Make sure your PHP environment supports WebP: This means that the function `imagewebp` should exist in PHP. We hope to add more checks for this in the extension itself soon. For now, just open up a PHP `phpinfo()` page and check for WebP support. Please note that installing `libwebp` on your system is not the same as having PHP support WebP. Check the `phpinfo()` file and add new PHP modules to PHP if needed. If in doubt, simple create a PHP script `test.php` and a line `<?php echo (int)function_exists('imagewebp');` to it: A `1` indicates that the function is available, a `0` indicates that it is not.

An alternative is that the `cwebp` binary from the WebP project is uploaded to your server and placed in a generic folder like `/usr/local/bin`. Make sure to grab a copy from this binary from the [rosell-dk/webp-convert](https://github.com/rosell-dk/webp-convert/tree/master/src/Converters/Binaries) project. This method is preferred because it is the fastest. But it assumes also that the binary is placed in a folder by the server administrator.

We recommend you to work on making all options work, not just one.

Please note that both tasks should be simple for developers and system administrator, but might be magical for non-technical people. If this extension is not working out of the box for you, most likely a technical person needs to take a look at your hosting environment.

### Instructions for using composer
Use composer to install this extension. First make sure that Magento is installed via composer, and that there is a valid `composer.json` file present.

Next, install our module using the following command:

    composer require yireo/magento2-webp2

Next, install the new module into Magento itself:

    ./bin/magento module:enable Yireo_WebP2
    ./bin/magento setup:upgrade

Enable the module by toggling the setting in **Stores > Configuration > Advanced > System > Yireo WebP > Enabled**.

Done.

### Requesting support
Feel free to open an **Issue** here on GitHub. However, do make sure to be thorough and mention the following:

- Your browser;
- Your Magento version;
- Your PHP version including an indication of it supporting WebP;
- The location of the `cwebp` binary on your system;
- Preferably an URL to a live demo;
- An indication on which Magento caches are enabled or disabled;

### FAQ
#### How do I know WebP is used?
Make sure to test things with the obvious caches disabled (Full Page Cache, Block HTML Cache). Once this extension is working, catalog images (like on a category page) should be replaced with: Their `<img>` tag should be replaced with a `<picture>` tag that lists both the old image and the new WebP image. If the conversion from the old image to WebP goes well.

You can expect the HTML to be changed, so inspecting the HTML source gives a good impression. You can also use the Error Console to inspect network traffic: If some `webp` images are flying be in a default Magento environment, this usually proofs that the extension is working to some extent.

#### After installation, I'm still seeing only PNG and JPEG images
This could mean that the conversion failed. New WebP images are stored in the same path as the original path (somewhere in `pub/`) which means that all folders need to be writable by the webserver. Specifically, if your deployment is based on artifacts, this could be an issue.

Also make sure that your PHP environment is capable of WebP: The function `imagewebp` should exist in PHP and we recommend a `cwebp` binary to be placed in `/usr/local/bin/`.

Last but not least, WebP images only work in WebP-capable browsers. The extension detects the browser support. Make sure to test this in Chrome first, which natively supports WebP.

#### Some of the images are converted, but others are not.
Not all JPEG and PNG images are fit for conversion to WebP. In the past, WebP has had issues with alpha-transparency and partial transparency. If the WebP image can't be generated by our extension, it is not generated. Simple as that. If some images are converted but some are not, try to upload those to online conversion tools to see if they work.

Make sure your `cwebp` binary and PHP environment are up-to-date.

### Todo
- Add a workaround for FPC with Fotorama
