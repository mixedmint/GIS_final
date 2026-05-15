# Instructions

## Submission

On clicking the exam link a new repository has been created for you in the exam classroom. You should now clone this repository into a new RStudio project then commit and push to GitHub as usual. Remember that your repository must appear in the GitHub classroom platform. Do not clone the repository to a location outside the GitHub classroom as this will not be marked.

GitHub takes a snapshot of your local git repository at the deadline so commit and push often (e.g. once per hour), as you should do in all spatial data science projects. The deadline is based on your last commit, but we expect the repository to be pushed to GitHub very soon after.


## Before you begin: 

* Go to the top of the `exam_response.Rmd` RMarkdown document and edit the name and student number fields (at the top of the .Rmd).

* Complete the originality declaration


## Task

You have six hours to complete this open book exam. You must select and undertake only one of the research questions below. Links to the data for each question have been provided and you are free to source additional data for extension analysis, but everything you need is provided.

* You must write your response in the exam_response.Rmd, under the originality declaration.

* You may use any resource to assist you but the work must be your own and you are required to sign an originality statement within the exam. Clearly acknowledge and state if/where AI has been used.

* Questions about the exam must be asked on the open Slack GIS channel.

* If you copy a section of code from an online source please provide a relevant link or acknowledgment.

Marks are awarded as per the marking scheme. It's important to note there is no 'right' answer, even if your findings are inconclusive or not as expected, you are awarded marks for how you approach the problem.

## Within your work you must:

* Provide an initial project scope in bullet point form. Your project scope should include:

  * If you intend to propose a variation of the original question (e.g. selecting a specific year of data to analyse), this must be based on appropriate reasoning and clearly stated.
  * A brief background to the topic.
  * A research question and/or hypothesis.
  * An evaluation of your research datasets.
  * An explanation of additional data that you might have sourced for your analysis (it is not a requirement to source additional data).
  * An explanation of the data wrangling you intend to undertake.
  * A justified plan for the spatial data analysis that will be required.
  * You may also wish to identify any constraints around the data you have been instructed to analyse or obvious omissions from the set task that could limit what will be produced in this short project. These could relate to spatial or temporal limitations in the dataset, what you decide is reasonable to analyse or anything else that is relevant.


* Commit and push regularly (e.g. once per hour) during the final exam.

* Provide any data your have sourced, unless it exceeds the GitHub upload limit, in which case a URL that links directly to the exact dataset must be given.

* Produce a well commented and fully explained RMarkdown document that attempts to answer the research question.

* Create at least one graphical output and at least one mapped output.

* Critically reflect on the results you have produced.

## Tips:

* In the time you have, prioritise good solid analysis tailored to answering your research question over elaborate approaches that produce lots of results that don’t directly contribute to answering your research question. 

* Structure your RMarkdown document with titles and subtitles.

* Comment and explain your working throughout.

* State assumptions and describe limitations.

* In most questions some administrative boundary data has been provided, use this to assist guiding recommendations and outputs.

* Provide critical commentary about the data you are using and the analysis you are undertaking throughout.

* Plan your time. We suggest 1 hour for data exploration, 2-3 hours for analysis, 1 hour for visualisations, 1 hour for interpretation and reflection. 

# Final Exam Questions

## TfL’s Vision Zero action plan

Transport for London aims to eliminate all **deaths and serious injuries** from London’s transport network by 2041. They have appointed you as a consultant to investigate and draw preliminary insight from their casualty data. 
You should use appropriate data processing and analysis methods to produce an overview report which summarises the patterns revealed in the data. It is expected that at least some of the methods you use will relate to the spatial dimensions of the data.
Your report should include a brief introduction including relevant contextual information at the beginning and a critical review of your findings at the end. You must include at least one map.

### Suggestions

Many possible research questions could be set covering point patterns, spatial autocorrelation and explaining patterns. You might consider **one** of the following:

  * Exploring if values are similar across London
  * Exploring factors that might influence collision incidence ***rate***. You will need to make some assumptions on representation with the independent variables . 

### Useful code

Should you wish to subset spatial units (e.g. MSOAs) to London the following code will be helpful.

```
  # centroids of MSOAs across UK
  MSOAs_centroids <- st_centroid(MSOAs)

  # centroids within the London boundary. London is an sf object of the Greater London boundary 
  London_MSOAs <- MSOAs[st_within(MSOAs_centroids, London, sparse = FALSE), ]

```

