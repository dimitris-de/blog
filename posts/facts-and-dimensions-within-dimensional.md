# Facts and Dimensions within Dimensional Modeling

![Data Modeling](https://img.shields.io/badge/Data%20Modeling-Guide-blue)
![License](https://img.shields.io/badge/license-MIT-green)

## Table of Contents

- [Introduction](#introduction)
- [Understanding Dimensional Modeling](#understanding-dimensional-modeling)
- [What are Facts?](#what-are-facts)
- [What are Dimensions?](#what-are-dimensions)
- [Types of Fact Tables](#types-of-fact-tables)
  - [Transactional Facts](#transactional-facts)
  - [Snapshot Facts](#snapshot-facts)
  - [Accumulating Snapshot Facts](#accumulating-snapshot-facts)
- [Types of Dimension Tables](#types-of-dimension-tables)
  - [Conformed Dimensions](#conformed-dimensions)
  - [Junk Dimensions](#junk-dimensions)
  - [Degenerate Dimensions](#degenerate-dimensions)
- [Star Schema vs. Snowflake Schema](#star-schema-vs-snowflake-schema)
- [Best Practices](#best-practices)
- [Common Pitfalls](#common-pitfalls)
- [Conclusion](#conclusion)
- [References](#references)
- [Contact](#contact)

## Introduction

Dimensional modeling is a design technique used in data warehousing to structure data for optimal query performance and ease of understanding. Central to this methodology are **facts** and **dimensions**, which together form the backbone of star and snowflake schemas. This guide delves into the roles of facts and dimensions, their types, and best practices for effective dimensional modeling.

## Understanding Dimensional Modeling

Dimensional modeling organizes data into **fact tables** and **dimension tables**:

- **Fact Tables:** Contain quantitative data for analysis and are typically at the center of a star schema.
- **Dimension Tables:** Contain descriptive attributes related to fact data, providing context for analysis.

This separation facilitates efficient querying and reporting, making it easier for users to extract meaningful insights from large datasets.

## What are Facts?

**Facts** represent measurable, quantitative data about a business process. They are typically numerical values that can be aggregated, such as sales amounts, quantities, or durations. Fact tables store these metrics and are linked to dimension tables to provide context.

### Characteristics of Fact Tables:

- **Granularity:** Defines the level of detail (e.g., individual sales transactions vs. daily sales summaries).
- **Additivity:** Determines how facts can be aggregated (e.g., can be summed across different dimensions).
- **Volatility:** Frequency of updates (e.g., transactional facts are frequently updated).

## What are Dimensions?

**Dimensions** provide descriptive context to facts, allowing users to slice and dice data for analysis. They contain attributes that describe the objects involved in business processes, such as time, location, product, or customer.

### Characteristics of Dimension Tables:

- **Descriptive Attributes:** Non-numeric data that provides context (e.g., product name, customer address).
- **Hierarchies:** Define levels of granularity (e.g., Year > Quarter > Month > Day).
- **Surrogate Keys:** Unique identifiers for dimension records, often used instead of natural keys.

## Types of Fact Tables

### Transactional Facts

Capture individual business transactions or events. Each row corresponds to a single transaction.

**Example:**

| TransactionID | DateKey | ProductKey | CustomerKey | SalesAmount | Quantity |
|---------------|---------|------------|-------------|-------------|----------|
| 1001          | 20230101| 2001       | 3001        | 150.00      | 3        |

### Snapshot Facts

Store data at regular intervals, providing a snapshot of the business process at a specific point in time.

**Example:**

| SnapshotDate | AccountKey | Balance |
|--------------|------------|---------|
| 2023-01-01   | 4001       | 5000.00 |
| 2023-01-02   | 4001       | 5200.00 |

### Accumulating Snapshot Facts

Track the progress of a process that has a defined start and end, capturing milestones along the way.

**Example:**

| OrderKey | OrderDate | ShippingDate | DeliveryDate | OrderStatus |
|----------|-----------|--------------|--------------|-------------|
| 5001     | 2023-01-01| 2023-01-03   | 2023-01-05   | Delivered   |

## Types of Dimension Tables

### Conformed Dimensions

Dimensions that are shared across multiple fact tables, ensuring consistency in reporting and analysis.

**Example:**

A `Date` dimension used by both `Sales` and `Inventory` fact tables.

### Junk Dimensions

Combine miscellaneous low-cardinality flags and indicators into a single dimension to reduce clutter in the fact table.

**Example:**

A dimension containing attributes like `IsActive`, `IsNewCustomer`, `HasDiscount`.

### Degenerate Dimensions

Dimensions that do not have their own table and are stored directly in the fact table, usually identifiers like transaction numbers.

**Example:**

`TransactionID` in a sales fact table.

## Star Schema vs. Snowflake Schema

### Star Schema

- **Structure:** Central fact table connected directly to dimension tables.
- **Advantages:** Simplicity, faster query performance.
- **Use Case:** Suitable for simpler, less normalized databases.

**Example:**

```
Fact_Sales
   |
   |--- Dim_Date
   |--- Dim_Product
   |--- Dim_Customer
```

### Snowflake Schema

- **Structure:** Dimension tables normalized into multiple related tables.
- **Advantages:** Reduced data redundancy, better data integrity.
- **Use Case:** Suitable for complex databases requiring normalization.

**Example:**

```
Fact_Sales
   |
   |--- Dim_Product
         |--- Dim_Product_Category
   |--- Dim_Customer
         |--- Dim_Customer_Address
```

## Best Practices

- **Define Clear Granularity:** Ensure fact tables have a well-defined level of detail to avoid ambiguity.
- **Use Surrogate Keys:** Implement surrogate keys for dimension tables to handle changes over time (slowly changing dimensions).
- **Optimize Indexing:** Create appropriate indexes on fact and dimension tables to enhance query performance.
- **Maintain Consistency:** Use conformed dimensions across different fact tables to ensure consistent reporting.
- **Document Schemas:** Maintain comprehensive documentation of schemas, relationships, and business rules.

## Common Pitfalls

- **Incorrect Granularity:** Misdefining the granularity can lead to inaccurate aggregations and reporting issues.
- **Over-Normalization:** Excessive normalization in snowflake schemas can complicate queries and degrade performance.
- **Ignoring Slowly Changing Dimensions:** Failing to handle changes in dimension attributes over time can result in historical inaccuracies.
- **Lack of Documentation:** Inadequate documentation can hinder maintenance and understanding of the data model.

## Conclusion

Understanding the roles of facts and dimensions is crucial for effective dimensional modeling in data warehousing. By correctly structuring fact and dimension tables and adhering to best practices, organizations can create robust, efficient, and scalable data models that facilitate insightful analysis and informed decision-making.

## References

- [Kimball Group: Dimensional Modeling Techniques](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/dimensional-modeling-techniques/)
- [Snowflake Documentation: Star and Snowflake Schemas](https://docs.snowflake.com/en/user-guide/data-modeling-schemas.html)
- [Microsoft: Data Warehouse Concepts](https://docs.microsoft.com/en-us/sql/analysis-services/data-modeling/warehouse-schema)

## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For any questions or feedback, please reach out:

- **GitHub:** [dimitris-de](https://github.com/dimitris-de)