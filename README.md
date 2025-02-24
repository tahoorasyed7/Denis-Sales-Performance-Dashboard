# Denis Sales Performance  By Region -Dashboard

###Dashboard Link: https://app.powerbi.com/groups/6788b278-8e12-4887-a834-2228de1cc600/reports/c297c8c0-9beb-4ff2-b4b1-90de7ef82548/2f833a310202322e4bce?experience=power-bi&bookmarkGuid=fa2292f2-583e-46c8-bf26-80cd640ce21b
## Problem Statement

Problem Statement: Sales Insights Dashboard with Medallion Architecture in Fabric & Azure
In this project, we aim to develop a Sales Insights Dashboard using Microsoft Fabric, Azure, and Power BI, following the Medallion Architecture (Bronze, Silver, Gold layers). The goal is to integrate data from multiple sources, perform data transformations, and create meaningful business intelligence insights through interactive visualizations.

### Key Challenges and Objectives:
Resilient Data Ingestion:

Load sales data from various sources (MySQL (On-prem), Azure Blob Storage, ADLS) into a structured Bronze layer.
Ensure a robust mechanism where adding or removing files does not disrupt the pipeline.
Data Transformation & Cleansing:

Process raw data in the Silver layer, including removing duplicates, handling null values, and restructuring fields.
Split Country and City from the "Location" field for geospatial analysis.
Clean up SalesRep and SubCategory IDs by removing unnecessary prefixes like "ID - ".
Data Modeling & Optimization:

Create unique keys (GeoKey) in the Sales and Geography tables for proper relationships.
Establish a well-structured data model by connecting fact and dimension tables in Power BI.
DAX Calculations for Business Insights:

Total Revenue: Units × Retail Price
Total Cost: Units × Standard Cost
Gross Profit: Total Revenue - Total Cost
Gross Profit MoM Growth %: Monthly trend analysis
Average Sales per Day: Revenue divided by actual sales dates
Quarter-over-Quarter (QoQ) Growth: Essential for QBR reports
Fabric & Power BI Integration:

Lakehouse (Denis_LH): Store data as files + tables in Fabric.
Warehouse (Denis_WareH): Process and structure data with SQL-based transformations.
Pipelines (Raw2Bronze, onprem2Bronze, AllRaw2Bronze): Automate data movement from Blob, ADLS, MySQL into Fabric.
Sales Insights Dashboard:

Create a one-page Power BI dashboard showcasing key KPIs, trends, and geographical insights.
Ensure Month sorting (Jan-Dec) for time-series analysis.
Provide breakdown analysis by product, region, and sales rep performance.
Business Impact:
By implementing this scalable data pipeline and interactive dashboard, the organization will gain real-time visibility into sales performance, enabling data-driven decisions, optimizing strategies, and improving overall profitability. 🚀

  for creating new measures following DAX expression was written;
  
 1)  Average Sales Per Day = 

  AVERAGEX(
    VALUES(Denis_TB[Date]),
    CALCULATE(SUM(Denis_TB[TotalRevenue]))
)
2) 
% Contribution = DIVIDE([Total Revenue],[Revenue ALL])

3)
PY KPI Sales = 
                 VAR SelectedYear = SELECTEDVALUE(DateMaster[Date])
                 VAR PrevYearSales = CALCULATE([Total Revenue], PREVIOUSYEAR(DateMaster[Date]))
                 VAR formatPYsale = FORMAT(PrevYearSales/1000000, "0.00")
                   RETURN 
                             IF(ISBLANK(PrevYearSales), "PY Sales : No Data" , " PY Sales :" & formatPYsale &"M")
                

4) 
QoQ Growth = 
VAR CurrentQuarterSales = [Total Revenue]
VAR PreviousQuarterSales = 
    CALCULATE(
        [Total Revenue], 
        PREVIOUSQUARTER(DateMaster[Date]))
    
RETURN
    IF(PreviousQuarterSales <> 0, (CurrentQuarterSales - PreviousQuarterSales)/ PreviousQuarterSales, BLANK())


5)
YOY GrossProfit % = DIVIDE([Total Gross Profit]-[Prev Year GrossProfit], [Prev Year GrossProfit])

6) 
MOM% Gross Profit = DIVIDE([Total Gross Profit]-[Prev Month Gross Profit],[Prev Month Gross Profit])


 The report was then published to Power BI Service.
 https://app.powerbi.com/groups/6788b278-8e12-4887-a834-2228de1cc600/reports/c297c8c0-9beb-4ff2-b4b1-90de7ef82548/594f4b407a95ec846884?experience=power-bi&bookmarkGuid=fa2292f2-583e-46c8-bf26-80cd640ce21b

 
# Snapshot of Dashboard (Power BI Service)

![dashboard_snapo](https://user-images.githubusercontent.com/102996550/174096257-11f1aae5-203d-44fc-bfca-25d37faf3237.jpg)

 
 # Report Snapshot (Power BI DESKTOP)

 
![Dashboard_upload](https://user-images.githubusercontent.com/102996550/174074051-4f08287a-0568-4fdf-8ac9-6762e0d8fa94.jpg)

# Insights

A single page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;







