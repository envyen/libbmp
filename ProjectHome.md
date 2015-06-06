# Introduction #
**libbmp** is a simple, cross-platform, open source (revised LGPL) C library designed for easily reading, writing, and modifying Windows bitmap (BMP) image files. The library is oriented towards the novice programmer with little formal experience, but it is sufficiently capable for anybody who desires to do I/O and pixel operations on uncompressed 1, 4, 8, 16, 24, and 32 bpp (bits per pixel) BMP files.

**libbmp** is intended to be cross-platform on both little-endian (e.g., x86, x86-64) and big-endian (e.g., IBM PowerPC, Sun Sparc) architectures. So far, it has been tested on x86 with Linux (2.6.x kernels and gcc and MinGW/gcc) and Windows (XP Pro with msvc6).

About BMP file format, please see also http://en.wikipedia.org/wiki/BMP_file_format.

# Example #
```
#include <stdio.h>
#include <bmpfile.h>

int
main(int argc, char **argv)
{
  bmpfile_t *bmp;
  int i, j;
  rgb_pixel_t pixel = {128, 64, 0, 0};

  if (argc < 5) {
    printf("Usage: %s filename width height depth.\n", argv[0]);
    return 1;
  }

  if ((bmp = bmp_create(atoi(argv[2]), atoi(argv[3]), atoi(argv[4]))) == NULL) {
    printf("Invalid depth value: %s.\n", argv[4]);
    return 1;
  }

  for (i = 10, j = 10; j < atoi(argv[3]); ++i, ++j) {
    bmp_set_pixel(bmp, i, j, pixel);
    pixel.red++;
    pixel.green++;
    pixel.blue++;
    bmp_set_pixel(bmp, i + 1, j, pixel);
    bmp_set_pixel(bmp, i, j + 1, pixel);
  }

  bmp_save(bmp, argv[1]);
  bmp_destroy(bmp);

  return 0;
}
```