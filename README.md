# Internship-2022-Topological-Data-Analysis
I finished my internship at [INRIA,France in MathNeuro research group](https://team.inria.fr/mathneuro/) under the supervision of [Prof Mathieu Desroches](https://www-sop.inria.fr/members/Mathieu.Desroches/).

[The Human-Connectome dataset](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) was used in this project that consists of 998 patients with cognition, motor, emotion characteristics as well as brain features: volume, thickness, surface area, gray matter volume, white matter volume,...


### Summary of this work:
The Topological Data Analysis (more specifically the Mapper algorithm) was applied to cluster the patients according to their cognitive and brain features. The Mapper created a mapper complex, where the topological features of this complex represents the different communities in the dataset. A topological feature can be a connected component, a 1D hole/loop, a 2D cavity, or more generally a d-dimensional “void”. The communities are the group of data points, mapped into the topological space by the Mapper algorithm. There are four different communities detected by Mapper such as three of them composed of female patients only and one group of consists only male patients. After community detection, we investigate which features are more representative in different communities. The Kolmogorov-Smirnov (KS) test was applied for each feature to measure the statistical difference between data points for each community. If the p-value of the KS test is less than 0.05 then we assign that feature as a unique and representative feature of that community. For example, in one of the female community the "Negative Affect-Anger" and "Executive Function/Inhibition" features are specific to only this community. In the male subgroup, there are two representative features that appeared only in this subgroup: "Negative Affect - Sadness" and "Social Relationships -Loneliness". Surprisingly, the men in the dataset seem to have issues with sadness and loneliness. More details of the Mapper algorithm, methodology and results can be found in Topological-data-analysis-internship-report.pdf. 



# 1 Introduction

## 1.1 What is Topological Data Analysis?

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

## 1.2 Applications of TDA in Data Science

There have been many promising results in recent years for applying topological and
geometric approaches in different areas. TDA was used in material sciences, shape
analysis of 3D objects, analysis of images in various domains, time series
analysis, medicine, biology, genomic data, or chemistry.
Topological approaches and how to use them in interdisciplinary research topics and data
science is currently a very active research domain.

In the last few years, there has been a great effort to implement robust TDA structures
and algorithms to make them available to the scientific community. These TDA libraries
are publicly available and easy to use for any research field. The most popular ones include
GUDHI (Python and C++), Scikit-TDA, or giotto-tda.
One of the methods in topological analysis of complex data is the Mapper algorithm,
which is very much used in research. The Mapper algorithm uses some metrics and lenses
to convert data into a topological network where nodes represent similar data points and
edges represent topological relations. It is used for visualization of high-dimensional data
and for clustering the data points to extract similarities between communities.

During this internship, the Mapper algorithm was applied to the data that were obtained
from the UCLA multi-modal connectivity database. This dataset has many features
of the human brain such as cognitive behavior, motor skills, or brain images of patients.
My goal was to apply Mapper to this complex dataset to (i) visualize, (ii) find similarities
between data points to extract different communities, and (iii) understand what features
make these communities unique. The outcome of the Mapper algorithm gives a graph
that one can visualize in 2D or 3D. I then applied some statistical tests to find unique
communities with their features in the topological network. Overall, the TDA pipeline can be described through the following steps 


1. Preprocessing of the data.
2. Applying the Mapper algorithm and extracting the low-dimensional shape of data points, which is the visualization result obtained from Mapper.
3. Detecting communities from the result of Mapper.
4. Discovering what features make these communities unique.

# 2 The Dataset and Preprocessing

The dataset belongs to the [NIH Human Connectome Project (HCP)](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) released in 2009
in the Blueprint Grand Challenge. The purpose of the Human Connectome Project
is to discover the structure of the human brain and its functional connectivity. It brings
many advances in neuroscience research by shedding new light onto key questions such
as : how the brain circuitry changes as humans age, how it changes from the psychiatric
and neurological standpoints, and how the electrical signals generate thought, feelings, or
human behavior. To date, there have been over 100 publications that use HCP data since
the first publicly available release. Overall, HCP data helps us to discover what makes
humans unique and leads to great advances in the understanding of the human brain in
particular in link with neurological and psychiatric disorders.

The dataset includes 998 patients with various features such as brain scans (diffusion
tensor images), personal information (family history, personality, age, gender, etc.), cogni-
tive behavior, motor skills, emotion, sensory (vision, taste, pain, audition, olfaction) and
more.

 The following features were selected for this research :
 
• Diffusion Tensor Image (Connectivity matrices)

• Personal information : Age and gender

• Cognition : Episodic memory(Picture Sequence Memory), Executive function :
Cognitive flexibility(Dimensional Change Card Sort) and inhibition (Flanker Task),
Fluid Intelligence(Penn Progressive Matrices), Language/Reading Decoding (Oral
Reading Recognition), Language/Vocabulary Comprehension (Picture Vocabulary),
Processing Speed (Pattern Completion Processing Speed), Self-regulation/Impulsivity
(Delay Discounting), Spatial Orientation (Variable Short Penn Line Orientation
Test), Sustained Attention (Short Penn Continuous Performance Test), Verbal Epi-
sodic Memory (Penn Word Memory Test), Working Memory (List Sorting)

• Emotion : Emotion recognition(Penn Emotion Recognition Test), negative affect,
psychological well-being, social relationships, stress and self efficacy

• Motor : Endurance(2 minute walk test), locomotion (4-meter walk test), dexterity
(9-hole Pegboard), strength (Grip Strength Dynamometry)

## 2.1 Preprocessing
Most of the features in the dataset have scalar data types. Only connectivity matrices
of patients’ brain scans have graph-type data and some personal information (gender and
age) is of string type. The Sklearn categorical encoders was used to convert string type
to scalar.

### Diffusion Tensor Image (Connectivity matrices)
In the dataset there are brain scans for each patient in the form of diffusion tensor
images (DTI). DTI is a popular brain imaging technique that measures white matter
of the brain i.e. the diffusion of water molecules. Each image has an equal
non-symmetric connectivity matrix. 

Connectivity matrices measure the amount of fibers from one region to another region
of the brain. There is one matrix for each patient and each represents a weighted directed
graph. There are 116 brain regions represented in this matrices, therefore the shape of one matrx is (116, 116).  An example of connectivity matrix is shown in Fig1 below.



If we can find a good way to extract features from connectivity matrices, then we
can extract underlying knowledge in the diffusion images and add them to the final da-
taset. There are several ways to extract information from graphs such as : (i) flattening
the matrices and using dimension reduction methods such as, e.g., principal component
analysis (PCA) or singular value decomposition (SVD), (ii) using graph metrics (density,
degree/strength, centrality, etc), (iii) using topological feature extraction methods. Which
methods one selects has a strong influence on the outcome, so the choice must be wise.
At this point, we should analyze our graph data where all graphs are weighted, directed,
and sparse. They are sparse because, in the matrices, most of the entries are 0, which
means there is no direct fiber between these two regions. Sparse graphs would also have
different shapes. Using TDA can be a good idea because topology is the field of analyzing
the shape of objects. We have different shaped graphs in the 116-dimensional matrices,
so using topology again seems appropriate.

A topological feature can be a connected component, a 1D hole/loop, a 2D cavity, or
more generally a d-dimensional “void”. These features are based on the shape of the data.
Firstly, graphs are converted to persistence diagrams via a persistence complex. There
are five complexes in the giotto-tda [18] library such as Vietoris-Rips, Weighted-Rips,
Sparse-Rips, Weak-Alpha, and Euclidean- ˇCech persistence. Since we have weighted, un-
directed and sparse graphs, Sparse-Rips persistence [22] was the most suitable one. After
the complex computation, the graphs turn into persistence diagrams. There is one persis-
tence diagram for each patient in the dataset calculated from the Sparse-Rips complex. Persistence diagrams are representations of topological features. Each point is a topological feature that appears between a birth and a death scale in the diagram. A point’s
distance from the diagonal visually represents how “persistent” the associated topological
feature is.

<img width="1440" alt="Screenshot 2023-07-04 at 15 01 20" src="https://github.com/CemBirbiri/2022-internship-Topological-data-analysis-of-human-brain/assets/46814542/225c2502-8b99-4528-ba1d-785350aac045">




For example, in Fig. 2 topological features are shown using different colors. In
dimension 0, the features are represented in red, and in dimension 1 they are represented
in green. In dimension 0, the diagram illustrates the connectivity structure of the graph.
In dimension 1, it illustrates the independent one-dimensional holes.
It is also possible to convert the information contained in persistence diagrams into scalar
features using Persistence Entropy transformer [23] in giotto-tda. From a practical view-
point, whenever there was a NaN value in a column, I replaced it with the mean value of
the corresponding column. In the end, I have obtained a dataset with 29 features, shaped
in a (998,29) matrix that is shown in Fig. 3 below

<img width="570" alt="Screenshot 2023-07-05 at 22 28 02" src="https://github.com/CemBirbiri/2022-internship-Topological-data-analysis-of-human-brain/assets/46814542/8fb23dd0-a14f-4d55-a74d-027df4de7d92">



#3. The Mapper Algorithm
Mapper is a computational method for complex datasets to extract the simplest in-
formation in the form of simplicial complexes that conserve the underlying topological
structures from the original data. It was first implemented in 2007 [24], and since then
it has become a popular visualization tool in topological data analysis. It is used for the




The key idea of the Mapper algorithm is to map data points into a metric space using
some filters defined on data points while capturing topological and geometric information
at a specified resolution and gain. The filter function (also called a lens) is the metric that
allows to map our data points. An example of Mapper algorithm of a 'hand' dataset is shown below:

<img width="400" alt="Screen Shot 2022-11-07 at 16 32 13" src="https://user-images.githubusercontent.com/46814542/200349905-b2812411-7254-4ad0-9302-8bd35ea1aa16.png">

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
7.Total cortical gray matter volume(1)
8.Total subcortical gray matter volume(1)
9.Total gray matter volume(1)
10. Supratentorial volume(1)
11. Total cortical white matter volume(1)
12. Topological features(2) extracted from connectivity matrices of patients


## RESULTS

The mapper result is shown below:

<img width="450" alt="Screen Shot 2022-11-07 at 16 41 15" src="https://user-images.githubusercontent.com/46814542/200352096-1167d846-2c1f-4845-b22f-c1f3b76ee075.png">


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

We found following results that are parallel with literature:
1. Total and subcortical gray matter volume decrease with age
2. Cognitive performance of humans decrease with age (Working Memory, Language/Vocabulary comprehension, Fluid Intelligence, Executive Function/Cognitive Flexibility)
3. Surface Area in Orbito-frontal Cortices decrease when people get old.
4. Lateral-frontal Cortical thickness decrease with age
5. Total Cortical White Matter Volume decrease with age
6. Supratentorial volume reduces with age
7. Rostral-anterior-cingulate Avg Thickness decrease with age(BONUS)





