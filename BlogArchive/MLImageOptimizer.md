# MLImageOptimizer

**使用 [`pngquant`](https://github.com/kornelski/pngquant)、[`jpegoptim`](https://github.com/tjko/jpegoptim)
、[`gifsicle`](https://github.com/kohler/gifsicle) 快速批量递归压缩 png、jpg、gif 图片 ( 支持无损压缩和有损压缩 )**

## 自动递归压缩图片（默认开启无损压缩）

两种脚本：
`optimizer.sh` 压缩完毕的图片会存储到原图 `<dir>` 同级目录下的文件夹 `<dir-optimized>` 内；
`replace.sh` 脚本压缩图片并直接替换原图

打开终端，cd 到当前目录，然后执行 shell 脚本：
```
$ sh optimizer.sh <dir>
or
$ ./optimizer.sh <dir>

$ sh replace.sh <dir>
or
$ ./replace.sh <dir>
```
当然也支持在其他路径下执行，无需cd到当前目录：
```
// 使用replace.sh脚本压缩图片并直接替换原图
$ sh /Users/mountainx/Documents/MLImageOptimizer/replace.sh /Users/mountainx/Documents/Images

// 使用optimizer.sh脚本压缩图片，保留原图，并将压缩后的图片保存在原图同级目录下的文件夹 `<dir-optimized>` 内
$ sh /Users/mountainx/Documents/MLImageOptimizer/optimizer.sh /Users/mountainx/Documents/Images
```

## 备注

**有损压缩比例 or 无损压缩可以在 `optimizer.sh` 和 `replace.sh` 中进行修改，参照下面的 `pngquant` 、 `jpegoptim` 、 `gifsicle` 的命令选项**

`if use '$ ./optimizer.sh <dir>', command line tool print error: 'Permission denied', please modify privilege using '$ chmod 777 optimizer.sh'`

## pngquant 命令选项
```
$ pngquant -h
pngquant, 2.12.3 (January 2019), by Kornel Lesinski, Greg Roelofs.
Color profiles are supported via Little CMS. Using libpng 1.6.26.

usage:  pngquant [options] [ncolors] -- pngfile [pngfile ...]
pngquant [options] [ncolors] - >stdout <stdin

options:
--force           overwrite existing output files (synonym: -f)
--skip-if-larger  only save converted files if they're smaller than original
--output file     destination file path to use instead of --ext (synonym: -o)
--ext new.png     set custom suffix/extension for output filenames
--quality min-max don't save below min, use fewer colors below max (0-100)
--speed N         speed/quality trade-off. 1=slow, 4=default, 11=fast & rough
--nofs            disable Floyd-Steinberg dithering
--posterize N     output lower-precision color (e.g. for ARGB4444 output)
--strip           remove optional metadata (default on Mac)
--verbose         print status messages (synonym: -v)

Quantizes one or more 32-bit RGBA PNGs to 8-bit (or smaller) RGBA-palette.
The output filename is the same as the input name except that
it ends in "-fs8.png", "-or8.png" or your custom extension (unless the
input is stdin, in which case the quantized image will go to stdout).
If you pass the special output path "-" and a single input file, that file
will be processed and the quantized image will go to stdout.
The default behavior if the output file exists is to skip the conversion;
use --force to overwrite. See man page for full list of options.
```

## jpegoptim 命令选项
```
$ jpegoptim -h
jpegoptim v1.4.6  Copyright (C) 1996-2018, Timo Kokkonen
Usage: jpegoptim [options] <filenames> 

-d<path>, --dest=<path>
specify alternative destination directory for 
optimized files (default is to overwrite originals)
-f, --force       force optimization
-h, --help        display this help and exit
-m<quality>, --max=<quality>
set maximum image quality factor (disables lossless
optimization mode, which is by default on)
Valid quality values: 0 - 100
-n, --noaction    don't really optimize files, just print results
-S<size>, --size=<size>
Try to optimize file to given size (disables lossless
optimization mode). Target size is specified either in
kilo bytes (1 - n) or as percentage (1% - 99%)
-T<threshold>, --threshold=<threshold>
keep old file if the gain is below a threshold (%)
-b, --csv         print progress info in CSV format
-o, --overwrite   overwrite target file even if it exists (meaningful
only when used with -d, --dest option)
-p, --preserve    preserve file timestamps
-P, --preserve-perms
preserve original file permissions by overwriting it
-q, --quiet       quiet mode
-t, --totals      print totals after processing all files
-v, --verbose     enable verbose mode (positively chatty)
-V, --version     print program version

-s, --strip-all   strip all markers from output file
--strip-none      do not strip any markers
--strip-com       strip Comment markers from output file
--strip-exif      strip Exif markers from output file
--strip-iptc      strip IPTC/Photoshop (APP13) markers from output file
--strip-icc       strip ICC profile markers from output file
--strip-xmp       strip XMP markers markers from output file

--all-normal      force all output files to be non-progressive
--all-progressive force all output files to be progressive
--stdout          send output to standard output (instead of a file)
--stdin           read input from standard input (instead of a file)
```

## gifsicle 命令选项
```
$ gifsicle -h
'Gifsicle' manipulates GIF images. Its most common uses include combining
single images into animations, adding transparency, optimizing animations for
space, and printing information about GIFs.

Usage: gifsicle [OPTION | FILE | FRAME]...

Mode options: at most one, before any filenames.
-m, --merge                   Merge mode: combine inputs, write stdout.
-b, --batch                   Batch mode: modify inputs, write back to
same filenames.
-e, --explode                 Explode mode: write N files for each input,
one per frame, to 'input.frame-number'.
-E, --explode-by-name         Explode mode, but write 'input.name'.

General options: Also --no-OPTION for info and verbose.
-I, --info                    Print info about input GIFs. Two -I's means
normal output is not suppressed.
--color-info, --cinfo     --info plus colormap details.
--extension-info, --xinfo --info plus extension details.
--size-info, --sinfo      --info plus compression information.
-V, --verbose                 Prints progress information.
-h, --help                    Print this message and exit.
--version                 Print version number and exit.
-o, --output FILE             Write output to FILE.
-w, --no-warnings             Don't report warnings.
--no-ignore-errors        Quit on very erroneous input GIFs.
--conserve-memory         Conserve memory at the expense of speed.
--multifile               Support concatenated GIF files.

Frame selections:               #num, #num1-num2, #num1-, #name

Frame change options:
--delete FRAMES               Delete FRAMES from input.
--insert-before FRAME GIFS    Insert GIFS before FRAMES in input.
--append GIFS                 Append GIFS to input.
--replace FRAMES GIFS         Replace FRAMES with GIFS in input.
--done                        Done with frame changes.

Image options: Also --no-OPTION and --same-OPTION.
-B, --background COL          Make COL the background color.
--crop X,Y+WxH, --crop X,Y-X2,Y2
Crop the image.
--crop-transparency       Crop transparent borders off the image.
--flip-horizontal, --flip-vertical
Flip the image.
-i, --interlace               Turn on interlacing.
-S, --logical-screen WxH      Set logical screen to WxH.
-p, --position X,Y            Set frame position to (X,Y).
--rotate-90, --rotate-180, --rotate-270, --no-rotate
Rotate the image.
-t, --transparent COL         Make COL transparent.

Extension options:
--app-extension N D       Add an app extension named N with data D.
-c, --comment TEXT            Add a comment before the next frame.
--extension N D           Add an extension number N with data D.
-n, --name TEXT               Set next frame's name.
--no-comments, --no-names, --no-extensions
Remove comments (names, extensions) from input.
Animation options: Also --no-OPTION and --same-OPTION.
-d, --delay TIME              Set frame delay to TIME (in 1/100sec).
-D, --disposal METHOD         Set frame disposal to METHOD.
-l, --loopcount[=N]           Set loop extension to N (default forever).
-O, --optimize[=LEVEL]        Optimize output GIFs.
-U, --unoptimize              Unoptimize input GIFs.
-j, --threads[=THREADS]       Use multiple threads to improve speed.

Whole-GIF options: Also --no-OPTION.
--careful                 Write larger GIFs that avoid bugs in other
programs.
--change-color COL1 COL2  Change COL1 to COL2 throughout.
-k, --colors N                Reduce the number of colors to N.
--color-method METHOD     Set method for choosing reduced colors.
-f, --dither                  Dither image after changing colormap.
--gamma G                 Set gamma for color reduction [2.2].
--lossy[=LOSSINESS]       Alter image colors to shrink output file size
at the cost of artifacts and noise.
--resize WxH              Resize the output GIF to WxH.
--resize-width W          Resize to width W and proportional height.
--resize-height H         Resize to height H and proportional width.
--resize-fit WxH          Resize if necessary to fit within WxH.
--scale XFACTOR[xYFACTOR] Scale the output GIF by XFACTORxYFACTOR.
--resize-method METHOD    Set resizing method.
--resize-colors N         Resize can add new colors up to N.
--transform-colormap CMD  Transform each output colormap by shell CMD.
--use-colormap CMAP       Set output GIF's colormap to CMAP, which can
be 'web', 'gray', 'bw', or a GIF file.

Report bugs to <ekohler@gmail.com>.
Too much information? Try 'gifsicle --help | more'.
```
