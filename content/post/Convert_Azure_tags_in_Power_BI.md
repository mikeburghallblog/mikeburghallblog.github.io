---
title: "Convert Azure tags in Power BI" # Title of the blog post.
date: 2021-08-10T20:24:16+01:00 # Date of post creation.
description: "How to make use of Azure tags in Power BI" # Description used for search engine.
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
tags:
  - Convert Azure tags into a usable format
# comment: false # Disable comment if false.
---
If you are using Power BI and pulling data from Azure Cost Management you will most likely want to extract data from tags created within Azure. This isn't as easy as it seems.

Assuming you are familiar with Power BI and have imported data from Azure Cost Management  - here are the steps to making them usable;

1. Edit the query for the data set that contains the tags such as 'Usage details'.

![Edit Query](/images/power_bi/Edit_Query.PNG)

2. Add a Custom Column to the dataset.

![Add Custom Column](/images/power_bi/Add_Custom_Column.PNG)

3. Give the column a name and add { and } around the Tags column for the formula as shown in the diagram below (this is to make it a proper JSON type).

![Custom Column Formula](/images/power_bi/Custom_Column_Formula.PNG)

4. Find the new custom column, right click the column header and select Transform and then JSON.

![Transform to JSON](/images/power_bi/Transform.PNG)

5. Expand the JSON records now in the column to select the tags.

![Expand Records](/images/power_bi/Expand_Record.PNG)

6. Select the tags you want to be able to use and click OK. Note this takes a while to load and may not load all possible values, if this is the case click on the 'Load more' link.

![Select Tags](/images/power_bi/Select_Tags.PNG)

7. The selected tags appear as new columns in the dataset, with the column name the tag key and the tag values as the column values.

![New Columns](/images/power_bi/New_Columns.PNG)

8. Close and apply the changes to the dataset. Once done the dataset fully reloads which may take a while.

![Close and Apply](/images/power_bi/Close_and_Apply.PNG)

9. The columns are now usable columns within the dataset.

![Usable Columns](/images/power_bi/Usable_Columns.PNG)

