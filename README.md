# Parsec---patch


This script will patch the errors that arise when tring to build parsec-3.0 benchmarks with perl 5 version >14xx. 

You may see the following errors while building parsec directories:

    installing man1/smime.1
    smime.pod around line 272: Expected text after =item, not a number
    smime.pod around line 276: Expected text after =item, not a number
    smime.pod around line 280: Expected text after =item, not a number
    smime.pod around line 285: Expected text after =item, not a number
    smime.pod around line 289: Expected text after =item, not a number
    POD document had syntax errors at /usr/bin/pod2man line 71.
    make: *** [install_docs] Error 255

The fix is this patch.

In case you do not want to downgrade your perl 5 version on your "updated" machines then just pull this script in the root directory of Parsec benchmark and do the following:

1. chmod 700 parsec_fix.sh 
2. ./parsec_fix.sh


Incase you get this error:
 
    /usr/include/wchar.h:94:3: error: conflicting types for ‘__mbstate_t’
     } __mbstate_t;
       ^
    In file included from ../include/machine/bsd_endian.h:37:0,
                     from ../include/sys/bsd_types.h:44,
                     from ../include/sys/bsd_param.h:64,
                     from if_host.c:48:
    ../include/sys/bsd__types.h:105:3: note: previous declaration of ‘__mbstate_t’ was here
     } __mbstate_t;
     
     
Then do the following step:

1. open [parsec_root_dir]/pkgs/libs/uptcpip/src/include/sys/bsd__types.h in your preferred editor. Note: This file is named bsd'_ _'types.h. There is a file called bsd_types.h too. Do not alter that file.

2. Comment out the following lines of code:

  /*
   * mbstate_t is an opaque object to keep conversion state during multibyte
   * stream conversions.
  */
  /* 
  #ifndef __mbstate_t_defined
   define __mbstate_t_defined    1
  typedef union {
    char      __mbstate8[128];
     __int64_t _mbstateL;  [> for alignment <]
  } __mbstate_t;
  #endif
  */
  
  You should be good to go...
