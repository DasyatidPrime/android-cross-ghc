# GHC cross compiler for Android NDK

This repository contains a build script intended to build a version of
[GHC](https://www.haskell.org/ghc/) (the Glorious Glasgow Haskell
Compilation System) capable of compiling executables compatible with an
existing installation of the [Android
NDK](http://developer.android.com/tools/sdk/ndk/index.html).

## License warning for executables built using toolchains built by this script

The way this script currently works involves compiling static versions
of libiconv, GMP, and ncurses, which GHC-compiled programs may link
against in the target environment.  As of this writing, ncurses is
released under an MIT-style liberal license, but libiconv and GMP are
released under versions of the LGPL.  THIS MAY HAVE COPYRIGHT
IMPLICATIONS FOR YOUR APPLICATION.

If your application is libre software and/or open source, you are
_probably_ fine.  If your application is proprietary, then the most
usual way to comply with the LGPL would be to dynamically link with
these libraries—but this is difficult to support via the Android NDK
and seems to be discouraged.  Alternatively, you may be compelled to
modify your build process to also be able to provide object code that
recipients can freely relink with different versions of libiconv
and/or GMP and install in place of the original code.  This script
does not directly support any of these types of functionality at the
moment.  If you are worried about this, you should inspect the source
code of this script carefully, as well as the licenses of all the
dependencies it uses, and decide for yourself.

## Prerequisites

The current script only supports GNU/Linux (aka most full “Linux”)
host systems.  It assumes the host has:

  - Perl >= 5.14
  - An Android NDK with at least a GCC 4.9 toolchain, accessible
    via $NDK_ROOT (variable name unstable!)
  - An Android SDK accessible via $ANDROID_SDK_HOME (variable name
    unstable!)
  - GNU tar, accessible as ‘`tar`’
  - GNU make, accessible as ‘`make`’
  - Automake, with a directory of support files accessible in
    /usr/share/automake-VERSION
  - For GHC build dependencies:
    + a host (non-cross) GHC
    + Happy and Alex
  - For LLVM build dependencies:
    + Python 2, accessible as ‘`python2`’ (preferred) or ‘`python`’
  - If you want to automatically download source archives:
    + curl

See the “[Setting up a Linux system for building
GHC](https://ghc.haskell.org/trac/ghc/wiki/Building/Preparation/Linux)”
page for more details on what GHC builds need.

The following build dependencies are handled by this script and do not
need to be present on the host, as the development files are installed
into the cross-environment:

  - ncurses
  - libiconv
  - GMP

Run the script with --help for more information on command line options.

## Test configurations

This appears to more or less work on:

  - (2014-12-07) (tested by Drake Wilson)
    Arch Linux i686, NDK r10c, host GHC 7.8.3

Success/failure reports for other systems are welcomed.

## Future

  - Needs Cabal integration.
  - Should support LWP for automatic downloads, not just curl.
  - Should support LLVM-based Android toolchains, not just GCC-based ones.
  - Various things should be less hardcoded.
  - Should have offline-readable command line option documentation.
  - Determine whether the SDK and NDK top directory environment variable
    names are actually the conventional ones; the provenance of these
    ones is uncertain and this seems almost undocumented.

## License and acknowledgments for this script

This script was originally written by Drake Wilson.  It is in the
public domain.  It is not the first or the only build script with its
intent.  It was mostly written using knowledge gained directly from
attempting to manually build a cross-compiling GHC with the NDK, but
some of it may have been affected by memory of results from attempting
to analyze and/or run [the ghc-android script in neurocyte's
repository](https://github.com/neurocyte/ghc-android), which see for
its contributor list.  This script does not share any code with that
script, and the author believes that no copyright implications flow
from this.
