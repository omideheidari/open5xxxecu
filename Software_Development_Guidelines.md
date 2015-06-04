# Introduction #

The basic guidelines that should be followed


# Details #
  * MIT license, **NO** GPL licensed code can be accepted
  * Keep generic code separate from hardware dependent code

  * Use a .h file for any global variables you create
  * Maintain the directory structure (separate directories for different things)
  * Use doxygen type comments
  * Use #defines or run-time switches for things that could be different
  * Don't hardcode constants deep in the code
  * Only SI units - degrees C, liters, etc.
  * Use "bin point" scaling and terminology
  * Don't do much to optimize for speed unless first proven necessary
  * We use git
  * Don't merge code into Master without a code review
  * Don't merge code into Stable without a concensus that everything has been tested
  * Don't use "int", "short", "long", etc. Only uint16\_t, etc.
  * Log errors with "log\_error(char ""string)
  * Both the [o5e\_Reference\_Manual](http://code.google.com/p/open5xxxecu/wiki/o5e_Reference_Manual) and [Open5xxxECU\_User\_Manual](http://code.google.com/p/open5xxxecu/wiki/Open5xxxECU_User_Manual) need to be updated when you add code.