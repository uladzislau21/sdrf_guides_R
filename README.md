# How to work with SDRF in R

## Load SDRF into R environment

There are several ways to load your SDRF file into R:
- utilizing base function **read.table()**
    ```
    sdrf <- read.table(file = 'path/to/sdrf_file.sdrf.tsv', sep = '\t', header = T,  check.names = F)
    ```
`sep` indicates what column separator is used in a file (SDRF uses tabulation)

`header` indicates whether the first row of the file should be used for column names (T = TRUE)

`check.names` set to F (FALSE) allows R to read column names as they in original file (contain [])
