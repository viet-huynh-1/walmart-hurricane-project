# Reflection: Walmart Hurricane Analytics

DS2002 Midterm, Viet Huynh (solo team, approved by Prof. Williamson)

## 1. Data Quality Impact

The issue that mattered most was SKU fragmentation. Strawberry Pop-Tarts was recorded under four SKUs and six name spellings. I merged them only where the transaction data supported it: same name spellings, same category, and prices within 1%. Before consolidation, the best Pop-Tart SKU showed 103 units a day in the storm week and ranked third, behind two water SKUs. After consolidation, Pop-Tarts sold 393 units a day and ranked first. Without the fix, the analysis would have named water as the main storm item and sized Pop-Tart orders at about a quarter of real demand. The percentage surge looked the same in every SKU, so the problem only shows up in volumes, which is what actually gets shipped.

## 2. ETL Trade-offs

884 transactions had no timestamp. The simple option was to drop them. Because transaction ids are issued in daily blocks, I instead filled in each missing date from the neighboring ids when both neighbors fell on the same day, and tested the rule on rows with known dates, where it was correct every time. Dropping the rows would have cut total units from 49,775 to 48,085 and Pop-Tarts' storm-week sales from 393 to 378 a day, while barely changing the surge multiples. So the choice affects how much to ship more than the trends. I would make the same call, but it depends on ids being issued in date order, which would not hold if registers numbered sales separately.

## 3. Pipeline Trust

I agree with the CDO. The analysis itself was simple; the errors that could have changed the answers all came from the pipeline, and none of them raised an error. The most fragile part was time. Sales are in Eastern time in five formats, the weather API returns UTC and mislabels September hours when asked to convert, and the SQLite dates are text in two formats, so a plain date filter skipped 22 of 360 rows. I also introduced a bug myself: my first duplicate check kept the re-entered copy of some sales instead of the original. The decision log caught it because the removed ids were in the wrong range. For the capstone I would move the cleaning into a tested Python module so those checks run automatically.

## 4. Business vs. Data

Board games did sell more, 2.67 times their baseline, across all three SKUs and all 12 stores. But total store traffic rose 2.47 times, so their lift over traffic was only 1.08, with an interval that includes 1. The data shows the extra sales are real but can't show they were caused by the storm. On the business side, overstocking board games is cheap because they don't spoil, but truck space is not: peak demand was 3.6 times normal while the distribution centers can only ship about twice normal. So I recommended stocking board games in line with traffic and saving truck space for items like Pop-Tarts and water. With more data I would check the same lift across other storms and look at whether board games were bought in the same baskets as storm supplies.

## 5. Team Collaboration

I worked alone, with Prof. Williamson's approval, so I took the charter's roles in order: cleaning, then the weather join, then the questions, then charts and writing. Without a reviewer, I relied on checks in the code, such as row-count assertions on every join and a test of the date-filling rule. Those caught some mistakes, but a second person would probably have caught the duplicate bug and an overstated chart title sooner. The biggest non-data problem was getting the weather API to work everywhere, which led me to add retries and a saved fallback copy. With another week, I would test the findings on a second storm, add NHC forecast data, and put the cleaning code under tests. The most useful habit was not trusting numbers just because they looked reasonable.
