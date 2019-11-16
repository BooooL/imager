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

Something that isn’t benchmarked here that I've been curious about. Presumably, negatively effecting every image related SAAS venture. Latency and bandwidth overhead. Unless I suppose clients are communicating directly to such services instead of VIA an intermediate backend server.


## Status - Fundamental

### Supported **Input** Image Formats

| Format | Decoding |
| ------ | -------- |
| PNG    | All supported color types |
| JPEG   | Baseline and progressive |
| GIF    | Yes |
| BMP    | Yes |
| ICO    | Yes |
| TIFF   | Baseline(no fax support) + LZW + PackBits |
| WebP   | Lossy(Luma channel only) |
| PNM    | PBM, PGM, PPM, standard PAM |

Essentially supports any image decodable by [image-rs](https://github.com/image-rs/image.git).

### Supported **Output** Image Formats

> These are your optimization targets (for lack of a better name). It’s a bit higher level, since e.g. rate control is automatically handled.

| Format | Encoding |
| ------ | -------- |
| JPEG   | progressive |

For now, support will pretty much just correspond to whats popularly available in browsers.

I’m considering `WebP` for the next supported codec.

## Status - Ecosystem

### Supported Operating Systems

| OS     | Current Status |
| ------ | -------- |
| Linux   | ✅ [GOOD] |
| MacOS   | ✅ [GOOD] |
| Windows   | ❌ [UNPRIORITIZED] |

### Supported Languages

| Name | Status | Links | Self Contained (i.e. no sys deps) |
| ------ | -------- | -------- | -------- |
| Rust   | ✅ [GOOD] | [crates](https://crates.io/crates/imager) | NO |
| NodeJS   | ✅ [GOOD] | [npm](https://www.npmjs.com/package/imager-io) - [example](https://github.com/imager-io/imager-nodejs-example) | YES |

### Supported Dev Tools

| Name | Status |
| ------ | -------- |
| Imager CLI   | ✅ [GOOD] |
| Webpack   | ❎ [IN-PROCESS] |


## Objective

Nothing short of becoming *the industry standard* for image optimization! :)

More concretely. Expose a uniform interface for image transcoding and optimization of popular codecs. Based on off-the-shelf encoders, akin to FFmpeg. With support predominately concerned with lossy codecs.

## Top-Level Organization: 
```
.
├── docs
│   ├── imager-nodejs.md
│   ├── imager-opt.md
│   └── imager-server.md
├── extras
│   ├── imager-jpeg2000
│   ├── imager-png
│   └── imager-webp
├── imager-rs
├── plugins
└── ports
    └── nodejs
```
* `./imager-rs`: The core imager codebase for the Rust library, `imager` CLI tool and server.
* `./docs`: Just the general docs.
* `./extras`: Just new stuff under development and not yet integrated with `./imager`, nor officially released.
* `./plugins`: Other developer tools and plugins; nothing officially released yet.
* `./ports`: Language specific ports.
	* `./ports/nodejs`: The NodeJS library. The NPM releases is supposed to be self contained.


# Documentation

> Everything is under `./docs`.

## [CLI Interface - [root]/docs/imager-opt.md](https://github.com/imager-io/imager/blob/master/docs/imager-opt.md)
## [Server Interface - [root]/docs/imager-server.md](https://github.com/imager-io/imager/blob/master/docs/imager-server.md)
## [NodeJS API - [root]/docs/imager-nodejs.md](https://github.com/imager-io/imager/blob/master/docs/imager-nodejs.md)


# CLI Examples

## `imager opt`
> See [docs/imager-opt.md](https://github.com/imager-io/imager/blob/master/docs/imager-opt.md) for all features and their usage examples.

### Basic

```shell
$ imager opt -i path/to/image.jpeg -o output/
```

### Batch

```shell
$ imager opt -i path/to/images/*.jpeg path/to/images/*.jpg path/to/images/*.png -o output/
```

## `imager server`
> See [docs/imager-server.md](https://github.com/imager-io/imager/blob/master/docs/imager-server.md) for details.

### Start Server

```shell
$ imager server --address 127.0.0.1:3030
```

### Client
> This example is using [HTTPie](https://httpie.org).

Given some:
* `path/to/input/image.jpeg`
* `path/to/output` for `path/to/output/image.jpeg`

```shell
$ http 127.0.0.1:3030/opt < path/to/input/image.jpeg > path/to/output/image.jpeg
```

# Imager CLI - Building From Source

## Requirements

* `make` build tool 
* `cargo` build tool
* `c/c++` compiler
* `libclang`
* `libc++` - The C++ standard library

### Step 1. [Cargo](https://rustup.rs)

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### Step 2.

#### For MacOS

```shell
$ brew install llvm
```

#### For Debian-based Linuxes

```shell
$ apt install llvm-dev libclang-dev clang
```

Additionally if you don’t have `make` installed:
```shell
$ apt install build-essential
```

### Step 3. Optional

Report any issues.

## Install

### Step 1. Download

```shell
$ git clone https://github.com/imager-io/imager.git && cd imager/imager
```

### Step 2. Build & Install
> Will install `imager` to `~/.cargo/bin`.

```shell
$ cargo install --path . --force
```

Note that long term wise I’d like to remove cargo from the installation picture for the CLI tool.


# More to come

Stay tuned for future updates. Currently in the works is an http server, and support for other image formats.


# Feedback & Feature Requests

Just use the GitHub issue tracker for this project.

# Other Miscellaneous

## Notes for Developers

* Rust
	* I’d recommend always using the `release` flag when in development. E.g. `cargo run --release`. Otherwise it’ll be significantly slower. 

## Articles

* [Modern Image Optimization for 2020 - Issues, Solutions, and Open Source Solutions](https://medium.com/@colbyn/modern-image-optimization-for-2020-issues-solutions-and-open-source-solutions-543af00e3e51)

## Future - Short (to long) Term

In addition to the preexisting CLI tool and in accordance with “becoming the industry standard” mantra.

Port to every major programming language. Idiomatically following the given languages conventions, including dependency management. What I have in mind is not requiring cargo in the installation picture, and distributing self contained libs. 

## Future - Long Term

* [Investigation] Internally, how I use VMAF contradicts the official recommendations (from what little documentation or commentary exists). 

* [Feature] [Advanced] Next-gen video codecs! This can work today in supporting browsers VIA HTML5 video APIs. I think the biggest issue will be that:
	1. Backend/frontend developers (outside the video streaming world) aren’t accustomed to fragmented codec support. Since e.g. JPEG is practically supported everywhere.
	2. Laymen users copying images will probably expect the download to be something encoded as JPEG. I think browsers send a redundant http request when ‘copying’ an image. So perhaps the request can be intercepted and made to return a JPEG encoded variant. This way we don’t need to do anything that visually or rather noticeably overrides default browser behavior.

**Note that all future aspirations is predominantly predicated on this project getting popular and/or funding (e.g. VIA Patreon).** So if this project is beneficial at your work, let others know about it! :)

## Regarding Imagers SAAS competitors
> This is something I realized from trying to implement a SAAS model.

In my mind SAAS products don’t make sense when it’s competing with a function. Contrarily, database services or team divisions are commonly split into separate services. Yet just resizing images rarely are, unless such is being pushed by SAAS ventures.

Furthermore using a SAAS component (outside official products from the big cloud platforms) entails SAAS specific requirements, the simplest being authorization, so they know *who to bill* on their end. This means the app will go down if something as simple as authorization fails. So environments from development to production will now have a requirement on such. A function or related with no effects, that simply maps images to images doesn’t have this issue. Another is not only offering a REST API, but an SDK that masks over the aforementioned REST API. Since no developer is going to prefer an HTTP based API over something idiomatic and operable from their language of choice.

Overall SAAS for basic image operations feel contrived. It doest e.g. automate scalability pain points and furthermore proposes a deep and fundamental risk of “what if this service that I’m spending time and money in integration goes down temporarily, or even out of business?“.

If there is a real and legitimate market for such feel free to email me about it. Since going forward the only benefit I see is as a means of funding the open source work.

<hr/>

Copyright 2019 Colbyn Wadman