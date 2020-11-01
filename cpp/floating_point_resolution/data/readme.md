
# How to split and recombine files

Note: a 750 MB text `*.csv` file might compress down into a 150 MB `*.csv.tar.gz` file. However, GitHub has a hard 100 MB max file size limit. See [here](https://stackoverflow.com/a/59479166/4561887), and my comment under that answer too. So, it is a good idea to split the text file into 400 MB chunks first, then compress it, to keep the compressed size down to <= 100MB. Here's a command to split a file into 400 MB chunks (see: https://linoxide.com/linux-how-to/split-large-text-file-smaller-files-linux/):

    split -b 400MB temp/somefile.csv temp/somefile_split.csv

This will generate files like this:

    temp/somefile_split.csvaa
    temp/somefile_split.csvab
    temp/somefile_split.csvac
    temp/somefile_split.csvad
    # etc.

Now, to rejoin them (see "[What's the best way to join files again after splitting them?](https://unix.stackexchange.com/q/24630/114401)"), do this:

    cat temp/somefile_split.csv* > temp/somefile_recombined.csv

## Example:

Running this:

    split -b 400MB temp/double_resolution_test_3_20201031#3.csv \
    temp/double_resolution_test_3_20201031#3_split.csv

will generate these two files:

    temp/double_resolution_test_3_20201031#3_split.csvaa
    temp/double_resolution_test_3_20201031#3_split.csvab

_Now you can compress those files individually into `*.tar.gz`, or similar, compressed files._

To recombine them, run:

    cat temp/double_resolution_test_3_20201031#3_split.csv* > \
    temp/double_resolution_test_3_20201031#3_recombined.csv 


# SHAs

Here are the `sha256sum` results you should expect, so you can be sure you recombined the files correctly:

    $ sha256sum temp/double_resolution_test_3_20201031#3_recombined.csv
    d1e093f83d8d9ce2d47dc1fdd2f576a5c152d41aa749119c3fb7ac6789e680ef  temp/double_resolution_test_3_20201031#3_recombined.csv

