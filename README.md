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
- getting MS raw file names from the folder,
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

### Creating a pipeline
- `%>%` allows to link several functions together:
  ```
  # add a column comment[fractionation method] and rename column characteristics[pathology] to characteristics[disease]
  sdrd <- sdrf %>%
      mutate(`comment[fractionation method]` = 'high pH fractionation') %>%
      rename(`characteristics[disease]` = `characteristics[pathology]`)
  ```
### Creation of number sequences
- The following code creates a vector for column `comment[fraction identifier]` (raw files' names should be in the same order):
  ```
  fractions <- rep(c(1, 2, 3), each = 10) # first 10 files are from the first fraction and so on
  # output [1] 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2 2 2 3 3 3 3 3 3 3 3 3 3

  # update or create a column with fractions identifiers
  sdrf <- sdrf %>%
      mutate(`comment[fraction identifier]` = fractions)
  ```
  
  This command is also useful for `comment[technical replicate]` column.

### Getting MS raw file names from the folder
- The following command will take all file names in the path `path/to/ms/files` with extension `.raw` and put them in a vector:
  ```
  ms_files <- list.files(list.files(path = 'path/to/ms/files/', pattern = '.raw')

  # fill comment[data file] column
  sdrf <- sdrf %>%
      mutate(comment[data file] = ms_files)
  ```

  Make sure to locate only necessary files in the path. Extension of the files can be changed with `pattern` parameter.

## Links
- [tidyverse package](https://www.tidyverse.org/)