If you were to compute metrics such as total pedestrian walkway length (or other road length) per spatial unit (e.g. MSOA) from the OSM road data the following code will be helpful:

```
 # extract pedestrian roads from OSM road shapefile
  pedestrian <- roads %>%
   filter(fclass %in% c("pedestrian", "footway", "path", "steps"))%>%
   st_transform(., 27700)

  # intersect pedestrian street and MSOA
	ped_MSOA <- st_intersection(pedestrian, MSOAs)%>%
	# get the length of each intersection 
 	 mutate(length_m = st_length(geometry))%>%
	# group the data (lengths) by the MSOAs
  	group_by(MSOA21CD) %>%  
	# compute the total length per MSOA
  summarise(total_ped_length_m = sum(length_m)) %>%
  ungroup()  

```
Remember that administration units (e.g. wards) are different to census units (e.g. output areas, LSOAs, MSOAs). 

### Data

  * Casualty collision data (2005-2024) and relevant documents: https://tfl.gov.uk/corporate/publications-and-reports/road-safety 
  * London boundary data: https://data.london.gov.uk/dataset/statistical-gis-boundary-files-london/ 
  * 2021 MSOA boundary data: https://geoportal.statistics.gov.uk/search?q=BDY_MSOA%20DEC_2021&sort=Title%7Ctitle%7Casc 
  * 2021 Census data: https://www.nomisweb.co.uk/sources/census_2021_bulk 
    * Household deprivation
    * Hours worked
    * Distance travelled to work
    * Car or van availability 
    * Disability
    * General health
    * Number of usual residents in households and communal establishments 

  * Open street map: https://download.geofabrik.de/europe/united-kingdom/england/greater-london.html
  
## Gambling related harms

According to Public Health England (2023) the “UK has one of the largest gambling markets in the World, generating a profit of £14.2 billion in 2020”. In 2018 Public Health England commenced a review of the health aspects of gambling-related harm being completed in 2023. The review failed to take into account spatial elements of the data. You have been enlisted as a consultant and tasked to conduct an analysis of the problem in London. 

You should use appropriate data processing and analysis methods to produce an overview report which summarises the patterns revealed in the data. It is expected that at least some of the methods you use will relate to the spatial dimensions of the data.

Your report should include a brief introduction including relevant contextual information at the beginning and a critical review of your findings at the end. You must include at least one map. 

### Suggestions

Many possible research questions could be set covering point patterns, spatial autocorrelation and explaining patterns.You might consider one of the following:
  * Exploring if locations or densities are similar.
  * Exploring factors that might influence gambling shop density. You will need to make some assumptions on representation with the independent variables 

### Useful code

There are many versions of gambling establishments (e.g. lottery, bingo, casino), we recommend focusing on betting stores otherwise known as “***bookmakers***”. 

Should you wish to subset spatial units (e.g. MSOAs) to a study area the following code will be helpful.

```
  #centroids of MSOAs across UK
  MSOAs_centroids <- st_centroid(MSOAs)

  #centroids within the London boundary. London is an sf object of the Greater London boundary 
  London_MSOAs <- MSOAs[st_within(MSOAs_centroids, London, sparse = FALSE), ]

```

To extract bookmakers from OSM data the following code will be helpful.

```
  #Install and load the osmdata package
  Study_area<- st_read("load study area spatial data")
  
  Bounding_box <- getbb("Study_area")
  #check it worked
  Bounding_box 
  
  #get the OSM data within the bounding box
  OSM_bookmaker <- Bounding_box %>%
  opq() %>%
  add_osm_feature(key = "shop", value = "bookmaker")%>%
  osmdata_sf()
  
  #extract the polygons of bookmakers
  bookmakers<-OSM_bookmaker$osm_polygons

```
You may need to convert the bookmaker polygons to points.

```
  #convert to points
  bookmaker_points <- st_centroid(bookmakers)
  
```

### Data

  * London boundary data: https://data.london.gov.uk/dataset/statistical-gis-boundary-files-london/ 
  * 2021 MSOA boundary data: https://geoportal.statistics.gov.uk/search?q=BDY_MSOA%20DEC_2021&sort=Title%7Ctitle%7Casc 
  * 2021 Census data: https://www.nomisweb.co.uk/sources/census_2021_bulk 
    * Household deprivation
    * Hours worked
    * Disability
    * General health
    * Number of usual residents in households and communal establishments 
    * Number of people in household who have previously served in UK armed forces
    * Highest level of qualification
    * Religion
    * Unemployment history 
    * Tenure 


