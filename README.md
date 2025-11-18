# financial-trends

# Financial Crime Research Insights

This project is a small end-to-end prototype showing how publicly available research data (OpenAlex) can be turned into something useful for analysing financial-crime topics. It’s meant to show how you can build a reusable analytics pipeline that scales to any area of interest by the end user, not just a one-off “dashboard for X”.

The idea is simple:  

The user sets a financial topic they are interested, it can be ANY text e.g. "casino money laundering" or "terrorism financing". From there, we do the following:

**Convert the initial search into openalex topics -> fetch the relevant openalex topics -> explore trends through a clean Power BI dashboard.**

It’s basically a lightweight version of how analysts might explore typologies, understand research activity internationally, or get a quick sense of “who is working on what” across the academic space.

It demonstrates:

- data engineering and transformation  
- working with relational + unstructured data  
- designing a reusable analytics workflow  
- mapping research data into financial crime-aligned visualisations  
- creating clear, analyst-friendly visualisations  

---

## Purpose

The goal is to help intelligence analysts or policy staff quickly answer:  
**“Who is researching this, how is it changing over time, and how does it relate to key financial-crime themes?”**

To do this, the pipeline:

- selects relevant OpenAlex concepts for a user-supplied search query phrase  
- retrieves associated research via the OpenAlex API  
- classifies papers into AUSTRAC-aligned categories  
  - predicate crimes  
  - financial system sectors  
  - high-risk / monitored jurisdictions  
- structures the data into fact + bridge + lookup tables  
- visualises everything in Power BI  

This creates a flexible, repeatable framework for exploring any new topic.

---

## Why this approach

### Topic selection instead of embeddings
Ideally, you would embed ~280 million OpenAlex documents and run similarity search directly. Realistically, that is not happening on a laptop or free tier cloud account. Using OpenAlex’s concept taxonomy is a pragmatic alternative that still provides meaningful results and keeps the pipeline light and transparent.

### Using Databricks
Databricks made the workflow quicker to iterate and more realistic:

- handles nested JSON much better than pandas  
- provides a clean environment for Python, Spark, and ML tools  
- lets you persist tables easily instead of juggling local files  
- mirrors cloud-based workflows used in government and industry  
- originally explored BigQuery, but Databricks offered more flexibility, particularly with potential integration of the unity catalog with power BI.

### Keyword-based classification
Predicate crime, sector, and jurisdiction tags come from:

- AUSTRAC’s 2024 National ML/TF Risk Assessment  
- AUSTRAC Sector Risk Assessment documents  
- FATF high-risk/monitored jurisdiction lists  

Using keywords keeps the classification explainable, simple to audit, and aligned with AUSTRAC frameworks.

### Reusable structure
Splitting the data into relational tables (fact + bridge + lookups) makes the dashboard more responsive (i.e. the slicing and dicing actually works) and means the workflow can be repeated cleanly for any new topic.

---

## Dashboard Overview

The Power BI dashboard surfaces key analytical questions:

- **How much research exists on this topic, and how is it trending?**  
- **Which countries appear most often in the context of this topic?**  
- **Which institutions are contributing the most?**  
- **Which predicate crimes and sectors appear frequently?**  
- **Which papers are most influential (citations)?**

The layout is intentionally clean:

- KPI cards at the top  
- trend lines in the centre  
- bar charts for country, institution, predicate, and sector  
- a deep-dive table for highly cited works  
- slicers for predicate crime, sector, jurisdiction, institution, country, and year  

The focus is clarity over complexity.

---

## Future improvements

Some enhancements I would explore next:

- refining the Power BI UI (tooltips, spacing, navigation)  
- normalising the data model fully into a star schema  
- replacing keywords with proper NER or embeddings  
- adding automated API refresh and metadata tracking (i.e. the user types in their search query, and that kicks off an automated job to trigger the whole ETL, rather than changing the actual code in an ipynb)
- Productionising things (Dockerising and containerising, AirFlow, or other self-managed cloud tools for data pipelines)
- improving topic selection with a lightweight embedding index  

But the current prototype is intentionally simple and readable.

---
