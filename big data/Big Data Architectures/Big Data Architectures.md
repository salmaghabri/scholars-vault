# Data Warehouses : Centralised Data Management for Informed Decisions
- centralized reservoir for extensive data gathered from **diverse source**
- serves as the bedrock for business analytics and reporting, offering a singular reference for data-driven activities.
-  By focusing on storing historical data, a data warehouse aggregates **relational** data from **various origins**, including application and business data. 
- This information is curated through **transformation** and **cleansing** steps **before** integration, ensuring its **quality** and **unity**.
- <ins>value</ins> : their capacity to expedite the generation of vital business insights across the organisation.
- This **ease of access** to comprehensive data enables well-informed decision-making for various stakeholders. Business analysts, data engineers, and decision-makers can utilise different tools like BI platforms and SQL clients to navigate, query, and interpret data **without extensive data science expertise**.
- strategic benefits: promoting collaboration and breaking down data silos. 
![[Big Data Architectures-1.png]]
#### However:
- often function as black boxes
- Data goes in, and insights come out: underlying processes remain hidden
- might limit users' understanding of the data trans- formation, integration, and query optimisation steps -> gap technicalities of data warehouse and user expectations
- + data can't be ingested unless it has been cleaned and fully transformed which is not always be possible in big data era.
### To harness dw effectively
1. robust infrastructure
2. scalable storage
3.  analyticstools
4. skilled personnel
5. data ethics, security, and privac
# Data Lake : Flexible and Scalable Data Storage
- a centralised repository securely housing vast amounts of structured and unstructured data in its raw form.
-  <ins>Unlike data warehouses </ins> : data lakes employ a flat architecture and object storage, offering flexibility, durability, and cost-effectivenes This enables insights from challenging unstructured data.
-  data lakes capture **data without a predefined schema**: allowing  (ELT) when needed.
- **This adaptability** suits **various data types**, such as IoT and social media data. Hence, data lakes facilitate machine learning and predictive analytics.
![[Big Data Architectures-2.png]]
####  benefits:
-  Data centralization: Centralized storage for all data formats.
-  Data flexibility: Native format storage fosters adaptability.
- Cost savings: Lower-cost storage via object storage.
#### Challenges
- Poor business intelligence performance: Disorganization can hinder analytics. 
- Data reliability and security: Lack of consistency complicates security
- s the lack of governance and metadata management, potentially leading to <ins>the "data swamp" problem </ins> : data becomes disorganised, impeding effective analytics; there are no ACID guarantees. 
- allows for data of varying quality and relevance to be ingested, potentially leading to difficulties in data cleaning and accurate analysis.
-  managing access controls and ensuring data security in a heterogeneous environment can be daunting. 
# Lake House: Merging Data Warehouses and Data Lakes
- combines the strengths of both traditional data warehouses (ACID Guarantees) and modern data lakes (all data types). 
-  establishes a unified repository capable of accommodating various data formats, including structured, semi- structured, and unstructured data. This unification streamlines data storage and integration, eliminating the need for separate storage environments and facilitating seamless data consolidation.
![[Big Data Architectures-3.png]]
#### advantages
- supports a wide spectrum of data processing needs.
- bridges the gap between structured data management, which data warehouses excel at, and the flexibility and cost-effectiveness of data lakes. -> enables organisations to easily **organise and query** their data, while also allowing them to leverage the **storage efficiency and adaptability** of data lakes.
- **Reduced Data Redundancy**:  The unified storage model of data lakehouses minimizes data duplication
-  **Cost Efficiency**:  By tapping into the cost-effective storage capabilities of data lakes, organizations can store massive volumes of data without incurring exorbitant costs.+ the unified storage system eliminates the complexity of **managing multiple storage systems**, thus reducing operational expenses.
-  **Versatile Workloads**: • This model provides direct access to widely used BI tools facilitating advanced analytics. + it employs open data formats and supports popular machine learning libraries, making data analysis accessible to data scientists and machine learning practitioners. 
-  **Enhanced Governance and Security**:  Through schema enforcement and data integrity measures, the data lakehouse architecture ensures a robust foundation for implementing data security.
# Data Design Patterns
## Lambda
+: can do both types of processing
-: speration betweeen speed and batch layers: limits some preocessing types
![[Big Data Architectures-5.png]]

## Kappa
- pas de batch mais dans la couche streaming(et non pas le user) je peux communiquer (bidirectionnelle) avec un data lake
![[Big Data Architectures-6.png]]
## Medaillon: a synthesis of Lambda and Kappa
![[Big Data Architectures-7.png]]
- **Bronze layer**: 
	- the primary entry point for data intake and retention. Information is preserved without alterations or conversions.
	- This layer prioritises quick Change Data Capture, historical archiving of source data for reference (cold storage), data lineage tracing, auditing, and the provision for reprocessing without requiring re-reading of source data . 
- **Silver layer** :
	- In the Silver stage, tables undergo cleansing, filtering, or adjustment to enhance their usability.
	-  the modifications at this point should be minor, no complex aggregations or enhancements. 
	-  brings together data from diverse origins, enabling self-service analytics for rapid reporting, analyses, and machine learning. It serves as a foundation ultimately converging in the Gold Layer. 
	- In the lakehouse data engineering concept, the Silver layer follows the ELT method, concentrating on minimal transformations and data purification during loading to ensure swift data lake integration and delivery. Regarding data modeling, the Silver Layer adopts data structures reminiscent of the 3rd-Normal Form or even employs Data Vault-like models optimized for efficient data writing.
- **Gold layer**
	- In the Gold tier of the lakehouse architecture, information is organized into databases customized for specific projects, ensuring readiness. This stratum is optimized for reporting, employing streamlined and efficient data models to reduce the need for extensive connections. It serves as the stage for final data adjustments and quality enhancements. 