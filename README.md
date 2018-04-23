# IMD by LSOA

Open Data Manchester [created a dataset](https://medium.com/@opendatamcr/deprivation-vs-representation-building-a-dataset-eea715a87e25) linking political representation in local authorities to Lower Super Output Areas (LSOAs). Jamie Whyte created [a visualisation of the dataset](http://propolis.io/dataviz/depvspolitic/depvspolitic_d3_2.htm). This is an adapted version of that visualisation to speed up rendering (via HTML rather than SVG) and reduce the bandwidth required to show the data.


## Creating cut-down files

The main data file is 5.6 MB in size. That means a pretty big download. Thankfully, much of the data in the file is unnecessary for the visualisation or duplicated so it can be reduced. First we create a list of Local Authorities with their IDs. The perl script `sort.pl` parses and manipulates simple CSV files. We just want columns 5 and 6. We will sort them alphabetically by Local Authority name and remove the duplicate entries:

> perl sort.pl -r 1 -s 6 -cols 5,6 DEPVSREP160418.csv > LA.csv

Now we create a list of ward names with ward IDs. For that we take columns 3 and 4, sort by column 4, and remove the duplicates:

> perl sort.pl -r 1 -s 4 -cols 3,4 DEPVSREP160418.csv > wards.csv

To speed up loading we create three separate files with LSOAs sorted by Indices of Multiple Deprivation (IMD15). We sort by IMD15 because this is the order in the visualisation. That means line number corresponds to IMD15 order and we can lose that column. We'll create one file which just gives the overall representation for each (ordered) LSOA.

> perl sort.pl -sort 8 -cols 12 DEPVSREP160418.csv > LSOA-repr.csv

Next we'll create a one-column file just containing the Local Authority ID for each (ordered) LSOA:

> perl sort.pl -sort 8 -cols 5 DEPVSREP160418.csv > LSOA-LA.csv

Finally we create a one-column file just containing the ward ID for each (ordered) LSOA:

> perl sort.pl -sort 8 -cols 3 DEPVSREP160418.csv > LSOA-ward.csv

We now have five files with different bits of the data. We've reduced it to ~20% of what it was. For the basic visualisation to show we just need the file `LSOA-repr.csv`. We can asynchronously load in the other files to add information such as the ward/LA names. 
