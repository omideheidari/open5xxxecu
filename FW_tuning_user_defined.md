# Introduction #

The issue is that, IMO,  there isn't enough ram to hold all the tables
we might want and leave lots of ram for all the other things (like
serial and CAN buffers and other things to come).  It's wasteful of a
scarce resource to keep all the tables in ram all the time.  That led me
to only putting the tables that are being edited in ram on a "as needed
basis".    Ie, ram is a dynamically allocated cache that holds the
"working set" (recently being edited).



# Implementation Details #

## Files (.c/.h) ##

??
??

## Method ##

The table of tables just keeps track of all the tables and where they
are located (a directory) - it's needed for the tuner anyways.
Magic cookie is just a random number that indicates that what you think
is stored there is actually there.  Google for more info.
Replace "all tables are now in new flash" with "erase old flash block".

This is the algorithm I'm planning for writing tables (and config info)
> to flash.  The idea is that the system ping pongs between two blocks of
> flash and that tables can be in ram (if they are being updated) or in
> flash.  Any comments?


> save\_to\_flash()
> > select unused new flash block
> > verify that new flash block is erased
> > for each table
> > > find it (in old flash or ram)
> > > copy it to new flash
> > > update new table of tables

> > next
> > write new table of tables to new flash
> > write magic cookie to beginning of new flash
> > release ram used (free())
> > update working copy of TofT
> > erase old flash block