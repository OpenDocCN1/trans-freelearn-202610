Some of the factors we use when making a compression decision are:

+   - The speed of compressing and uncompressing
+   - The compression ratio (how much smaller did the file get?)
+   - The interoperability of the compression solution (can my audience easily decompress this file?)

# Rules of thumb

Gzip is faster to compress and decompress, and it is readily available on every OSX and Linux machine. However, some Windows users will not have a gunzip program readily available.

Bzip2 makes smaller files than gzip and zip, but it takes longer to do so. It is widespread on OSX and Linux. Windows users will probably struggle to handle bzip2 files if they have not already installed special software.

Zip is readily available on Linux, OSX, and Windows, and its speed to compress and decompress is not terrible. However, it does not create very favorable compression ratios (other compressors make smaller files). Still, its ubiquity and relative speed (compared to bzip2, for example) are strong points in its favor.

RAR is a widely available archiving and compression solution for Windows; however, its availability for OSX and Linux requires special software, and its compression speed is not as good as some of the other solutions.

Ultimately, you will have to decide on a compression standard based on the particular project you are working on and the needs of your audience or user, whether that is yourself, a customer, or a client.

# Data types, nulls, and encodings

This section provides an overview of the most common data types that data scientists must deal with on a regular basis and some of the variations between these types. We also talk about converting between data types and how to safely convert without losing information (or at least understanding the risks beforehand).

This section also covers the mysterious world of empties, nulls, and blanks. We explore the various types of missing data and describe how missing data can negatively affect results of data analysis. We will compare choices and trade-offs for handling the missing data and some of the pros and cons of each method.