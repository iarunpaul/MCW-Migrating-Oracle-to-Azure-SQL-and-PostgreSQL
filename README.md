Capsular Bullet points for Hands On Lab - MCW-Migrating-Oracle-to-Azure-SQL-and-PostgreSQL
WDS - Migrating Oracle to Azure SQL

Customer Situation

Need a proof of concept (POC) for customer site analysis
to compare cost, performance, and level of effort required to migrate
evaluate the dependent applications and reports and come up with a migration plan
Also take advantage of new SQL Server features to improve performance and resiliency, as well as explore ways to migrate from an old version of SQL Server to the latest and consider the impact of migrating from on-premises to the cloud.

Customer situation
Data Influx - Oracle relational database management system (RDBMS)  data has become increasingly expensive to store, Oracle upgrades are tedious and costly
have requested a proof of concept (POC) for replacing Oracle with Microsoft SQL Server
improve the performance of their transactional databases without incurring expensive new license fees
concerned with keeping their transactional system available and online for their store
Oracle has been slowing down as their growth has doubled
would need to invest in new hardware to achieve this on-premises
several external and internal applications that need to migrate with the database
database is used by an online store application, written in ASP.NET MVC, internal applications that manage their product catalog, written in Oracle Forms
have many reports to aid in forecasting, sales reporting, and inventory maintenance, mixture of SQL Server Reporting Services (SSRS), Excel, and Oracle Forms hit the Oracle OLTP database directly

database to interact with vendors, require real-time access to their sales data through an API so they can draw warranty information on the date of sale

also loads the Oracle database into a Microsoft SQL Server 2008 R2 Standard Edition data warehouse
fails an audit because their data warehouse does not provide encryption of data at rest, extended support period for SQL Server 2008 R2 has passed, which translates into more security, compliance, and audit implications
needs to upgrade to Azure SQL Database or SQL Server Enterprise Edition and would like to include an upgrade in the POC
 New requirement
have an existing web service that interacts with a vendor to get the latest certifications of that vendor's products, JSON parser sometimes fails, and they can't figure out why
'd like to store the original, unparsed JSON in a table for troubleshooting purposes, would like to be able to query the JSON data by date or other identifying pieces of the JSON
interested in learning more about the best way to do that

how to monitor that situation and possible remedies, like one of their audit tables ran out of space, and scrambled to make space on an already overloaded Storage Area Network (SAN)
would also like high availability to be built into the project plan and what additional fees that would incur

Needs and concerns summary 
looking to decrease their software license fees, take advantage of a modern data warehouse, and provide a strong vision of availability for the future
DBA's concerns
hard to find tools that make it possible, SQL Server is not as high performing as Oracle, does not have great high availability like Oracle Real Application Clusters (RAC), and doesn't have a replacement for Oracle Forms
concerned about the upgrade path because of all the dependencies on the data warehouse

Customer Needs  - bullets

Wants to migrate an existing Oracle database to SQL Server 2017 on-premises, SQL Server 2017 in an Azure VM, or Azure SQL Database.

Needs to know what's involved in migrating the external sales application to SQL Server.

Wants a better understanding of what to do with the internal Oracle Forms application.

Has multiple touchpoints with external vendors and wants to know what needs to change with those web services.

Wants to upgrade their existing data warehouse from SQL Server 2008 R2 Standard Edition to Azure SQL Database or SQL Server 2017 Enterprise Edition to take advantage of some new features:

They want Transparent Data Encryption (TDE), so they pass audits when asked if they encrypt data at rest.
They want compression for some of their large fact tables.
They want to implement SSRS mobile reporting.
They heard about in-memory structures and are wondering if they can benefit from those. They aren't entirely sure how it is different from what they are using now.
They have a lot of SQL Server Integration Services (SSIS) packages that execute through SQL Server Agent jobs. They'd like to know the upgrade path for those.
Need web-based visualizations on sales and forecasting, and a plan on how to upgrade their existing reporting infrastructure.

Have a new requirement on what to do with JSON data.

They experienced an outage last year and are hyper concerned with not repeating that experience. The audit table filled up, and they ran out of disk space. They'd like to know what would have happened if SQL Server experienced the same issue and what your solution would be.

As a follow-up, they'd also like to know how to answer the Oracle DBA's allegation that SQL Server doesn't have an answer for Oracle RAC.


Design proof of concept (POC) solution

with the target customer audience in a 15-minute chalk-talk format

Directions: With all participants at your table, respond to the following questions on a flip chart:

High-level architecture

Without getting into the details (they are addressed in the following sections), diagram your initial vision for handling the top-level requirements for data loading, data preparation, storage, high availability, application migration, and reporting. You will refine this diagram as you proceed.

What should be included in the POC?

How will SQL Server save them on licensing costs?

Schema and data movement

How would you recommend to move their data and schema into SQL Server? What services would you suggest and what are the specific steps they would need to take to prepare the data, to transfer the data, and where would the loaded data land?

Update your diagram with the data loading process with the steps you identified.

Application changes

What product would you recommend to migrate their storefront MVC application to the new SQL Server database?

How would you migrate the Oracle Forms applications? How would you define success? Are there any technologies the customer needs to know?

What will you do about the vendor touchpoints? How will you recommend they store the JSON data?

Data warehouse and reporting

How can they discover which reports and Excel spreadsheets hitting the Oracle database need to be upgraded? What's a proper upgrade path?

