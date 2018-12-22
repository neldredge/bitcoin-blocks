# bitcoin-blocks

Script to parse Bitcoin block header data from `blk*.dat` files

## Installing

You need a perl interpreter, and the packages `Digest::SHA` and
`Getopt::Std`.  Once these are installed you can simply run `perl
bitcoin-blocks ...` or `./bitcoin-blocks ...` if you set it
executable.

## Usage

    ./bitcoin-blocks [-i | -d | -t] [-s] block-files...

Process the given `block-files` (e.g. `blk*.dat`), which should be in
the format used by Bitcoin Core, and produce output according to one
of the following modes:

- Index (`-i`): Output (to stdout) a comma-separated text file with
  one line per block, containing the information from its header.
  Suitable for further processing with a spreadsheet program or your
  own scripts.  The first line can be used as column headings; filter
  the output through `tail +2` if you don't want it.

- Dump (`-d`): Output (to stdout) a new `.dat` file containing all the
  processed blocks in increasing order by height.  Should be suitable
  for use as a `bootstrap.dat` file to be imported into another
  Bitcoin Core instance.

- Test (`-t`): Only makes sense if given a single input file.  Test if
  all the blocks are in increasing order by height.  Mostly useful to
  run on the output of `-d` to check that it did the right thing.
  Exit code is 0 if they are in order, or 2 if not.

By default only the main chain (i.e. the longest chain, by number of
blocks) is processed, and all blocks not in that chain are omitted.
Use `-s` to include them as well.  For `-i` and `-d`, the "side"
blocks will come after the main chain, in random order.  A future
version may organize them in a more useful way.

## Notes

The main chain is identified as the chain having the largest number of
blocks, whereas according to the true Bitcoin protocol, it should be
identified as the chain with the most proof-of-work.  Normally they
should be the same, unless someone is doing something weird.
