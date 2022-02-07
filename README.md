# cmyk2rgb

This is a simple tool to fix images that were saved with the wrong colorspace so, hopefully, they'll look like they were supposed to.

Background
----------

This tool was originally made to fix some promotional renders of My Little Pony: A New Generation. Most notably, many images shared on [the official VKontacte profile](https://vk.com/mlp) of My Little Pony show the wrong colors. Here's [a post](https://vk.com/photo-57324365_457310359) with an image affected by this problem ([direct link](https://sun9-28.userapi.com/sun9-66/dPf8VqFNEpEyr0unZS6klOgBcK-i4NPqbAuLyQ/vnY1SbSCHuE.jpg), [webarchive](https://web.archive.org/web/20220206235129/https://sun9-28.userapi.com/sun9-66/dPf8VqFNEpEyr0unZS6klOgBcK-i4NPqbAuLyQ/vnY1SbSCHuE.jpg)).

It's assumed that the problem with these images is that they originally included a color profile, but at some point this color profile was lost, so any image viewer will fail to display the images properly. Due to the highly saturated colors observed in these images, the most likely explanation is that the missing color profile was a CMYK color profile.

The process to fix these images requires setting its colorspace to CMYK, attaching a proper color profile, and re-encoding the image to sRGB. This is not a particularly difficult process, but it requires the use of command-line tools such as ImageMagick. This tool's goal is to make this process more accessible for the average user.


Usage
-----

This tool is very simple to use. It only requires to load the webpage and drop one image with wrong colors somewhere in the window. The fixed image will be shown on the bottom of the page, and it will also be downloaded automatically. In case the user wants to convert more images, he or she can drop a new image as soon as the previous image has been downloaded. Images must be dropped one by one. In case the user can't drop images or prefers to use a file picker instead, he or she can click on the text on the top of the page.


Internals
---------

Instead of recreating the usual process, including colorspace changes and the use of ICC profiles, this approach uses a lookup table stored as [a 4096x4096 image](palCMYK.png). This image is simply the result of applying the usual process to [an image with the 16 million colors](palRGB.png) of the RGB colorspace. Thus, the conversion is trivial: knowing the color of any pixel, we can know its "fixed" value simply by checking how this color changed between the reference and the modified table.

Note that the pre-calculed conversion did require the use of ICC profiles. Upon a few experiments with a few profiles, it was decided to use the *cmyk.icc* profile included with [Honeyview 5.46](https://en.bandisoft.com/honeyview/) and the *GIMP built-in sRGB* profile included with [Gimp 2.10.30](https://www.gimp.org/). These profiles are property of their respective owners, they're not included in this repository, and they're not needed to use the tool.


License
-------

cmyk2rgb is licensed under the [MIT License](https://opensource.org/licenses/MIT).

    MIT License
    Copyright (c) 2022 nnuudev

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    THE SOFTWARE.

See also [LICENSE.md](LICENSE.md).