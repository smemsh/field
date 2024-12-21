field
==============================================================================

::

  field [options] [args]
    select and output fields from each record of stdin
  
  args:
    - 1-arg: field [options] <rangelist>
    - 2-arg: field [non-field opts] <indelim> <rangelist>
    - 3-arg: field [non-field opts] <indelim> <outdelim> <rangelist>
    - if -r/--regex, indelim is a python regular expression
    - field and record delimiters can also be given as options
    - fixed delimiters are python string literals
    - regex delimiters are python raw strings
  
  fields:
    - defaults to whitespace: 'bytes.split(sep=None)'
    - if supplied as $1, uses 'bytes.split(sep="$1")'
    - if '-r', uses 're.split(r'$1')'
    - if '-i <value>', uses 'bytes.split(sep="<value>")'
    - if '-F <value>', uses 're.split(r"<value>")'
    - if $IFS set, splits on its chars: 're.split(f"[{getenv(IFS)}]+")'
    - if '-e', all empty fields are discarded
    - if '-G', only leading or trailing blank fields are discarded
  
  records:
    - defaults to newline: 'bytes.split(sep="\n")'
    - if '-I <value>', uses 'bytes.split(sep="<value>")'
    - if '-R <value>', uses 're.split(r"<value>")'
    - if '-E', empty records in input are discarded
    - if '-N', trailing separator on last record not emitted
    - processes input as bytes, in chunks of io.DEFAULT_BUFFER_SIZE
    - chunk size overridden with '-b/--bsz' (optional k or m suffix)
  
  outdelims:
    - fields default to space, ie '\x20', or via $2, or via -o/--ofs
    - records default to newline, ie '\n', or via -O/--ors
  
  rangelist:
    - specifier is a comma separated list of ranges
    - ranges are either single numbers or first-last sequences
    - unspecified Y in X-Y will default to highest Y in the input
    - if first field in range greater than than last, print reversed
    - if any specified fields do not exist in the record, skip them
    - field '0' is the same as all fields (range '1-')
  
  range:
        N: just field number N (1 is first)
       -N: just field Nth from the end (-1 is last)
      N-M: fields N through M (M > N)
      N-M: fields M through N, backwards (N > M)
       N-: fields N through the end
      -N-: fields from Nth from the end, through the end
     N--M: fields N through Mth from end
    -N--M: fields Nth from the end through Mth from the end
  
  examples:
    "echo one two three four five | field 2 -> "two"
    "echo one two three four five | field -2" -> "four"
    "echo one two three four five | field 2-4" -> "two three four"
    "echo one two three four five | field 4-2" -> "four three two"
    "echo one two three four five | field 3-" -> "three four five"
    "echo one two three four five | field -2-" -> "four five"
    "echo one two three | field 1,0" -> "one one two three"
    "echo {1..12} | field -8--10" -> "5 4 3"
    "echo {1..12} | field -8--15" -> "5 4 3 2 1"
    "echo {1..12} | field 3--15" -> "3 2 1"
    "echo {1..12} | field 1-3,-1--2,8,8,10-,-2" -> "1 2 3 12 11 8 8 10 11 12 11"
  
  todo:
    - read from files given as additional arguments
    - /R/-/S/: starting with /R/ and ending with /S/
    - specify record selection criteria (pattern)
    - field reformatting, eg wrapping, fit in column or on page
    - args like -g columnate_group
    - different behaviors for range overlaps (set union, addition)
    - way to disinclude fields ("all fields except...")
    - multiple delimiters, maybe a -i and -d possible for every -f
    - multiple delimiters, as in multiple patterns will serve as one
    - implement a "record" in terms of "field?"
    - flag to discard empty records in output (no non-empty fields)

____


Option Summary
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

options::

  -h, --help            show this help message and exit
  -F IFSRE, --ifsre IFSRE
                        input field separator as python regex
  -R IRSRE, --irsre IRSRE
                        input record separator as python regex
  -i IFS, --ifs IFS     input field separator as python string
  -I IRS, --irs IRS     input record separator as python string
  -o OFS, --ofs OFS     output field separator string
  -O ORS, --ors ORS     output record separator string
  -b BSZ, --bsz BSZ     bytes per read, optional m or k suffix
  -0, --null            irs is a '\0' char
  -z, --zero            ors is a '\0' char
  -r, --regex           positional ifs is a python regex
  -G, --noedges         discard initial or trailing empty fields
  -e, --noempty         discard empty fields within a record
  -E, --noblanks        discard blank records with no fields
  -N, --noendrec        skip ors after last record was emitted
  -l, --flushrecs       do individual writes every record

___


| scott@smemsh.net
| https://github.com/smemsh/field
| https://spdx.org/licenses/GPL-2.0
