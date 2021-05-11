### Dependencies

#### `biogl`

This script needs the [biogl](https://github.com/glarue/biogl) module to function properly. If you use (or can get) `pip`, you can simply do

```python3 -m pip install biogl```

to add the package to a location reachable by your Python installation.

Otherwise, you can clone the `biogl` repo and source it locally (to run from anywhere, you'll need to add it to your `PYTHONPATH` environmental variable, a process that varies by OS):

```git clone https://github.com/glarue/biogl.git```

#### `palign`

This script also requires [palign](https://github.com/glarue/palign) to run the alignment steps (using DIAMOND or BLAST), which you can clone and then add to your `PATH`:

```git clone https://github.com/glarue/palign.git```

#### Recommended: `networkx`

In use-cases with more than two files, `reciprologs` builds a graph representation of all of the reciprocal best hits (RBH), which allows construction of maximal cliques where every member is an RBH of every other member (e.g. `A-B, A-C, B-C, A-D --> A-B-C, A-D`). In order to do this _efficiently_, the Python package [networkx](https://networkx.org/) is needed (via `pip` or otherwise). If you want less-strict requirements for clustering (e.g. `A-B, A-C, A-D --> A-B-C-D`), see the `--chain` argument.

### Usage info

```
usage: reciprologs [-h] [-p PARALLEL_PROCESSES] [-q PERCENTAGE] [--chain]
                   [--subset subset_1 [subset_2 ...]] [--ignore_same_id]
                   [--ignore_same_prefix <prefix_delimiter>] [-o [OUTPUT]]
                   [-d dir [dir ...]] [-b BLAST_FILE] [--overwrite]
                   [--one_to_one] [--logging]
                   file_1 file_2 ... [file_1 file_2 ... ...]
                   {diamondp,diamondx,blastn,blastp,blastx,tblastn,tblastx}

Find reciprocal best hits between two or more files. Any unrecognized
arguments will be passed along to the chosen alignment program.

positional arguments:
  file_1 file_2 ...     files to use to build reciprolog sets (space
                        separated)
  {diamondp,diamondx,blastn,blastp,blastx,tblastn,tblastx}
                        type of alignment program to run

optional arguments:
  -h, --help            show this help message and exit
  -p PARALLEL_PROCESSES, --parallel_processes PARALLEL_PROCESSES
                        run the alignment step using multiple parallel
                        processes (default: 1)
  -q PERCENTAGE, --query_percentage_threshold PERCENTAGE
                        require a specified fraction of the query length to
                        match in order for a hit to qualify (lowest
                        allowable percentage (default: None)
  --chain               cluster reciprologs without requiring all-by-all
                        pairwise relationships, e.g. A-B, A-C, A-D --> A-B-
                        C-D (default: False)
  --subset subset_1 [subset_2 ...]
                        Files containing subsets of headers to be used as
                        queries for each input file. Supplied in the same
                        order as the input files; one header per line. To
                        omit a subset file for a given input file, provide
                        "." as the argument, e.g. for three input files
                        with only 1 & 3 with subsets: --subsets subset_1 .
                        subset_2 (default: None)
  --ignore_same_id      ignore hits where both query and subject have
                        identical IDs (default: False)
  --ignore_same_prefix <prefix_delimiter>
                        ignore hits where both query and subject have
                        identical prefixes, where the prefix for each ID is
                        delimited by the specified <prefix_delimiter>
                        (default: None)
  -o [OUTPUT], --output [OUTPUT]
                        output filename (if no argument is given, defaults
                        to stdout) (default: stdout)
  -d dir [dir ...], --alignment_source_directory dir [dir ...]
                        check for existing alignment files to use in this
                        directory first (default: None)
  -b BLAST_FILE, --blast_file BLAST_FILE
                        aggregated BLAST output to use (both directions)
                        (default: None)
  --overwrite           overwrite existing output files (instead of using
                        them to bypass alignment step) (default: False)
  --one_to_one          remove any many-to-one reciprolog relationships in
                        each pairwise set, such that each member of each
                        pairwise comparison is only present exactly one
                        time in output (default: False)
  --logging             output a log of best-hit choice criteria (default:
                        False)
```