What must change about the way company loads its data warehouse?

What components do they need to use to upgrade the SQL Server 2008 R2 data warehouse to Azure SQL Database or SQL Server 2017 Enterprise?

Identify the significant milestones of delivering an upgrade to Azure SQL Database or SQL Server 2017 Enterprise.

Are there any tools or processes that would make this easier? How does Azure Database Migration Service (DMS) compare to other Microsoft database migration tools, such as Database Migration Assistant (DMA) or SQL Server Migration Assistant (SSMA)?

What are the steps required to use the Azure Database Migration Service to perform a database migration?

What are the post-upgrade steps we should consider in the POC? How would this address their concerns?

High Availability and Audit Table

If our solution were SQL Server, what could have done with the audit table when it filled up?

What are the SQL Server options for high availability?

Azure SQL Database POC

Should they move to on-premises first?

Is there any benefit to going straight to Microsoft Azure? Does Azure SQL Database take care of all their requirements?

Are there any questions we need to answer before we can begin a POC directly to Microsoft Azure?

Present the solution

Present a solution to the target customer audience in a 15-minute chalk-talk format
### Let us know how we’re doing!  
Please take a moment to fill out the [Microsoft Cloud Workshop Survey](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyEtIpX7sDdChuWsXhzKJXJUNjFBVkROWDhSSVdYT0dSRkY4UVFCVzZBVy4u) and help us improve our offerings.

# Migrating Oracle to Azure SQL and PostgreSQL

Wide World Importers (WWI) has experienced significant growth in the last few years. In addition to predictable growth, they’ve had a substantial amount of growth in the data they store in their data warehouse. Their data warehouse is starting to show its age, slowing down during extract, transform, and load (ETL) operations and during critical queries. The data warehouse is running on SQL Server 2008 R2 Standard Edition.

The WWI CIO has recently read about new performance enhancements of Azure SQL Database and SQL Server 2017. She is excited about the potential performance improvements related to clustered ColumnStore indexes. She is also hoping that table compression can improve performance and backup times.

WWI is concerned about upgrading their database to Azure SQL Database or SQL Server 2017. The data warehouse has been successful for a long time. As it has grown, it has filled with data, stored procedures, views, and security. WWI wants assurance that if it moves its data store, it won’t run into any incompatibilities with the storage engine of Azure SQL Database or SQL Server 2017.

WWI’s CIO would like a POC of a data warehouse move and proof that the new technology can help ETL and query performance.

November 2020

## Target audience

- Application developer
- SQL Developer
- Database Administrator

## Abstracts

### Workshop

In this workshop, you gain a better understanding of how to conduct a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server or PostgreSQL. There are two migration paths included in this workshop and the training files are named appropriately for each path. Both workshops share a common customer Oracle migration scenario. Each workshop path has tailored database platform migration steps to assist you in this learning journey.  You will evaluate the dependent applications and reports that need to be updated and come up with a migration plan. Also, you will design and build a proof of concept (POC) to help the customer take advantage of new SQL Server or PostgreSQL features to improve performance and resiliency.

For those students focusing on the SQL Server migration, you will explore ways to migrate from an old version of SQL Server to the latest version and consider the impact of migrating from on-premises to the cloud.

At the end of this workshop, you will be better able to conduct a site analysis for compare cost, performance, and level of effort required to migrate from Oracle to SQL Server or PostgreSQL.  Given the time required to complete the workshop, it is recommended the student and trainer pick a single migration path.

### Whiteboard design session

In this whiteboard design session, you work with a group to design a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and level of effort required to migrate from Oracle to SQL Server or PostgreSQL. You evaluate the dependent applications and reports that need to be updated and come up with a migration plan. Also, you review ways to help the customer take advantage of the database features to improve performance and resiliency. For the SQL Server path, you explore ways to migrate from an old version of SQL Server to the latest version and consider the impact of migrating from on-premises to the cloud.

At the end of this whiteboard design session, you will be better able to design a database migration plan and execute the steps.

### Hands-on lab

In this hands-on lab, you implement a proof of concept (POC) for conducting a site analysis for a customer to compare cost, performance, and migration level of effort. You will evaluate the dependent applications and reports that need to be updated and come up with a migration plan. Also, you help the customer take advantage of new features to improve performance and resiliency and perform a migration+.

At the end of this hands-on lab, you will be better able to design and build a database migration plan and implement any required application changes associated with changing database technologies.

## Azure services and related products

- Azure App Services
- Azure Database Migration Service (DMS)
- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server on Azure Virtual Machines (SQL Server 2008 R2 and SQL Server 2017)
- Data Migration Assistant (DMA)
- SQL Server Management Studio (SSMS)
- SQL Server Migration Assistant (SSMA)
- Visual Studio 2019
- Azure Database for PostgreSQL
- ora2pg
- pgAdmin

## Azure solution

Data Modernization to Azure

## Related references

[MCW](https://github.com/Microsoft/MCW)

## Help & Support

We welcome feedback and comments from Microsoft SMEs & learning partners who deliver MCWs.

**_Having trouble?_**

- First, verify you have followed all written lab instructions (including the Before the Hands-on lab document).
- Next, submit an issue with a detailed description of the problem.
- Do not submit pull requests. Our content authors will make all changes and submit pull requests for approval.

If you are planning to present a workshop, _review and test the materials early_! We recommend at least two weeks prior.

**Please allow 5 - 10 business days for review and resolution of issues.**
