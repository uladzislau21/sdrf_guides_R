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


- utilizing function **read_tsv()** from `tidyverse` package
    
    This method is preferred since this function reads SDRF file correctly with just providing a file path. Moreover, `tidyverse` package will be used in the following file processing.
    ```
    install.packages('tidyverse')
    library(tidyverse)
    sdrf <- read_tsv(file = 'path/to/sdrf_file.sdrf.tsv')
    ```

## Process file

General table manipulation operations include:
- column(s) addition/removal,
- renaming of column(s),
- changing value in particular cell.

Advanced programming includes:
- creation of number sequences,
- strings' manipulation.

### Add columns/rows
- To add **columns** to the SDRF we can use `mutate` function:
    ```
    sdrf <- sdrf %>%
      mutate(`comment[number of missed cleavages]` = 2)
    ```
    `%>%` allows to pass the result of previous function execution to the next one (take dataset sdrf and add column with the name comment[number of missed cleavages])

    `mutate` function creates or updates columns, indicate the name of the column (enclose with `` if it contains spaces) and data (data can be a single value that will be recycled to length of n, or vector, where n is the number of rows)

  More columns can be indicated through comma.

- The following command will remove the column from the dataset:
```
sdrf <- sdrf %>%
  mutate(`comment[number of missed cleavages]` = NULL)
```
### Renaming of column(s)
- Columns can be renamed with `rename` function:
  ```
  sdrf <- sdrf %>%
      rename(`comment[missed cleavages]` = `comment[number of missed cleavages]`)
  ```
  `rename` takes arguments in the following format: new_name = old_name

  More columns can be listed through comma.

### Changing value in particular cell
- The following command will change a value in 3rd row of `comment[depletion]` column to `non-depleted`:
    ```
    sdrf[3, 'comment[depletion]'] <- 'non-depleted'
    ```
    While subsetting a dataframe first the number of row is indicated, then the name of the column (it can be indicated with number also, but not advised since the order of columns can be changed later)

## Links
- [tidyverse package](https://www.tidyverse.org/)
