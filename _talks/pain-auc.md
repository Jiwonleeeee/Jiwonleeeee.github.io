---
title: "Area under the post-surgery pain scores curve"
collection: talks
type: "Internship work"
permalink: /talks/pain-auc
venue: "CCHMC"
date: 2024-09-28
location: "Cincinnati"
---

The area-under-the-curve (AUC) is a common measure to quantify continuous variables recorded over time. 

Check these papers: 

Cappelleri, J. C., Bushmakin, A. G., Zlateva, G., & Sadosky, A. (2009). Pain responder analysis: use of area under the curve to enhance interpretation of clinical trial results. Pain Practice, 9(5), 348-353.

Andersen, L. P. K., Gögenur, I., Torup, H., Rosenberg, J., & Werner, M. U. (2017). Assessment of postoperative analgesic drug efficacy: method of data analysis is critical. Anesthesia & Analgesia, 125(3), 1008-1013.

Suppose we have a dataset like this:


| record_id | surgery_dt     | painscore_dt   | painscore | POD |
| --------- | ----------     | ------------   | --------- | --- |
| ID1       | 03/22/21 10:31 | 03/22/21 11:40 | 5         | 0   |
| ID1       | 03/22/21 10:31 | 03/22/21 15:21 | 7         | 0   |
| ID1       | 03/22/21 10:31 | 03/23/21 09:22 | 6         | 1   |
| ID1       | 03/22/21 10:31 | 03/24/21 19:22 | 3         | 2   |
|    ...    |       ...      |    ...         | ...       | ... |
| ID10      | 07/15/21 14:12 | 07/15/21 18:52 | 8         | 0   |
| ID10      | 07/15/21 14:12 | 07/16/21 08:11 | 7         | 1   |
| ID10      | 07/15/21 14:12 | 07/17/21 12:08 | 4         | 2   |
| ID10      | 07/15/21 14:12 | 07/17/21 17:45 | 2         | 2   |


Patients are asked to rate their pain on a scale from 0 to 10 after surgery. Here, 'POD' stands for postoperative day. For example, if a pain score is recorded on the day of surgery, it is referred to as POD0, and scores recorded on the following days are labeled POD1, POD2, and so on. POD0 represents the time between the end of surgery and midnight, while POD1 and POD2 each represent 24-hour periods. Based on this dataset, we can create a post-surgery pain score graph like this:

<br/><img src='/images/auc-1.png'>


If the task is calculating the whole region under the curve, then we can just simply add the individual trapezoid as we have recorded times and corresponding scores.
<br/><img src='/images/auc-3.png'>

However, what we need is the AUC for each POD. For POD0, for example, we first evaluate the area of the very first trapezoid, then we need the area of the trapezoid A. To do this, we need to know the value marked as 'X'. This can be achieved by using the trapezoid midsegment theorem (i.e, simply using the ratios). This is called 'imputation'. 
<br/><img src='/images/auc-4.png'>

What makes it hard for this algorithm to automatically calculate the AUC for each POD is the missing cases. For example, what if there's no observation for POD 1? Then there's no need for the imputation and we simply pull the observation closest to the boundaries and evaluate the area of rectangles, this is the example:
<br/><img src='/images/auc-5.png'>


There are several other considerations. When we typically have multiple points for each period, we use the for loop to automatic calculation (we should exclude the both end points because they might need an imputation or not, so it's little bit tricky). But it's also possible that there's only one point in a period, then it doesn't need a for loop and whether we evaluate the area of rectangle or trapezoid is determined whether we have another points in the next period (so this is only applied to the POD0 and POD1 as POD2 always need a rectangle for the last observation). In this example, since POD1 is missing, we only evaluate the rectangle area for POD0. You can see that with just one point, the AUC might not be enough to accurately represent the pain level.
<br/><img src='/images/auc-6.png'>

There are many other cases that can cause your code to fail, so it's important to keep debugging, fixing issues, and running tests until you get the correct results for all cases! When you succeed, you should have the result like this:


| record_id | AUC0  | Duration0 | AUC1  | Duration1 | AUC2  | Duration2 |
| --------- | ----  | --------- | ----  | --------- | ----  | --------- |
| ID1       | 124.4 | 19        | 221.1 | 24        | 76.2  | 24        |
| ID2       | 34.6  | 12        | 125.5 | 24        | 175.6 | 24        |

This problem becomes a bit more complex when we want to categorize pain into mild, moderate, and severe levels. Typically, mild pain is defined as a score between 0-4, moderate between 4-7, and severe between 7-10. However, if you have the correct code for the simpler case, you can adjust it slightly. If the shape is a rectangle, you only need to focus on a single point, determine where it falls, and divide the region accordingly. If it’s a trapezoid, the approach changes slightly. For example, in the diagram, 'B' is the point we're interested in.
<br/><img src='/images/auc-7.png'>

Since one point is greater than 7, while the other is between 4 and 7, we can split it further into B1 (severe pain region) and B2 (moderate pain region). Using the trapezoid midsegment theorem, we can calculate the value for the red 'X' point. The area of B1 will then contribute to the severe pain AUC, and B2 to the moderate AUC.
<br/><img src='/images/auc-8.png'>



The result you should have is like this:


| record_id | AUC0_mild | AUC0_mod | AUC0_sev | Duration0 | ... |
|-----------|-----------|----------|----------|-----------|-----|
| ID1       | 30.5      | 48.6     | 26.8     | 12        | ... |
| ID2       | 34.6      | 12       | 79.2     | 20        | ... |


We can combine this AUC output with the demographic information from each patient and explore some interesting questions, such as: is there a relationship between a patient's race, age, place of residence, or household income and their recovery after surgery? We actually did amazing work on a project like this and submitted papers on it. I can't wait for them to be published!












