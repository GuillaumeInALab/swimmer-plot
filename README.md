# Swimmer Plot

Script to easily generate Swimmer Plots from clinical tables

The `swimmer_plot()` function creates the base of the swimmer plot.

The required arguments are a dataframe, an id column name, and the column name of where the bars end (i.e. overall survival)
By default the bars are in increasing order, but any order can be specified.
A column name for the fill, transparency and colour can also be included.
Individual bars can change colour/transparency over time.
Other aesthetics can be manipulated using geom_bar() arguments (eg. fill,width, alpha).

## Basic usage of the script

First, it is handy to load the data which is done by:

```{r}
# Load Data
clinicalData <- read.csv(file.choose(), header = T)
```

This chunk helps to refine the dataset with clear annotations.

```{r}
clinicalData <- within(clinicalData, {
 cr = factor(cr, levels = c(1,0), labels = c("Yes", "No"))
 allograft = factor(allograft, levels = c(1, 0), labels = c("Yes", "No"))
 relapse = factor(relapse, levels = c(1, 0), labels = c("Yes", "No"))
 died = factor(died, levels = c(1, 0), labels = c("Yes", "No"))
 alive = factor(died, levels = c("Yes", "No"), labels = c(NA, 1))
 efsevent = factor(efsevent, levels = c(1, 0), labels = c("Yes", "No"))
})
```

A basic swimmer plot is then created using this code:

```{r basic swimmer plot}
basic_plot <- swimmer_plot(df=clinicalData,
                           id='protocol_number',
                           end='os',
                           name_fill = "pheno_summary",
                           #stratify = "",
                           id_order='pheno_summary',
                           width=.85)+
 scale_fill_manual(values = med.brewer("Wellspring"))

basic_plot

ggsave(plot = basic_plot, filename = "figures/Basic Swimmer Plot.pdf", width = 7, height = 5)
```

- `id` refers to the variable used to name the samples
- `end`
