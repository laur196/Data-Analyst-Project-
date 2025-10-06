# Media advertising performance optimization (November 2023- April 2024)
## Problem Statement
Clay Art communication is a media advertising company in Nigeria that focuses on creating campaigns that add value to people's lives and resonate with consumer. The marketing operation came up with media ads optimization project to address inefficient performance and increased cost across platforms through a media campaign. The project is geared towards investigating the right platform that will generate long-term profitability and competitive positioning in the digital marketplace. 
The project focuses on important key performance indicators (KPIs) such as Impression, Clicks, Conversion, Revenue, Cost and profit across platforms. 
This project will help identify platform/s that would generate high conversion rate with less revenue and vice versa. It will help examine how different platform perform and how budget should be utilized efficiently.


## Project Questions
The main objective of this analysis is to identify the key questions, such as;

	Which platform contribute the most in terms of volume and value from November 2023 to April 2024?

	Which platform delivered the highest return on investment and conversion efficiency?


## Data Structure 
This Dataset contains important metrics such as impression, clicks and conversion rates from November 2023 to April 2024. 
 
	Factmedia: This is the main fact table that contains media advertising rates of impressions, clicks, conversion, revenue, cost and profit. It also contains location rows that represent names of regions and countries.

Metrics calculated for the table are; conversion rates(Total clicks /Total conversion),Conversion efficiency(Total conversion/Total cost), Click-through rate(Total click/Total impression,0),cost per click(Total cost/Total clicks) and revenue per click(Total revenue/Total clicks).

	DimDate: The table contains column with day, weeks, months, month year and year.

<a href="https://github.com/laur196/Media-advertising-performance-optimization-November-2023--April-2024-/blob/main/Untitled%20(1).png"> Media Data Struture</a>

## Technical Details 
This project used Power BI for all stages of data cleaning and analysis, with a focus to address inefficient performance across media campaign from November 2023 to April 2024.

## Skills & Function Used 
Skills: Format, Calculate, Aggregate Function(Sum, Divide, AVERAGEX)

## Steps Overview:

Initial Inspection: Checked all source tables (DimDate, Factmedia) for missing values, duplicate entries and inconsistent data types.
Cleaning & Standardization: Converted Types.

Calculated Metrics: Conversion Rate, Conversion efficiency, Profit margin, Click-through rate, Cost per click and Revenue per click.

## Data Cleaning Actions
Here are the key cleaning operation;
	Converted Types
Purpose: Converted the cost row data type from whole number to decimal number for accurate analysis.

## Date Table Details 
The date was inserted into this StartDate and the last day was inserted as the EndDate and this helped to create a date table which contains day, weeks, month, month year, Quarter year, year. 

Steps Details 
The date table was created through this method;
let 
// Create parameter 
    StartDate = List.Min(FACT_MEDIA[Date]),
    EndDate = #date(2024,4,30),
    Step = Duration.Days(EndDate - StartDate)+1, 
// Create the List of date
    Source = List.Dates(StartDate,Step, #duration(1,0,0,0)), 
    #"Converted to Table" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Changed Type" = Table.TransformColumnTypes(#"Converted to Table",{{"Column1", type date}}),
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type",{{"Column1", "Date"}}),
// Create additional columns 
    #"Inserted Year" = Table.AddColumn(#"Renamed Columns", "Year", each Date.Year([Date]), Int64.Type),
    #"Inserted Quarter" = Table.AddColumn(#"Inserted Year", "Quarter", each Date.QuarterOfYear([Date]), Int64.Type),
    #"Inserted Merged Column" = Table.AddColumn(#"Inserted Quarter", "Year quarter", each Text.Combine({Text.From([Year], "en-US"), "/Q", Text.From([Quarter], "en-US")}), type text),
    #"Inserted Quarter Year" = Table.AddColumn(#"Inserted Merged Column", "Quarter Year", each Text.Combine({"Q", Text.From([Quarter], "en-US"), " ", Text.From([Year], "en-US")}), type text), 
    #"Inserted Month" = Table.AddColumn(#"Inserted Quarter Year", "Month", each Date.Month([Date]), Int64.Type),
    #"Inserted Month Name" = Table.AddColumn(#"Inserted Month", "Month Name", each Text.Start(Date.MonthName([Date]),3), type text),
    #"Inserted Month Year" = Table.AddColumn(#"Inserted Month Name", "Month Year", each Text.Combine({[Month Name], " ", Text.From([Year], "en-US")}), type text)
in
    #"Inserted Month Year"
