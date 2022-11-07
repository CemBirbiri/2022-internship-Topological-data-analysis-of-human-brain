# Internship-2022-Topological-Data-Analysis
I finished my internship at [INRIA,France in MathNeuro research group](https://team.inria.fr/mathneuro/) under the supervision of [Prof Mathieu Desroches](https://www-sop.inria.fr/members/Mathieu.Desroches/).

[The Human-Connectome dataset](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) was used in this project that consists of 998 patients with cognition, motor, emotion characteristics as well as brain features: volume, thickness, surface area, gray matter volume, white matter volume,...


I applied Topological Data Analysis (more specifically the Mapper algorithm) to cluster the patients according to their age and found different characteristics of subgroups. I found 2 younger group(age: 22-30), 1 older group(age: 31+) and 1 mixed-age group. The [DyNeuSR](https://braindynamicslab.github.io/dyneusr/) and [GUDHI](https://gudhi.inria.fr/) libraries were used in topological data analysis.

A summary of the internship is below:

# What is Topological Data Analysis (TDA) ?

Data gathering and analyses become a fundamental power in all research communities
such as medicine, engineering, social sciences, economy or mathematics, to name but a
few. The amount of data gathered from different areas grows in a gigantic rate therefore it
is essential to find a scientific methods to analyse them and extract from them meaningful
knowledge. Understanding the shape of data brings us to topology, which is a branch of
mathematics. Topology is the field of mathematics that studies how space is “connected”.
It was first studied by Swiss mathematician Leonhard Euler in the 18th century. Over
the last 20 years, topological techniques have been used in various applied problems, one
of these fields is called Topological Data Analysis (TDA). TDA provides detailed information about the characteristics of the data by providing some topological, statistical and
geometrical methods to infer complex topological structures such as connected components, holes, branches or loops. This analysis method is very robust to outliers and noise
where the data is usually represented as point clouds or distance matrices in a Euclidean
metric space. 

There have been many promising results in recent years for applying topological and
geometric approaches in different areas. TDA was used in material sciences, shape
analysis of 3D objects, analysis of images in various domains, time series
analysis, medicine, biology, genomic data, or chemistry.
Topological approaches and how to use them in interdisciplinary research topics and data
science is currently a very active research domain.


## The work is done in the following steps:

1. Preprocessing of the data ;
2. Applying the Mapper algorithm and extracting the low-dimensional shape of data points, which is the visualization result obtained from Mapper ;
3. Detecting communities from the result of Mapper ;
4. Discovering what features make these communities unique.


The key idea of the Mapper algorithm is to map data points into a metric space using
some filters defined on data points while capturing topological and geometric information
at a specified resolution and gain. The filter function (also called a lens) is the metric that
allows to map our data points. An example of Mapper algorithm of a 'hand' dataset is shown below:

<img width="250" alt="Screen Shot 2022-11-07 at 16 32 13" src="https://user-images.githubusercontent.com/46814542/200349905-b2812411-7254-4ad0-9302-8bd35ea1aa16.png">

## The final Dataset:

My final dataset(998,51) consists of following features :

1. Cognition columns(14)
2. Motor columns(4)
3. Emotion columns(8)
4. Age and Gender columns(2)
5. Anterior Cingulate Cortex(8) :
- Left caudal-anterior-cingulate Surface Area,
- Left rostral-anterior-cingulate Surface Area,
- Right caudal-anterior-cingulate Surface Area,
- Right rostral-anterior-cingulate Surface Area,
- Left caudal-anterior-cingulate Average Thickness,
- Left rostral-anterior-cingulate Average Thickness,
- Right caudal-anterior-cingulate Average Thickness,
- Right rostral-anterior-cingulate Average Thickness,
6. Orbitofrontal Cortex(8) :
- Left lateral-orbitofrontal Surface Area,
- Left medial-orbitofrontal Surface Area,
- Right lateral-orbitofrontal Surface Area,
- Right medial-orbitofrontal Surface Area,
- Left lateral-orbitofrontal Average Thickness,
- Left medial-orbitofrontal Average Thickness,
- Right lateral-orbitofrontal Average Thickness,
- Right medial-orbitofrontal Average Thickness
7. Total cortical gray matter volume(1)
8. Total subcortical gray matter volume(1)
9. 9. Total gray matter volume(1)
10. Supratentorial volume(1)
11. Total cortical white matter volume(1)
12. Topological features(2) extracted from connectivity matrices of patients


## RESULTS

The mapper result is shown below:

<img width="300" alt="Screen Shot 2022-11-07 at 16 41 15" src="https://user-images.githubusercontent.com/46814542/200352096-1167d846-2c1f-4845-b22f-c1f3b76ee075.png">


## Significant characteristics and clinical features specific to subgroups :
#### G1[31+]
We found 25 clinical variables significantly specific to G1[31+] (n=197). Among
these clinical variables, there is only one clinical feature that is unique to G1[31+], which
is Negative Affect (Sadness). 

#### G2[22-30]
There are 24 clinical variables significantly specific to G2[22-30] (n=263). There is
no feature that is only unique to this group. 

#### G3[22-30] 
24 clinical variables were found to be significantly specific to G3[22-30] (n=335).
There are two variables that are unique to this group such as Left rostral-anterior-cingulate
Avg Thickness and Sustained Attention. 

#### G4[mixed age]
This group(n=282) consists of patients with mixed age i.e. both [22-30] and [31+].
There are 23 clinical features specific to this group with only one variable that is unique :
Executive Function/ Cognitive Flexibility

