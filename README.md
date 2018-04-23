# IMD by LSOA

Open Data Manchester [created a dataset](https://medium.com/@opendatamcr/deprivation-vs-representation-building-a-dataset-eea715a87e25) linking political representation in local authorities to Lower Super Output Areas (LSOAs). Jamie Whyte created [a visualisation of the dataset](http://propolis.io/dataviz/depvspolitic/depvspolitic_d3_2.htm). This is an adapted version of that visualisation to speed up rendering (via HTML rather than SVG) and reduce the bandwidth required to show the data.


## Creating cut-down files

The main data file is 5.6 MB in size. That means a pretty big download. Thankfully, much of the data in the file is unnecessary for the visualisation or duplicated so it can be reduced. The perl script `sort.pl` parses and manipulates simple CSV files and we use it to chop up the original file. 

For the bare minimum visualisation to show we only really need the overall representation for each LSOA ordered by IMD15. So we sort the CSV file by column 8 (IMD15) and output column 12 (overall representation) and save this in `LSOA-repr.csv`:

> perl sort.pl -sort 8 -cols 12 DEPVSREP160418.csv > LSOA-repr.csv

We can now render the visual but we want to be able to find out which ward and local authority an LSOA belongs to. So we create two more single-column files containing the ward ID and the local authority ID:

> perl sort.pl -sort 8 -cols 5 DEPVSREP160418.csv > LSOA-LA.csv
> perl sort.pl -sort 8 -cols 3 DEPVSREP160418.csv > LSOA-ward.csv

These are in the same order as the representation file so once we load them we combine them back. Now we have IDs but they aren't exactly human-friendly. We want lookup tables to go from LA/ward ID to names. For local authorities we just want columns 5 and 6, sorted alphabetically by name and with the duplicate entries removed:

> perl sort.pl -r 1 -s 6 -cols 5,6 DEPVSREP160418.csv > LA.csv

Finally we create a list of ward names with ward IDs. For that we take columns 3 and 4, sort by column 4, and remove the duplicates:

> perl sort.pl -r 1 -s 4 -cols 3,4 DEPVSREP160418.csv > wards.csv

These final five files each have different bits of the data. We've reduced the total to ~20% of what it was and can get the basic visual going with just `LSOA-repr.csv` (74kB). We can asynchronously load in the other files to add information such as the ward/LA names. 
