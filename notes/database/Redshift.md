# Redshift

[Back to Index](../../README.md)

## Exam Tips / Summary

- Single node: 160 G
- Multi node: Leader node + up to 128 compute nodes
- MPP: Massively Parallel Processing
- You can scale out by adding more nodes to maintain query performance with growing data
- Backups:
    - Enabled by default with 1 day retention period
    - Maximum retention period is 35 days
    - Always attempts to maintain at least 3 copies of your data
        - S3
        - Original
        - Backup/replica on compute nodes
    - Can asynchronously replicate snapshots to S3 in another region
- Pricing
    - Compute node hours (1 unit per compute node per hour)
    - Backups
    - Data transfer
- Encryption
    - SSL
    - AES-256 at rest
    - By default takes care of key management
        - You can manage your own through HSM
        - AWS KMS
- Availability
    - Single AZ
    - You can restore snapshots to new AZ
- Used for BI

## What is it?

Redshift is a fast, managed data warehouse which makes it simple and cost-effective to analyze data using SQL and existing BI tools. It can run complex analytical queries against petabytes of structured data, using query optimization, columnar storage, high-performance local disks, and massively parallel query execution.

Redshift clusters consist of a leader node, compute nodes (where the data is stored), and slices within each compute node. Storage consists of 1MB blocks for massive parallelization of operations, where most DBs use blocks of 2-32KB. They use zone maps (min-max values for each block) to figure out which blocks to skip.

#### Good for:
- Online analytical processing (OLAP)
    - Typically batched write operations, and heavy reads/aggregations of the data.

#### Bad for:
- Online transaction processing (OLTP)
    - Continuous write operations, and high volumes of small read operations.
- Small datasets (< 100GB)
- BLOB data (use S3)

## Table Design

### Distribution Styles

These are handled at the cluster level.

#### Even

The leader node distributes data in a round-robin style across nodes and slices.

When:
- No JOIN statements
- Key or All are not a clear choice

#### Key

Rows are distributed according to the distribution key. The leader node places rows with matching distribution key values on the same node and slice.

When:
- Column is used in a JOIN
- Large fact table in a star schema
- One distribution key per table
- Distribution key has high cardinality

`Star schema:` Consists of a fact table and dimension tables.

`Fact table:` A table that contains measurements, metrics, and facts of a business process, such as sales price, quantity, and other measurable figures.

`Dimension table:` A table that stores dimensions describing objects in a $FACT_TABLE - models, colors, sizes, etc..

#### All

A copy of the entire table is distributed to every node. This ensures that every row is collocated with other data for every join that the table participates in.

When:
- Data that doesn't frequently change
- Reasonably sized data (a few million rows or smaller)

### Sort Keys

Redshift's query optimizer uses sort keys when it determines optimal query plans.

#### Types:
- Compound
    - Works best when using filters and conditions involving the following operators:
        - JOIN
        - GROUP BY
        - ORDER BY
        - PARTITION BY window function
        - ORDER BY window function

- Interleaved
    - Gives equal weight to each column
    - Works best for multiple queries w/ different filters

#### Benefits of ordering data:
- Reducing disk I/O by improving zone map effectiveness
- Reducing compute overhead and I/O by avoiding or reducing cost of sort steps
- Improve join performance by enabling MERGE JOIN operations

#### When:
- Looking for timeboxed data
    - When querying for recent data, TIMESTAMP datatype works well to skip data via zone maps
- Columns used in BETWEEN conditions or EQUALITY operators
- Sort and dist keys work well together (JOIN columns)
- Interleaved
    - Restrictive predicates
    - Multiple queries using different columns as filters

NOTE: If you need a dist/sort key without high cardinality, normalization/denormalization may be helpful.

### Explain Plans

This is the query optimizer's sequential plan for executing your query.

[Here are some AWS docs that are helpful in evaluating an explain plan.](https://docs.aws.amazon.com/redshift/latest/dg/c_data_redistribution.html)

It can be used to identify problems and complex operations due to unoptimized table design.

#### Good
- No distribution required because all joins are collocated
    - Indicators:
        - `DS_DIST_NONE`
        - `DS_DIST_ALL_NONE`

#### Bad
- Tables are not joined on their distribution keys
    - Indicators:
        - `DS_BCAST_INNER`
        - `DS_DIST_BOTH`
    - If the fact table does not have a `DISTKEY`, specify the joining column as `DISTKEY` for both tables
    - If the fact table has a different `DISTKEY`, you should evaluate whether changing it will improve overall performance. 
    - If changing fact table's `DISTKEY` is not an optimal choice, you can achieve collocation by specifying `DISTSTYLE ALL` for the inner table.

- Entire inner table is redistributed to a single slice because outer table uses `DISTSTYLE ALL`
    - Indicators:
        - `DS_DIST_ALL_INNER`
    - Results in inefficient execution on a single node instead of massive parallelization

#### Bad(ish) - relatively high cost:
- Inner table is redistributed to the nodes during execution
    - `DS_DIST_INNER`


### Compression

#### Automatic

This is the recommended method for compressing Redshift tables. It is achieved via the `COPY` command.

Steps:

1. Sample of data is loaded.
2. Compression algorithms are chosen.
3. Table is loaded w/ sample data (number of rows can be chosen via `COMPROWS`)
4. Samples are removed from table.
5. Empty table.
6. Table is recreated with compression encodings in place.
7. Full user dataset is loaded and compressed.

#### Manual

When:
- Recommended encodings are not a good fit.
- Sort key columns are more compressed than others in the same query.
    - This means more rows/scanned data per block in parallel operations.

### Other Design Considerations

#### Avoid overly large columns/datatypes. This can **seriously** affect query performance.
- Use smallest possible column size.
- Complex queries store intermediate results in temp tables that are **uncompressed**.
- Results in wasted memory.
- Queries can spill over to disk.

#### Primary Key and Foreign Key constraints can be implemented, but not enforced.
- Only declare them if your ETL process or application enforces integrity.
- Can help order large #'s of JOINs
- Can help eliminate redundant JOINs

#### DO NOT use CHAR or VARCHAR datatypes for dates/timestamps
- Per Amazon, these can definitely affect performance
- Use DATE and TIMESTAMP

## Helpful Resources

[Amazon Redshift Engineering's Advanced Table Design Playbook](https://aws.amazon.com/blogs/big-data/amazon-redshift-engineerings-advanced-table-design-playbook-preamble-prerequisites-and-prioritization/)

In-depth explanation of best practices when designing Redshift tables.

[Evaluating an "Explain Plan" (AWS Docs)](https://docs.aws.amazon.com/redshift/latest/dg/c_data_redistribution.html)

[Designing Tables (AWS Docs)](https://docs.aws.amazon.com/redshift/latest/dg/t_Creating_tables.html)

[Amazon Redshift Utils](https://github.com/awslabs/amazon-redshift-utils)

Helpful scripts and queries for use with redshift.