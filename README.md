# WIP tools/mkhybrid to build HFS/ISO9660 hybrid image for mac68k and macppc

## How to use

    $ cvs -d anoncvs@anoncvs.netbsd.org:/cvsroot checkout -rnetbsd-8-0-RELEASE src
    $ cd src/external/gpl2
    $ git clone https://github.com/tsutsui/netbsd-tools-mkhybrid mkhybrid
    $ cd ../..
    $ patch -p0 < external/gpl2/mkhybrid/patch/mkhybrid.diff
    $ sh build.sh -U -m macppc -j 4 build
    $ cd distrib/cdrom
    $ ${TOOLDIR}/bin/nbmake-macppc RELEASE=8.0 TARGET_CD_IMAGE=macppccd fetch
    $ ${TOOLDIR}/bin/nbmake-macppc RELEASE=8.0 TARGET_CD_IMAGE=macppccd all

(See src/distrib/cdrom/README how to fetch set binaries and build iso images)

## What's this?

WIP sources that provides tools'fied mkhybrid to build
HFS/ISO9660 hybrid CD images for mac68k and macppc install media,
based on OpenBSD's mkhybrid 1.12b5.1.
http://cvsweb.openbsd.org/cgi-bin/cvsweb/src/gnu/usr.sbin/mkhybrid/src/

Currently based on netbsd-8-0-RELEASE tree.

## Changes from OpenBSD's one

- pull sources in OpenBSD's src/gnu/usr.sbin/mkhybrid/src into
  NetBSD's src/external/gpl2/mkhybrid/dist
- pull Makefile in OpenbsD's src/gnu/usr.sbin/mkhybrid/mkhybrid
  into NetBSD's src/external/gpl2/mkhybrid/bin
- src/external/gpl2/mkhybrid/bin is prepared to build tools version
  in src/tools/mkhybrid using src/tools/Makefile.host
- currently configure script is not used
  (only build on NetBSD host is tested)
- tweak configure.in to pull some header files on NetBSD
- pull -hide-rr-moved option from mkisofs-1.13
- pull -graft-points option from mkisofs-1.13 and cdrtools-2.01
- pull malloc related fixes in tree.c from cdrtools-2.01

See commit logs and diffs for more details.

## Known issue

- [fixed] there is something wrong and get SIGSEGV during writing an output image
  so that there is one kludge (comment out free(3) that causes SIGSEGV)
