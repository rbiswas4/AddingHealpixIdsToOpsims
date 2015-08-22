# Add healpix IDs to the output of OpSims which is an sqlite3 database

The opsim output has a lot of information indexed on a key called ObsHistID which is a unique identifier for a visit. We use these opsim outputs as an input for our survey SN simulations. The Carroll et al. 2014 and Awan et al. (in prep) work referenced this morning. This work can be largely summarized in a higher resolution version of the Opsim output, and the dithered positions. We can
add this information to Opsim outputs at the cost of increasing the size of the table. This can
be done by splitting records in OpSims and assigning healpix Ids to them.

# Steps

- Figure out the number of healpixels numHealpix = 4 nside *2, round to nearest integral power of 10
- create a new primary key for our database: obsHistId * numHealpix + healpixId
- For each record, which is a visit in a field, loop over healpixels in that field and for each healpixel, write out a new record instead of the original record with the new primary key
    - The main thing we would need is a relation between the ra/dec coverage
    of a field to the healpixels in that field: 
    - We could get a lot of this by `cheating' and using glob to read the
    output of the  works referenced above. We could also use the basic functions
    they used to do this (so that it will work for different field
    tilings/ nside in future: Discussed with Eric Gawaiser, and he decided that we should redo it using
    query_disc which also exists is healpy which is already in the stack.
    - Discussed using MAF, and may use the MAF library.
