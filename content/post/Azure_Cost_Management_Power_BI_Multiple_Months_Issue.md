---
title: "Azure Cost Management Power BI Multiple Months Issue" # Title of the blog post.
date: 2021-09-03T18:00:01+01:00 # Date of post creation.
description: "How to make resolve the issue of pulling multiple month's of data into Power BI from the Azure Cost Management data connector" # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: false # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
#featureImage: "/images/path/file.jpg" # Sets featured image on blog post.
#thumbnail: "/images/path/thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
#shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
#codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
#codeLineNumbers: false # Override global value for showing of line numbers within code block.
#figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Power BI
  - Azure Cost Management
tags:
  - Azure Cost Management data connector month's of data issues
# comment: false # Disable comment if false.
---
If you are using Power BI and pulling data from Azure Cost Management you will most likely want to pull in multiple months worth of data. This usually is fine, however there can sometimes be issues, in particular with RI Usage Summary. In this case we need to implement a workaround.

Assuming you are familiar with Power BI and have imported data from Azure Cost Management  - here are the steps to resolving the issue with pulling in multiple months of data;

1. Edit the query for the data set that contains the tags such as 'Usage details'.

![Edit Query](/images/power_bi/date_issue/Edit_Query.PNG)

2. Click the Advanced Editor.

![Advanced Editor](/images/power_bi/date_issue/Advanced_Editor.PNG)

3. This will probably look like the below, where the 6 on the second row is the number of month's of data being retrieved. Note the enrollmentNumber has been obfuscated.

![Standard Data Retrieval](/images/power_bi/date_issue/Standard_Data_Retrieval.PNG)

4. Change the query to the below. This gets the data in 3 steps of 6 to 3 months ago, 3 to 1 months ago and 1 month ago to now. change these numbers to suit. Note the maximum is 3 months per step, you can replicate a step to get more than 6 month's of data. The text is at the botom of the page for you to copy and paste.

![Updated Data Retrieval](/images/power_bi/date_issue/Updated_Data_Retrieval.PNG)

5. Once updated click 'Done' to update the query, and then click 'Close & Apply'. You can then refresh the data to pull back the multiple months data without issues.

![Close and Apply](/images/power_bi/date_issue/Close_and_Apply.PNG)


    let
        enrollmentNumber = "XXXX",
        optionalParameters1 = [startBillingDataWindow = "-6", endBillingDataWindow = "-3"],
        source1 = AzureCostManagement.Tables("Enrollment Number", enrollmentNumber, 3, optionalParameters1),
        riusagesummary1 = source1{[Key="riusagesummary"]}[Data],
        optionalParameters2 = [startBillingDataWindow = "-3", endBillingDataWindow = "-1"],
        source2 = AzureCostManagement.Tables("Enrollment Number", enrollmentNumber, 3, optionalParameters2),    
        riusagesummary2 = source2{[Key="riusagesummary"]}[Data],

        optionalParameters3 = [startBillingDataWindow = "-1", endBillingDataWindow = "0"],
        source3 = AzureCostManagement.Tables("Enrollment Number", enrollmentNumber, 3, optionalParameters3),
        riusagesummary3 = source3{[Key="riusagesummary"]}[Data],

        riusagesummary = Table.Combine({riusagesummary1, riusagesummary2, riusagesummary3})
    in
        riusagesummary
