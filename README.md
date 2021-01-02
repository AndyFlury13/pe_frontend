# Frontend for Public Editor

[Public Editor](https://www.publiceditor.io/) is a news-vetting organization devoted to fighting misinformation in the media, and I've helped build its frontend. This repository has two related but independent codebases, contained in the following folders:
1. **visualizations**: visualize PE algorithm data on articles, creating an interactive UI where the user can explore the different fallacies of an article.
2. **newsfeed**: aggregate and organize PE-vetted articles in a specialized search engine.

## Visualizations
![Visualization.html image](https://www.dropbox.com/home/pe_github_pics?preview=Screen+Shot+2021-01-01+at+3.44.49+PM.png)
[Published Version](https://newsfeed.publiceditor.io/visualizations/47990959103662e94e796d979018922a/visualization.html)
After the text from article.txt has been loaded onto the page, the hard work begins. We then process vis_data.csv, which contains the output of PE's algorithm. In essence, vis_data.csv contains the locations of fallacies in the article and the severity of each of these fallacies. From vis_data.csv, we need to do two things: 1) place highlights on the text and 2) create the hallmark interactive visualization
##### Highlights
[Picture of highlights](IMAGE URL HERE)
To place highlights on the text, we process vis_data.csv line by line. For each line, start a highlight at start_index and end the highlight at end_index. The color of the highlight is dependent on the credibility indicator category, aka the general category of the fallacy. This is done with some nifty Jquery and string concatenation. For each highlight, we also track certain metadata.
visualizations/assets/creteHighlights.js.
#### Hallmark
[Picture of hallmark](IMAGE URL HERE)
To create the highlights, we again process vis_data.csv. This time, we create a tree data structure that will be the skeleton for the hallmark. The first layer of tree nodes is the overall fallacy category (Credibility Indicator Category), for example, "Reasoning". The leaf nodes will be the more specific subcategory (Credibility Indicator Name), for example, "Begging the question". We then need to sum the errors associated with each subcategory in order to find a sum for each overall category. Thus, a user will be able to find how many points an article loses due to Reasoning errors, as well as "Begging the question errors". From this tree structure, we can create a pie-chart shaped hallmark visualization using the D3.js package. The sections of the hallmark all correspond to different fallacy arguments, and their sizes represent how large the errors are in this article. This work is done in visualizations/assets/sunburstGenerator.js.
#### Connecting Hallmark to Highlight
One of the key functionalities of the PE frontend is that hovering over a highlight in the text lights up the hallmark and reveals information about said highlight. Hovering over the hallmark will light up highlights in the text, and clicking on a section of the hallmark will scroll the user down to the first instance of that fallacy in the article. This is done in both sunburstGenerator.js and createHighlights.js

## Newsfeed
[Newsfeed inage](IMAGE URL HERE)
The newsfeed acts as a PE's search engine, with the added benefit of seeing a simplified hallmark (1 layer) for each article. This requires reading in the article metadata contained in visDataTest.json. This json file contains a document for each article in the PE database. Based on the search conditions (see newsfeed/assets/newsfeed.js), we can obtain a set of articles to display on the newsfeed. We can sort by date, title, and credibility score. We create a div for each article, and load into the div the title, author, and first 100 characters of each article. In addition, I needed to create a small hallmark with one layer for each article in the newsfeed. This process is very similar to that of **visualizations**.

### Dependencies
article text -> algorithm data -> D3 manipulation
Sorting out dependencies turned out to be an incredibly tricky part of this project. I ultimately needed to use a cascading set of promises to ensure that no code executed before its requisite resources were fully loaded onto the page.
