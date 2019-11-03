# Imager
> Brute force image optimization; optimizes the compression using ML based metrics in a trial ’n error sorta manner.

## About

This is a tool that can competitively optimize (e.g.) extremely noisy, high resolution images; at the expense of increased encoding time and CPU overhead. This is a tradeoff that should be suitable for over 90% of online content, where site performance matters.


## [Benchmarks](https://github.com/colbyn/imager-bench-2019-11-2)

```text
source        : ▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇▇ 39.00M (4 images)
compression.ai: ▇▇▇▇▇▇▇▇ 8.90M
imager        : ▇▇▇▇ 4.20M
```


## Status

Supports any image decodable by `image-rs`. For output targets, currently supports JPEG.

## Aspirations

Nothing short of becoming *the industry standard* for your image optimization needs.


# Install

```shell
$ git clone https://github.com/colbyn/imager.git && cd imager
$ cargo install --path . --force
```

Note that long term wise I’d like to remove cargo from the installation picture for the CLI tool.

# Example

```shell
$ imager opt -i path/to/images/**/*.jpeg -o assets/output/
```

Also supports resizing:
```shell
$ imager opt -i path/to/images/**/*.jpeg -o assets/output/ -s 1200x1200
```

Help:
```shell
$ imager opt --help
```


# Future - Short (to long) Term:

In addition to the preexisting CLI tool and in accordance with “becoming the industry standard” mantra.

Port to every major programming language. Idiomatically following the given languages conventions, including dependency management. What I have in mind is not requiring cargo in the installation picture, and distributing self contained libs. 

So for NodeJS using NAPI (which I have some experience with) computationally expensive work should be offloaded from the main thread to the NodeJS managed thread pool. Following on the JS side, control will return immediately with a promise wrapping the eventual result. I’ve written a macro that does essentially this [here](https://github.com/colbyn/web-images-js/blob/9b766b8bdfccb2c429832e461d2be680b61966c9/src/utils.rs#L116).


Although for now the CLI tool can work everywhere, as is common with FFmpeg.




# Future - Long Term

* [Investigation] Internally, how I use VMAF contradicts the official recommendations (from what little documentation or commentary exists). 

* [Feature] [Advanced] Next-gen video codecs! This can work today in supporting browsers VIA HTML5 video APIs. I think the biggest issue will be that
	1. backend/frontend developers (outside the video streaming world) aren’t accustomed to fragmented codec support. Since e.g. JPEG is practically supported everywhere.
	2. Laymen users copying images will probably expect the download to be something encoded as JPEG. I think browsers send a redundant http request when ‘copying’ an image. So perhaps the request can be intercepted and made to return a JPEG encoded variant. This way we don’t need to do anything that visually or rather noticeably overrides default browser behavior.


**Note that all future aspirations is predominantly predicated on this project getting popular and/or funding (e.g. VIA Patreon).** So if this project is beneficial at your work, let others know about it! :)

<hr/>

Copyright 2019 Colbyn Wadman