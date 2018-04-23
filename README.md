# IMD by LSOA

## Creating the cut-down files

Create Local Authority list:

> perl sort.pl -r 1 -s 6 -cols 5,6 DEPVSREP160418.csv > LA.csv

Create ward list:

> perl sort.pl -r 1 -s 4 -cols 3,4 DEPVSREP160418.csv > wards.csv

To speed display we create separate files with LSOAs sorted by IMD15:

Representative

> perl sort.pl -sort 8 -cols 12 DEPVSREP160418.csv > LSOA-repr.csv

Local Authority

> perl sort.pl -sort 8 -cols 5 DEPVSREP160418.csv > LSOA-LA.csv

Ward

> perl sort.pl -sort 8 -cols 3 DEPVSREP160418.csv > LSOA-ward.csv
