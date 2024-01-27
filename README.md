# Shark Attack Data Analysis Project


## Problem Statement

A nature website, Wanderleaf, is publishing an article on shark attacks. The writer of the article, Adrian Chambers, has been asked to collaborate with the data analysis team to gain insights into shark attacks thus supporting the content creation process.

Mr. Chambers has specified that the range of time he is interested in is 1917-2017. He also identified that the key insights he is interested in for the article are:
- Trend of shark attacks with time over the years
- Seasonal trends in shark attacks
- Location of the attacks
- Gender and age range of the victims
- The mortality rate of the attacks

The data analysis team has been tasked with developing a dashboard to aid Mr. Chambers in gaining insight into shark attacks for the range of years of interest.

## Project Planning
1. Purpose: To gain accurate insight into shark attacks, thus facilitating the content creation process.
2. Stakeholders:
- Mr. Chambers (writer)
- Data Analytics team
- Content lead 
3. End result: A dashboard providing necessary insights into shark attacks from 1917-2017.
4. Success Criteria: The success criteria is the creation of a dashboard that offers accurate insights regarding:
- Trend of shark attacks with time over the years
- Seasonal trends in shark attacks
- Location of the attacks
- Gender and age range of the victims
- The mortality rate of the attacks

## Project Steps

### Data collection
A dataset of shark attacks was collected from Kaggle (https://www.kaggle.com/datasets/mysarahmadbhat/shark-attacks). This data was pretty raw and needed a lot of transformation before analysis.


### Data cleaning
1. Initial Data exploration: The data set contained 6094 rows with 22 columns 

![Data Overview](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/f5eabffd-fb8a-45c2-808c-26a7d7ab0480)

2. Remove duplicates: Duplicates were eliminated from the dataset using the remove duplicate data tool.
3. Create an appropriate date column for analysis: A month-year date column would be appropriate for this study as it would help track seasonal and yearly trends throughout the time period of concern. However, due to data quality issues that would be discovered later, not all attack entries that have a year entry have a month entry. So separate month and year columns would be developed.


Three columns relevant to date are present in the dataset case number, date, and year. The date column was quite messy, while the year and case number columns were in better condition.

![Date Columns](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/cfc11414-6575-4266-af13-3c6286fec0c5)

Weird entries in the year column, like '0' and blank entries, were examined further using the date and case number column. It was discovered that the blank entries could be replaced from the date portion of the case number column, while the '0' entries could not be rectified for the purpose of this study, even using the other two columns.

Validating the case number column:

- Checking if the year portion of the case number aligns with the year column 
=IF(VALUE(LEFT(A2,4))=VALUE(D2),TRUE,FALSE)

Only 134 entries showed issues. 120 of these were data collection problems from the source and could not be rectified. 

![Year portion of case study 1](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/9faac815-f14a-4b22-bb68-a30bb1c83fc4)


From the remainder, some were data entry problems that could be rectified by comparing the Case Number, Date, and Year columns and picking the year that was indicated by two of the columns. Others had 0 in the year portion of the case number.


![year portion of case study](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/2126b506-cd5c-45eb-aadf-18d3d722f215)


- Check if the month portion of the case number aligns with the month portion of properly formatted entries in the Date column

=IF(VALUE(MID(A2,6,2))=MONTH(B2),TRUE,FALSE)

In cases where the date entry was properly formatted, it appears that most of the month portions of the case number align with the date column. In cases where the date column was not formatted properly, the month section of the date still appears to align with the month section of the case number.

![Month Check](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/ce34a4f5-00c6-49e1-a4cc-e4460c41052a)


Based on the validation of the case number column, it was selected to create a new month column to track seasonal trends, while the year column was cleaned up and used as is. 

a. First, the blank entries in the year column were rectified using the case number and date column

b. the year column was used to filter the data to only show entries between 1917 and 2017

c. The following formula was used to extract the month portion of the case number column:

      =VALUE(MID(A2,6,2))

d. Extracted month entries of 0 were filtered out, and then the following formula was used to convert the month numbers to month names

      =TEXT(DATE(2022, C2, 1), "mmmm")

e. Then, month entries of 0 were given an entry of 'Unknown' in the new Month column. Although some of the corresponding entries in the date column had months, suspiciously, most of the entries with the no month issue had the same year '1905' in the date column even though the had various years in the case number and years column. This might be a data entry problem.


4. Clean country column
a. a well-formatted list of countries, islands, and oceans was imported into a new sheet containing a pivot table attached to the data

b. A pivot table was used to extract the unique entries in the country column then the following function was used to check if those entries were found in the list of countries, islands, and oceans imported.

=IF(ISNUMBER(MATCH(A8, $H$4:$H$2349, 0)), "Found", "Not Found")

![Country clean set up](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/76c49e39-425c-444c-b5a8-ff2be9fb6780)

c. Items not found on the list were further investigated; some were typographical errors, while others were actual places but were not on the list. Through this method, records like these were unified

![incorrect countries](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/2472e491-f22c-4315-afee-0bb3673324c8)

d. Then, a text formatting formula was implemented to standardize and clean the country column further. The "USA" entries were filtered out before this formula was applied because the capitalization was already as required, and the pivot table showed they were clean.

= PROPER(TRIM(SUBSTITUTE(SUBSTITUTE(J2, "  ", " "),"?","")))

![Cleaning country](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/8d268239-2f8c-48fb-b75d-ed0c5bb116a5)

e. The following formula was used to identify country entries containing "/" only six were found, and they were replaced with 'Unknown'

=IF(ISNUMBER(FIND("/", J2)), "Slash found", "No slash")

![country slash](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/d98ad32a-213a-438a-a3a1-ff2bf3863e91)

5. Clean sex column
a. a pivot table was used to identify potential errors in the column. Extra spaces were identified in some entries.

![Sex pivot](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/461fa055-53d3-475b-b9c4-df0522bc6c0d)

The following formula was used to remove them.

=TRIM(N2)

![Sex trim](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/257b8907-3dee-4ab4-8222-2879baad27d5)

b. Subsequently, "F" and "M" were filtered out, and other erroneous entries were replaced with 'Unknown'

c. Gender entries for shark attacks with multiple victims to Unknown'. This decision was taken because they were very few, and ascertaining the gender of the victims was not so straightforward in some entries

6. Clean age column

Age range categories were developed to replace the age entries to aid in the analysis process.

      Infant: 0-2 years
      Toddler: 3-5 years
      Child: 6-12 years
      Teen: 13-19 years
      Young Adult: 20-39 years
      Middle-Aged: 40-59 years
      Senior or Elderly: 60+ years

Examining the filter drop-down showed that there were some text entries in the age column. The following formula was used to identify text entries.

=ISTEXT(P19)

The non-text entries were checked for decimal numbers using the following formula, and none were found.

=IF(ISNUMBER(FIND(".", P2)), "Contains Decimal", "Doesn't Contain Decimal")

 The text entries were filtered for by filtering for 'TRUE' results  in the text check column (=ISTEXT(P19))

![age text](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/94df9f37-c30e-42b8-a84f-6011ff4b9c29)

a few entries were returned. some repeated entries were observed, and the following formula was used to convert some of the text entries into the desired age category.

=IF(OR(TRIM(P19)="Teen", TRIM(P19)="teen"), "Teen", IF(OR(TRIM(P19)="20s", TRIM(P19)="30s"), "Young Adult", IF(OR(TRIM(P19)="40s", TRIM(P19)="50s"), "Middle-Aged", IF(TRIM(P19)="60s", "Senior", "N/A"))))

![Clean age text](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/e5618401-fa7a-4b4b-a4d2-82809cead683)

The 'N/A' entries created by the formula were filtered for and examined further.

![Age NA](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/de25f8cd-7459-4bc8-92da-274806df446c)

some were manually modified to the required age categories

A very few shark attack entries were about multiple people being attacked at the same time. Age entries for these situations were replaced with 'Unknown'

The numeric entries were converted to age ranges using the following:

=IF(P2>=60, "Senior", IF(P2>=40, "Middle-Aged", IF(P2>=20, "Young Adult", IF(P2>=13, "Teen", IF(P2>=6, "Child", IF(P2>=3, "Toddler", IF(P2>0, "Infant", "")))))))

![Age range](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/ad9d3f72-cf1e-442d-885f-a0a4056fb0f5)


7. Clean fatal column
the name of the column was changed from 'Fatal(Y/N)' to 'Fatal'

A pivot table was used to examine the unique entries, and some entries with extra spaces were observed.

![Fatal pivot](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/a4b23bbd-ae04-4c41-8236-f97a1921a34e)

these entries were rectified using:

=TRIM(F2)

![Fatal trim](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/a8ca2e23-6e6b-4c4f-ba76-8f2220a89383)

b. Subsequently, "Y" and "N" were filtered out, and other erroneous entries were replaced with 'Unknown'

8. Other columns were eliminated. 
The final cleaned dataset is shown below.

![Final Clean](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/e4a1852a-8729-40ad-b3d7-e6f555d536c9)


### Data Visualization

#### Data Transformation

Two columns were added to the dataset to aid the visualization process.

- The month column was renamed to 'Month Order'
- A new month column was created by duplicating 'Month Order'

      Month = 'Cleaned Attacks'[Month Order]

- The 'Month Number' column was created by assigning numbers 1-12 to the months from January-December

      Month Number = 
      SWITCH (
            [Month Order],
            "January", 1,
            "February", 2,
            "March", 3,
            "April", 4,
            "May", 5,
            "June", 6,
            "July", 7,
            "August", 8,
            "September", 9,
            "October", 10,
            "November", 11,
            "December", 12,
            BLANK()
      )

- The  'Month Order' and 'Month Number' columns were hidden from the report view to prevent confusion.

- The 'Month Number' column was then used to sort the newly created 'Month' column. This would aid in sorting the 'attacks by month' visualization by the calendar order of months.

The final data table is shown below:

![FInal Data Table](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/0b6fee92-6f8f-4dee-a3e4-87dd51e089d9)

#### Dashboard

The following dashboard was created using the final data table.

![Dashboard](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/e8452a26-f127-476c-8a4e-f3ce74ef1231)



### Shark Attack Insights
1. Attack Yearly Trend: 

      ![Attack yearly trend analysis](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/1535ec95-f140-47ab-bb85-3b8e7a87a71c)

There has been a gradual rise in shark attacks over the years, with a spike in attacks from 1959-1961.

2. Attack Monthly Trend:


      ![Attack by month](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/fe2ce7f1-2e4a-4d9e-9399-6323b89ea34c)

The attacks vary with the months, with July, August, and September accounting for most of the attacks.


3. Attacks by Gender: 

      ![attack by gender](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/67764133-6503-4f80-bac5-a4290c42019c)

Males are the major victims of the attacks as they account for over 85% of the attacks.

4. Attack Fatality: 

      ![Attack fatality](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/af13c336-dd78-4fa0-9e20-6b62a55f0903)
      
Most shark attacks are not fatal, as non-fatal attacks accounted for over 75% of the attacks.

5. Location of Attacks: 

      ![Attack location](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/5962d4ae-de9d-4506-8eb5-1a7d6f7408dd)

Most attacks take place in the USA, with Australia and South Africa coming in second and third. These three countries account for about 70% of the attacks. 

6. Attacks By Age Range: 

      ![Attacks by Age range](https://github.com/Jucodez/Shark-Attack-Data-Analysis/assets/102746691/acd0e1ac-13cd-4eb8-8f50-386d540bf253)

Young Adults account for most of the attacks, followed by teens.





## Conclusion 

Overall, this dashboard helps Mr. Chambers, the content creator, have a better understanding of the trends and statistics of shark attacks for the desired time range. This would aid in the development of the article and support the company in its drive to provide insightful articles to its customers.
