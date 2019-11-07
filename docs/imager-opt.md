# Interface
The overall interface is very simple.

There are two `imager` sub-commands:
* `imager opt` - optimizes given files directly
* `imager server`  - http server alternative to `imager opt`.

## `imager opt`
Two flags are important, `-i` and `-o`, for input image file(s), and the output directory, respectively. 

#### [required] `-i` or `--input` 
> input image(s)

Images to be optimized. Can be multiple file paths, and also supports file globs.

E.g. An input glob for both JPEG (`.jpeg` and `.jpg`) and PNG files
```
imager -i images/*.jpeg images/*.jpg images/*.png -o output
```  

#### [required] `-o` or `--output`
> output directory

Where to save the optimized images. If the given directory is missing it will be created automatically. 
> For your sake, `imager opt` will never implicitly override input file paths. 

The image output(s) will always have the same file name as the input image. So e.g. given `imager opt -i input1.jpeg -o output`. The optimized `input1.jpeg` will be saved under `output/input1.jpeg`. 

#### [optional] `-s` or `--size`
> optional max resolution constraint 

If the given image exceeds the given `size` or resolution. Downsize to given dimension. This will always preserve aspect ratio, and will only downsize images. I.e. it will never scale images to a larger resolution (since this isn’t what people commonly want). 

Example:
```shell
imager opt -i path/to/image.jpeg -o output -s 1200x1200
```

#### [optional] `-f` or `—format`
> output format. Default is ‘jpeg’.

Currently only `jpeg` is supported, so this parameter isn’t all that useful. 
 
Example
```shell
imager opt -i path/to/image.jpeg -o output -f jpeg
```

# Examples of `imager opt`

## Basic

To optimize a single image, given some:
* `path/to/image.jpeg`
* `output/path/`

```shell
imager opt -i path/to/image.jpeg -o output/path/
```

The result will then be saved to `output/image.jpeg`.

## Batch - Basic

To optimize multiple images, given some:
* `path/to/image/dir/*.jpeg`
* `output/path/`

```shell
imager opt -i path/to/image/dir/*.jpeg -o output/path/
```

The result will then be saved to `output/path/`; see [output flag](####-[required]-`-o`-or-`--output`) for details.

## Batch - Multiple Input Types

To optimize multiple images, given some:
* `path/to/image/dir/*.jpeg`
* `path/to/image/dir/*.jpg`
* `path/to/image/dir/*.png`
* `output/path/`

```shell
imager opt -i path/to/image/dir/*.jpeg path/to/image/dir/*.jpg path/to/image/dir/*.png -o output/path/
```

The result will then be saved to `output/path/`; see [output flag](####-[required]-`-o`-or-`--output`) for details.

Each output image will have the same file name as the input image. 

## Batch - Recursive Wildcard

To optimize multiple images, given some:
* `path/to/image/dir/**/*.*`
* `output/path/`

```shell
imager opt -i path/to/image/dir/**/*.* -o output/path/
```

The result will then be saved to `output/path/`; see [output flag](####-[required]-`-o`-or-`--output`) for details.


## Help:
```shell
$ imager opt --help
```
