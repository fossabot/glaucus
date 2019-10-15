# mpfr

## Cross

### Configure
no need for --build and --host (set to gnu) as the configure script is smart
enough to detect them

LTO couldn't be enabled on the shared version of mpfr, as it would result
in the following error when running configure:
error: --enable-lto can only work in static mode (--disable-shared)

mini-gmp has less memory footprint and is smaller than GMP but is slower (up
to 10 times slower in some cases). You should never install mini-gmp as
applications can either include `mini-gmp.c` or compile it and use the
declarations in `mini-gmp.h`

--with-gnu-ld since we're using GNU binutils ld bfd (and not gold as it 
doesn't build with musl without PIE)

Don't use `--with-gmp` flag, even if it correctly detects libgmp in the first
stage of mpfr as `/home/glaucus/cross/lib/libgmp.so.10` as this isn't 100%
correct, as it should show `/cross/lib/libgmp.so.10` and it will cause build
failures in the second stage of building mpfr such as detecting that the C
compiler doesn't work, or not finding the correct libgmp.so to link to.
Instead stick with what's recommended, and that's using LDFLAGs rpath.

Passing `--with-sysroot` alone (without `--with-gmp`) will cause mpfr to look
for libgmp.so in $CRSS/$TUPL instead of $CRSS.

Still needs to get rid of the long path in `ldd libmpfr.so`

## Native

### Configure
LTO couldn't be enabled on the shared version of mpfr, as it would result
in the following error when running configure:
error: --enable-lto can only work in static mode (--disable-shared)