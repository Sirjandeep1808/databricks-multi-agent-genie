# Databricks Multi-Agent System – Genie Case Study

## Overview
This case study implements a **multi-agent analytical system** inside Databricks, inspired by the Databricks Genie architecture.  
The goal is to design a coordinated set of agents that query and combine data from two different functional areas — *Sales Analytics* and *Customer Insights* — to generate useful business insights.

Since Genie’s REST API and SDK access are restricted in standard workspaces, the interaction with Genie is simulated using Python classes and Spark SQL queries.  
Each agent acts as an independent logical unit, much like a Genie workspace would in production.

---

## Objective
The case study required building a system with three agents:

1. **Sales Agent** – Queries the *Sales Analytics* space and handles all sales-related analysis such as total revenue, top-selling categories, and sales by region.  
2. **Customer Agent** – Queries the *Customer Insights* space and focuses on churn risk, customer segments, and lifetime value.  
3. **Coordinator Agent** – Acts as the central orchestrator that calls both agents, merges their outputs, and produces combined insights.

The agents collectively answer the following business questions:

- Which regions have high revenue but high customer churn?  
- How does sales performance compare with customer segments?  
- Which product categories appeal most to premium customers?  
- (Additional) Which regions have the highest revenue contribution from premium customers?

---

## Implementation Summary
All logic was developed within Databricks Community Edition using Python and Spark SQL.  
The system was structured as follows:

- Each agent is defined as a separate Python class.  
- The classes use SQL queries executed through the active Spark session.  
- The `CoordinatorAgent` joins and analyzes the results from the other two agents to generate combined insights.  
- The output is displayed in Databricks tables and can be visualized as charts if required.

Because Genie API access is not available in the Community Edition, the same behavior was reproduced using direct queries.  
If Genie or Databricks SDK endpoints become available later, these class methods can easily be modified to call API endpoints instead of running SQL directly.

---

## Data Design

### 1. `sales_transactions`
| Column | Description |
|---------|-------------|
| sale_date | Date of sale |
| product | Product name |
| category | Product category |
| revenue | Total revenue of sale |
| region | Sales region |

### 2. `customer_behavior`
| Column | Description |
|---------|-------------|
| customer_id | Unique customer identifier |
| segment | Customer segment (Premium, Standard, etc.) |
| lifetime_value | Lifetime value of the customer |
| churn_risk | High, Medium, or Low |
| region | Customer region |

---

## Key Insights Produced
After running the notebook, the following insights are generated:

1. **High Revenue and High Churn Regions**  
   Highlights regions that generate strong sales but have high churn risk among customers.

2. **Sales Performance vs. Customer Segments**  
   Compares revenue performance with customer segments to understand value distribution.

3. **Product Categories for Premium Customers**  
   Identifies which categories are most popular among premium-tier customers.

---

## Technical Stack
- Databricks Community Edition (runtime with Apache Spark)  
- Python 3  
- Spark SQL for querying and joining data  
- Databricks Notebook Environment for modular cell execution  

---

## Architecture Summary
| Agent | Input Source | Role | Output |
|--------|---------------|------|---------|
| SalesAgent | sales_transactions | Performs sales aggregation and category analysis | Regional and category revenue |
| CustomerAgent | customer_behavior | Analyzes churn and customer segments | Churn summaries and lifetime metrics |
| CoordinatorAgent | Combines both | Generates cross-functional insights | Integrated analytics reports |

---

## Challenges and Solutions
| Challenge | Explanation | Resolution |
|------------|--------------|-------------|
| Genie API returned 401/404 errors | REST endpoints are restricted in community workspaces | Replaced API calls with direct Spark SQL queries |
| Attribute mismatches (`table` vs. `table_name`) | Class variable naming inconsistencies | Unified attribute definitions in all agent classes |
| Variables not defined after re-running | Cluster resets cleared memory | Recreated agent instances automatically in setup cells |

---

## Outcome
The project successfully demonstrates how a multi-agent architecture can be implemented inside Databricks.  
Even without direct Genie access, the solution simulates the same analytical flow — separate workspaces performing domain-specific tasks and a coordinator agent synthesizing insights.

The design is modular, extensible, and ready for real API integration if provided in the future.

---

## How to Run
1. Import the notebook into Databricks.  
2. Ensure both tables `sales_transactions` and `customer_behavior` are created.  
3. Run all cells from top to bottom.  
4. The three agents will initialize and produce analytical outputs for each case study question.

