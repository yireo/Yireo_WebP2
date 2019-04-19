# How do I know WebP is used?
Make sure to test things with the obvious caches disabled (Full Page Cache, Block HTML Cache). Once this extension is working, catalog images (like on a category page) should be replaced with: Their `<img>` tag should be replaced with a `<picture>` tag that lists both the old image and the new WebP image. If the conversion from the old image to WebP goes well.

You can expect the HTML to be changed, so inspecting the HTML source gives a good impression. You can also use the Error Console to inspect network traffic: If some `webp` images are flying be in a default Magento environment, this usually proofs that the extension is working to some extent.

# After installation, I'm still seeing only PNG and JPEG images
This could mean that the conversion failed. New WebP images are stored in the same path as the original path (somewhere in `pub/`) which means that all folders need to be writable by the webserver. Specifically, if your deployment is based on artifacts, this could be an issue.

Also make sure that your PHP environment is capable of WebP: The function `imagewebp` should exist in PHP and we recommend a `cwebp` binary to be placed in `/usr/local/bin/`.

Last but not least, WebP images only work in WebP-capable browsers. The extension detects the browser support. Make sure to test this in Chrome first, which natively supports WebP.

# Some of the images are converted, but others are not.
Not all JPEG and PNG images are fit for conversion to WebP. In the past, WebP has had issues with alpha-transparency and partial transparency. If the WebP image can't be generated by our extension, it is not generated. Simple as that. If some images are converted but some are not, try to upload those to online conversion tools to see if they work.

Make sure your `cwebp` binary and PHP environment are up-to-date.

# This sucks. It only works in some browsers.
Don't complain to us. Instead, ask the other browser vendors to support this as well. And don't say this
is not worth implementing, because I bet more than 25% of your sites visitors will benefit from WebP. Not
offering this to them, is wasting additional bandwidth.