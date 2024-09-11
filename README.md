### Uber Drive Data Case study (using Excel)

#### Scenerio

The given dataset has following columns:

- START_DATE: starting date and time(24 hours format) of Drive
- END_DATE: Ending date and time(24 hours format) of Drive
- CATEGORY: business or personal
- START: start location
- STOP: stop location
- MILES: miles travelled
- PURPOSE: purpose of drive eg meal/entertainment, meeting, customer visit, commute.......

I need to find out unique start and end points, highest miles travelled, highest speed and for what purpose, average speed per category, etc.


####  Steps followed

1. Applied filter to all columns to check column distribution, values present, blank values, null values, etc, also checked duplicates.
2. I found out dates and times in START_DATE and END_DATE were not in consistent format(some in date while some in text format).
3. Made date and time consistent 

select > data > text to column > fixed width > next, next > date format mdy > finish

4. I got date and time in two different columns. 
5. Calculated time difference by " END_TIME - START_TIME "
6. Few values were negetive. This is because, the start time was high compared to end time. Some drives were started on previous day eg 23:00 i.e. 11 pm and ended next day eg 01:00 i.e. 1 am. Hence the difference was negetive. 

To solve this anomaly, used
= IF(START_TIME > END_TIME, END_TIME-START_TIME+1, END_TIME-START_TIME)

7. Converted this time difference in minutes by multiplying with 24*60.

8. Calculated time in hours by time in minutes divided by 60.
9. Calculated speed= miles/hours
10. Checked speed for anomalies. Got #Div error in few rows. Some time differences were zero. As the miles got divided by 0, we got this error. I deleted those rows with #Div error.

11. Also, I found the speed were greater than 200 in some rows which was not possible. (Some drives were lasted for 2-3 days hence, we were getting this error) So, better to delete these extreme values.

12. To find extreme values,

=QUARTILE(range,1).....for 1st QUARTILE
=QUARTILE(range,3).....for 3rd QUARTILE

INTER QUARTILE RANGE= 3rd-1st QUARTILE

Lower Limit(LL) = QUARTILE 1)-1.5*IQR
Upper Limit(UL) = QUARTILE 3)+1.5*IQR


Quartile range describes the spread of middle 50% of the data. 
By focusing on the range of the middle 50% of data, it can help identify outliers, as values falling far outside this range might be considered outliers.

Outliers are often defined as values that fall below 
𝑄1−1.5×IQR or above Q3+1.5×IQR. Thus, IQR helps in identifying and understanding outliers.

Hence we will neglect the outliers.
LL and UL were found -5 and 48 resp. I will neglect the outliers. 

13. After cleaning the data and performing the required calculations, I used the pivot table to answer the queries. 

14. To find out unique start points, START was selected in pivot table rows, found out 177 unique start points using COUNTA formula. similarly, found out 188 unique stop points.
![image](https://github.com/user-attachments/assets/e4b5d187-e591-4266-9238-932915559f4e)

![image](https://github.com/user-attachments/assets/202ca8a5-7b79-41c6-847e-619d656ed94a)

15. To find out highest miles travelled, in pivot table, I selected START and STOP in rows and sum of miles in values. The max miles was found Morrisville to carry(395 Miles)
![image](https://github.com/user-attachments/assets/420310df-257e-4e26-9910-fef5e63a419a)

16. To know the highest average speed travelled and for what purpose, I selected purpose in rows and average of miles in values section of pivot table.
![image](https://github.com/user-attachments/assets/7006ef07-ab54-46ea-817c-da92a1bce14a)
the answer is: For commute 58.44 miles/hour average speed.

17. To learn about the average speed per category, I selected category in row section and average of miles in values section. 

![image](https://github.com/user-attachments/assets/931e72ae-241e-495f-93ff-4b692c861409)
the required answer is seen in snapshot. 

Thank you. 
