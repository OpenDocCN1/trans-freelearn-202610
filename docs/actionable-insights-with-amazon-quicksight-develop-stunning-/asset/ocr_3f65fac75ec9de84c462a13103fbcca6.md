2021-02-26 16:54:00 134481400 yellow_tripdata_2020-12.
csv

We don't need to download all of them. For this tutorial, we will copy only the files
for 2020.

2. We can use the S3 CLI `exclude` and `include` parameters to apply a pattern to
match the desired filenames. The command to copy is as follows:

% aws s3 cp "s3://nyc-tlc/trip data/" s3://data-lake-
xxxxxxxxxxx/yellowtrips/ --recursive --exclude "*" 
--include "yellow_tripdata_2020*"

3. Once completed, we can then verify that the files exist in our environment with the
`aws s3 ls` command, which lists the files under a specific S3 location:

% aws s3 ls s3://data-lake-xxxxxxxxxxx/yellowtrips/
2021-03-27 16:53:41 593610736 yellow_tripdata_2020-01.
csv
2021-03-27 16:53:41 584190585 yellow_tripdata_2020-02.
csv
2021-03-27 16:53:42 278288608 yellow_tripdata_2020-03.
csv
2021-03-27 16:53:41 21662261 yellow_tripdata_2020-04.
csv
2021-03-27 16:53:43 31641590 yellow_tripdata_2020-05.
csv
2021-03-27 16:53:42 50277193 yellow_tripdata_2020-06.
csv
2021-03-27 16:53:44 73326707 yellow_tripdata_2020-07.
csv
2021-03-27 16:53:46 92411545 yellow_tripdata_2020-08.
csv
2021-03-27 16:53:50 123394595 yellow_tripdata_2020-09.
csv
2021-03-27 16:53:54 154917592 yellow_tripdata_2020-10.
csv
2021-03-27 16:53:57 138989555 yellow_tripdata_2020-11.
csv
2021-03-27 16:53:58 134481400 yellow_tripdata_2020-12.
csv