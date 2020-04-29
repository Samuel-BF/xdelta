# Introduction #

The command-line syntax is like **gzip**, with the additional option -s SOURCE.

Like gzip, -d means to decompress. The default mode (-e) is to compress.

For output, -c and -f flags behave likewise (use standard output, force overwrite).

Unlike gzip, xdelta3 defaults to stdout (instead of having an automatic extension).

Without -s SOURCE, xdelta3 behaves like gzip for stdin/stdout purposes.

Compress examples:

```
xdelta3 -s SOURCE TARGET > OUT
xdelta3 -s SOURCE TARGET OUT
xdelta3 -s SOURCE < TARGET > OUT
```

Decompress examples:

```
xdelta3 -d -s SOURCE OUT > TARGET
xdelta3 -d -s SOURCE OUT TARGET
xdelta3 -d -s SOURCE < OUT > TARGET
```

There are several special command names, such as `xdelta3 printdelta` and `xdelta3 test`.

```
usage: xdelta3 [command/options] [input [output]]
special command names:
    config      prints xdelta3 configuration
    decode      decompress the input
    encode      compress the input
    test        run the builtin tests
special commands for VCDIFF inputs:
    printdelta  print information about the entire delta
    printhdr    print information about the first window
    printhdrs   print information about all windows
    recode      encode with new application/secondary settings
    merge       merge VCDIFF inputs (see below)
merge patches:

  xdelta3 merge -m 1.vcdiff -m 2.vcdiff 3.vcdiff merged.vcdiff

standard options:
   -0 .. -9     compression level
   -c           use stdout
   -d           decompress
   -e           compress
   -f           force (overwrite, ignore trailing garbage)
   -F           force the external-compression subprocess
   -h           show help
   -q           be quiet
   -v           be verbose (max 2)
   -V           show version
memory options:
   -B bytes     source window size
   -W bytes     input window size
   -P size      compression duplicates window
   -I size      instruction buffer size (0 = unlimited)
compression options:
   -s source    source file to copy from (if any)
   -S [lzma|djw|fgk|none] enable/disable secondary compression
   -N           disable small string-matching compression
   -D           disable external decompression (encode/decode)
   -R           disable external recompression (decode)
   -n           disable checksum (encode/decode)
   -C           soft config (encode, undocumented)
   -A [apphead] disable/provide application header (encode)
   -J           disable output (check/compute only)
   -m           arguments for "merge"
the XDELTA environment variable may contain extra args:
   XDELTA="-s source-x.y.tar.gz" \
   tar --use-compress-program=xdelta3 \
       -cf target-x.z.tar.gz.vcdiff target-x.y
```

The `-A` flag may be used to set application-specific data in the VCDIFF header (you may view with `xdelta3 printhdr`). By default, the application-specific data includes the source and input filenames, as well as descriptors to help with ExternalCompression. You can disable the application header with `-A=`.
