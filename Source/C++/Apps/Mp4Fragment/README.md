Mp4Fragment - VFrag Edition
====
What is "Virtual Fragmentation"?
----
When the --vfrag flag is used, standard fragmentation, *including index creation*, is performed.  The standard output file is created.  But it may be ignored.
Also created are an output.sidx and output.json file.

Output.sidx has the binary sidx atom to be handed to players on-request.  

Output.json has the data necessary for the fragment atoms.  The [traf] atom with the [tfhd] and the [trun] atoms are output in hex, including all sample sizes.
(That runs to the end of the hex.)  Also, the source locations in the input file for the trun-referenced samples.

Using The Outputs
----
When a request comes in for a fragment or sequence, look it up (i.e. store the JSON in a database indexed by contentID-TrackID-Segment Numer.)
The Traf field should be converted back to binary, and written *ending* at the DestinationOffset.  

Grab the data from the *original* file at the specified offsets, preferably in one byte range request covering the entire set of offset-size combinations.
In a loop, memmove them down to form a single contiguous chunk, and serve it.

Note that while you can reuse the moov atom from the source file, it won't necessarily be the same size as Bento would create in the fMP4, due to compatibility flags and other additions.  The ftyp atom, for example, is re-created rather than copied.