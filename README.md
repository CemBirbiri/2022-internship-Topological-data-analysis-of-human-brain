# Internship-2022-Topological-Data-Analysis
I finished my internship at [INRIA,France in MathNeuro research group](https://team.inria.fr/mathneuro/) under the supervision of [Prof Mathieu Desroches](https://www-sop.inria.fr/members/Mathieu.Desroches/). 

[The Human-Connectome dataset](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) was used in this project that consists of 998 patients with cognition, motor, emotion characteristics as well as brain features: volume, thickness, surface area, gray matter volume, white matter volume, etc.


### Summary of this work:
The Topological Data Analysis (more specifically the Mapper algorithm) was applied to cluster the patients according to their cognitive and brain features. The Mapper created a mapper complex, where the topological features of this complex represents the different communities in the dataset. A topological feature can be a connected component, a 1D hole/loop, a 2D cavity, or more generally a d-dimensional “void”. The communities are the group of data points, mapped into the topological space by the Mapper algorithm. There are four different communities detected by Mapper such as three of them composed of female patients only and one group of consists only male patients. After community detection, we investigate which features are more representative in different communities. The Kolmogorov-Smirnov (KS) test was applied for each feature to measure the statistical difference between data points for each community. If the p-value of the KS test is less than 0.05 then we assign that feature as a unique and representative feature of that community. For example, in one of the female community the "Negative Affect-Anger" and "Executive Function/Inhibition" features are specific to only this community. In the male subgroup, there are two representative features that appeared only in this subgroup: "Negative Affect - Sadness" and "Social Relationships -Loneliness". Surprisingly, the men in the dataset seem to have issues with sadness and loneliness. 

More details of the Mapper algorithm, methodology and results can be found below, or in TDA-internship-2022.pdf. The Python implementation of this project is in GUDHI_Notebook.ipynb






# 1. Introduction



## 1.1 What is Topological Data Analysis (TDA)?
Data gathering and analyses become a fundamental power in all research communities such as medicine, engineering, social sciences, economy, and mathematics, to name but a few. The amount of data gathered from different areas grows at a gigantic rate therefore it is essential to find a scientific method to analyse them and extract from them meaningful knowledge. Understanding the \textit{shape of data} brings us to topology, which is a branch of mathematics. Topology is the field of mathematics that studies how space is ``connected". It was first studied by the Swiss mathematician [Leonhard Euler](https://www.maa.org/press/periodicals/convergence/leonard-eulers-solution-to-the-konigsberg-bridge-problem) in the 18th century. Over the last 20 years, topological techniques have been used in various applied problems, one of these fields is called Topological Data Analysis (TDA). TDA provides detailed information about the characteristics of the data by providing some topological, statistical, and geometrical methods to infer complex topological structures such as connected components, holes, branches, or loops. This analysis method is very robust to outliers and noise where the data is usually represented as point clouds or distance matrices in a Euclidean metric space.

## 1.2 Applications of Topological Data Analysis in Data Science
There have been many promising results in recent years for applying topological and
geometric approaches in various research fields. TDA was used in [material sciences](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.87.042207), [shape analysis of 3D objects](https://www.arxiv-vanity.com/papers/1905.12200/), [analysis of images in various domains](https://papers.nips.cc/paper/2020/hash/4d771504ddcd28037b4199740df767e6-Abstract.html), [time series analysis](https://www.firaskhasawneh.com/assets/files/publications/Khasawneh2016.pdf), [medicine](https://inria.hal.science/hal-02155849), [biology](https://link.springer.com/chapter/10.1007/978-3-030-47358-7_17),  [genomic data](https://journals.aps.org/prd/abstract/10.1103/PhysRevD.80.034502), or [chemistry](https://www.nature.com/articles/ncomms15396). Topological approaches and how to use them in interdisciplinary research topics and data science is currently a very active research domain.


One of the methods in topological analysis of complex data is the Mapper algorithm. The Mapper algorithm uses some metrics and lenses to convert data into a topological network where nodes represent similar data points and edges represent topological relations. It is used for the visualization of high-dimensional data and for clustering the data points to extract similarities between subgroups.

The Mapper algorithm is applied to the data that were obtained from the [The Human-Connectome dataset](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease). This dataset has many features of the human brain such as cognitive behavior, motor skills, or brain images of patients. The goal was to apply Mapper to this complex dataset to (i) visualize, (ii) find similarities between data points to extract different communities, and (iii) understand what features make these communities unique. The outcome of the Mapper algorithm gives a graph that one can visualize in 2D or 3D. After achieving the Mapper result, the Kolmogorov-Smirnov (KS) test was applied to find unique communities with their features in the topological network. The overall TDA pipeline can be described through the following steps:

-  Preprocessing of the data.
-  Applying the Mapper algorithm and extracting the low-dimensional shape of data points, called the mapper complex.
-  Detecting different subgroups from the result of Mapper.
-  Discovering what features make these communities unique.

The following sections include details about the dataset, methodology, and results.



# 2. Dataset and Preprocessing


The dataset belongs to the [NIH Human Connectome Project (HCP)](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) released in 2009 in the Blueprint Grand Challenge. The purpose of the Human Connectome Project is to discover the structure of the human brain and its functional connectivity. It brings many advances in neuroscience research by shedding new light onto key questions such as: how the brain circuitry changes as humans age, how it changes from the psychiatric and neurological standpoints, and how the electrical signals generate thought, feelings, or human behavior. To date, there have been over 100 publications that use HCP data since the first publicly available release. Overall, HCP data helps us to discover what makes humans unique and leads to great advances in the understanding of the human brain\, in particular in link with neurological and psychiatric disorders.

The dataset includes 998 patients with various features such as brain scans (diffusion tensor images), personal information (family history, personality, age, gender, etc.), cognitive behavior, motor skills, emotion, sensory (vision, taste, pain, audition, olfaction) and clinical brain characteristics such as gray matter volume, surface area, thickness, etc. 
- Diffusion Tensor Image (Connectivity matrices)}
- Personal information: Age and gender
- Cognitive features:

Episodic memory(Picture Sequence Memory), Executive function: Cognitive flexibility(Dimensional Change Card Sort) and inhibition (Flanker Task), Fluid Intelligence(Penn Progressive Matrices), Language/Reading Decoding (Oral Reading Recognition), Language/Vocabulary Comprehension (Picture Vocabulary), Processing Speed (Pattern Completion Processing Speed), Self-regulation/Impulsivity (Delay Discounting), Spatial Orientation (Variable Short Penn Line Orientation Test), Sustained Attention (Short Penn Continuous Performance Test), Verbal Episodic Memory (Penn Word Memory Test), Working Memory (List Sorting)

- Emotion: Emotion recognition(Penn Emotion Recognition Test), negative affect, psychological well-being, social relationships, stress, and self-efficacy
- Motor: Endurance(2-minute walk test), locomotion (4-meter walk test), dexterity (9-hole Pegboard), strength (Grip Strength Dynamometry)

  

## 2.1 Preprocessing
Most of the features in the dataset have scalar data types. Only connectivity matrices of brain scans have graph-type data and some personal information (gender and age) is of string type. [The Sklearn categorical encoders](https://contrib.scikit-learn.org/category_encoders/) was used to convert string type to scalar. 

### Diffusion Tensor Image (Connectivity matrices)
In the dataset, there are brain scans for each patient in the form of diffusion tensor images (DTI). DTI is a popular brain imaging technique that measures the white matter of the brain i.e. the diffusion of water molecules. The DT images were converted to connectivity matrices for each patient. 

The shape of one connectivity matrix is (116, 116) meaning that there are 116 brain regions. The rows and columns represent brain regions, and the values represent interactions-number of fibers in DTI-  between different regions. An example of a connectivity matrix is shown in Fig.1 below.

<img width="566" alt="Screenshot 2023-07-06 at 13 38 13" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/4ff14a14-393e-4ecd-9fb5-058bfba8e670">

 There are several ways to extract information from connectivity matrices such as: (i) flattening the matrices and using dimension reduction methods such as, e.g., principal component analysis (PCA) or singular value decomposition (SVD), (ii) using graph metrics since a connectivity matrix can be represented as a graph(density, degree/strength, centrality, etc), (iii) using topological feature extraction methods. Since this research includes topological data analysis, the third method will be selected to extract features. 

A topological feature can be a connected component, a community hole/loop, a 2D cavity, or more generally a d-dimensional “void”. These features are based on the shape of the data. Firstly, the connectivity matrices are converted to Networkx graphs, then to persistence diagrams via a persistence complex. There are five complexes in [the giotto-tda](https://giotto-ai.github.io/gtda-docs/0.5.1/library.html) library such as Vietoris-Rips, Weighted-Rips, Sparse-Rips, Weak-Alpha, and Euclidean-Cech persistence. Since we have weighted, undirected, and sparse graphs, [Sparse-Rips persistence](https://giotto-ai.github.io/gtda-docs/latest/modules/generated/homology/gtda.homology.SparseRipsPersistence.html) was the most suitable one. After the complex computation, the graphs turn into persistence diagrams. There is one persistence diagram for each patient in the dataset calculated from the Sparse-Rips complex.

Persistence diagrams are representations of topological features. Each point is a topological feature that appears between a birth and a death scale in the diagram. A point's distance from the diagonal visually represents how “persistent” the associated topological feature is. In Fig.2 topological features are shown using different colors. 

<img width="575" alt="Screenshot 2023-07-06 at 13 39 46" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/9d372ba6-4b19-4b32-9efa-68da12071c2b">


In dimension 0, the features are represented in red, and in dimension 1 they are represented in green. In dimension 0, the diagram illustrates the connectivity structure of the graph. In dimension 1, it illustrates the independent one-dimensional holes.
From the persistence diagrams, we can extract scalar topological features using [the Persistence Entropy transformer](https://giotto-ai.github.io/gtda-docs/latest/modules/generated/diagrams/features/gtda.diagrams.PersistenceEntropy.html) in the giotto-tda library. After this translation, two scalar topological features were obtained(in dimensions 0 and 1) and added to the dataset.

In the end, the final dataset contains features including the sex, and gender of the patients, as well as cognitive, emotional, and motor test scores and scalar topological connectivity matrix features. The final shape of the dataset is (998,29) which is shown in Fig.3.

<img width="567" alt="Screenshot 2023-07-06 at 13 41 09" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/a7a76cea-a74d-4de6-8596-45e1bfa747f4">


# 3. The Mapper Algorithm

Mapper is a computational method for complex datasets to extract the simplest information in the form of simplicial complexes that conserve the underlying topological structures from the original data. [It was first implemented in 2007](https://diglib.eg.org/handle/10.2312/SPBG.SPBG07.091-100), and since then it has become a popular visualization tool in topological data analysis. It is used for the visualization of high-dimensional datasets, simplification, and qualitative analysis. The idea is to partially cluster the data that is guided by scalar filter functions. The resulting visualization is a simple collapse of data into a low-dimensional graph, where the filter function (lens) acts as guidance. The Mapper algorithm is used from [the GUDHI library](https://gudhi.inria.fr/).

The Mapper has various steps, which can be seen in Fig.4 and that are explained in the following sections.\\

 <img width="528" alt="Screenshot 2023-07-06 at 13 43 52" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/be09b963-0e57-4aba-8313-6f217ce282b7">

## 3.1 Selecting the filter function
 The key idea of the Mapper algorithm is to map data points into a metric space using some filters defined on data points while capturing topological and geometric information at a specified resolution and gain. The filter function (also called a lens) is the metric that allows us to map our data points. It should be a scalar vector whose size equals the number of data points in the dataset. For example, in our case, the shape of the final dataset is (998,29), so the size of one filter function must be (998,1). Filter functions are calculated based on the dataset. Each entry in a filter vector represents the cover of an individual data point. Each row in a dataset is converted to a single number, i.e., each row provides a real number in the filter vector.

 Filter selection is a key point of the Mapper algorithm. In Fig.5, three different filters are applied to the same point cloud data such as vertical, horizontal, and radial filters. Although the point cloud, in this case,  is small with only 2 dimensions, this is a good example of how filters could change the Mapper result. Filter choice depends on the data, the complexity of the data, and the goal of the Mapper user. A different filter function
will generate a graph with a different shape, thus filters allow one to explore the data from different mathematical perspectives. 

 <img width="572" alt="Screenshot 2023-07-06 at 13 44 33" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/19668dbb-aae3-454f-beec-3a2f225b3fab">


Here is a list of filter functions that I used in my internship work:
- **A column of the dataset.**  If we want to visualize the data according to a column in the data we can choose that column as a filter. Using a specific column as a filter causes the layout to separate the variables of that column. For example, if one considers a dataset of patients that have different types of breast cancer. These types are represented in the dataset in a specific column. The goal is to visualize the data based on different cancer types. [Then, that specific column can be used to separate the data points based on it](https://www.nature.com/articles/srep01236). Another example is that the gender information of patients can be used as a filter. If there are two genders in the data, using the gender column will put the data points into two different groups. In this work, the gender column is used as the first filter.

- **$L_p$ Norm:** The filter function can be chosen based on the $L_p$ norm for each data point. Below, $f_{p,k}$ represents the filter function, $\vec{V}$ denotes the coordinates of each row vector: $\vec{V} = <f_1,f_2,...,f_s>$ where $f_i$ represents the $i$th feature(column) and $s$ is total number of features in the dataset. The $L_p$ norm of an individual patient can be calculated as follows:

<img width="195" alt="Screenshot 2023-07-06 at 13 47 12" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/b75a82bc-b046-46e2-b7c5-49fe5b9c537c">

Note that when $p = 2$ and $k=1$, then the above formula corresponds to the $L_2$ (i.e., Euclidean) norm of a row vector. 

- **L-infinity centrality:** for each data point $y$, it amounts to finding the maximum distance from $y$ to any other point in the dataset. It assigns each data point to the furthest distance from itself to another data point. Large values of this filter put points that are far away from the center of the dataset. The $L$-infinity centrality can be implemented with a few lines of code, as shown in Fig.6 below.

<img width="518" alt="Screenshot 2023-07-06 at 13 48 46" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/0d4402bd-bc34-4c38-a0c3-dc9af9cc1cc3">

- **Singular Value Decomposition (SVD):** SVD is a linear dimension reduction technique. Unlike PCA, it does not center the data points before the actual computation, meaning that it efficiently works with sparse data. In the present work, I have used the Truncated SVD function from Sklearn. Here, the important part is that one uses the output vector of the Truncated SVD function after using "fit_transform" as the filter vector.
- 
- **Principal Component Analysis (PCA):** PCA is a well-known dimension reduction technique. The first component of the PCA result is used as a filter. I have used the PCA function from Sklearn.


The choice of the filter is an important factor for achieving a good result in Mapper. Instead of using one filter, one can use more filters to separate the data better. Applying multiple filters is a popular choice, for instance, in [Identification of type 2 diabetes subgroups through topological analysis of patient similarity](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4780757/) paper, the authors have used the $L$-infinity centrality as well as principal metric singular value decomposition (SVD1) and managed to identify a new subgroup of type-2 diabetes. The gender column and $L$-infinity centrality were used as the first and second filters in our Mapper implementation.


## 3.2 Selecting gain and resolution parameters

Resolution refers to the number of intervals required to cover each filter image; the output is a natural number. If one increases the resolution, then the number of communities obtained through the Mapper result increases as well. 

The gain is the overlap percentage of the intervals covering each filter image. The range of the gain parameter is $[0,1]$; it is a real number. If one increases the gain, then the connection between communities increases too, and, as a result, the number of edges between nodes in the Mapper result increases. Users should decide about these two parameters carefully to obtain an efficient topological representation of their data. For each filter, one gain and one resolution value should be chosen. 


## 3.3 Selecting clustering algorithm
 
 After mapping the data points into corresponding intervals, the data points get clustered in each interval separately. For this phase, any clustering algorithm can be used. To obtain good results, both the DBSCAN and the Agglomerative clustering methods were applied from Sklearn. 
 
 
## 3.4 Resulted Mapper visualization

Finally, a simplicial complex is created and it looks like a graph (see Fig.4, right panel). The vertices/nodes of the graph represent the cluster sets. The edges represent connectivity information of the complex where an edge is added between two nodes whenever two clusters share the same data points.

Coloring the nodes makes the simplicial complex more understandable. The color of a vertex shows the value of the filter function. For instance, in Fig.7, yellow corresponds to high values and blue to low values. More details about the parameters of the Mapper algorithm and how to choose them wisely are given in the following sections.

A very good visualization output can be achieved by tuning parameters of the Mapper algorithm, such as filters, gain, resolution, clustering algorithm, and hyper-parameters inside the clustering algorithm. To obtain the results presented in this report, the following parameters were chosen:



-  Gender column as the first filter, 
-  $L$-infinity centrality as the second filter, 
-  Resolution of gender column filter: 2, 
-  Gain of gender column filter: 0.01, 
-  Resolution of the $L$-infinity centrality filter: 19, 
-  Gain of the $L$-infinity filter: 0.02, 
-  Clustering algorithm: DBSCAN with metric correlation, and min_samples parameter is 3. The other parameters stayed default.


With this setup, the Mapper complex was obtained which is shown in Fig.7. Firstly, using the gender column as the first filter divided the data points into two groups according to the patient's genders. Then, the $L$-infinity centrality filter helped us to find subgroups of patients by placing them far away from the center.

<img width="543" alt="Screenshot 2023-07-06 at 13 55 00" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/fb83575c-8100-4cc0-971d-d910c0871941">







# 4. Community Detection
Community detection is the process where unique groups are found from the complete result of the Mapper algorithm.  What makes these groups unique is that they share different characteristics. First, we find the different communities in the Mapper complex, and then we use some statistical tests to identify what unique features represent each community. 

## 4.1 Finding communities
Finding different communities from the Mapper result corresponds to finding the topological features of the simplicial complex. Up to this point, the Mapper algorithm has converted our data into a simplicial complex which is called the Mapper Complex. Topological features of the Mapper complex represent $0$- or $1$-dimensional topological features such as connected components, up/down branches, and loops. Therefore, the topological features of the Mapper complex represent the different communities. So if we extract topological features from the Mapper complex, we can obtain these communities.

 The GUDHI library was used to extract topological features from the mapper complex. Connected components and loops are computed with [SciPy functions](https://scipy.org/), and branches are detected with ``Union-Find'' and $0$-dimensional persistence of the 1-skeleton. Each shape in Fig.8 below shows one community colored in yellow.

<img width="600" alt="Screenshot 2023-07-06 at 13 57 10" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/ceb1f536-8c7c-4f05-a1b0-f9d663cb4a3e">


## 4.2 Finding unique features of communities
 Once the different communities contained in the data have been identified, following the procedure explained in the previous section, one finds which features are special in each community. To do so, one computes the coordinates that best explain a set of nodes compared to the rest of the nodes with a Kolmogorov-Smirnov (KS) test. For each community, all the features in the dataset are considered in the KS test.
 
 If $\vec{V}$ denotes the row vector of each patient, then the coordinates of each row vector are given by: $\vec{V} = <f_1,f_2,...,f_s>$ where $f_i$ represents the $i$th feature (column) and $s$ is the total number of features/columns in the dataset. If one assumes that $C_1$, $C_2$ ,..., and $C_t$ are communities, then each represents a different topological feature of the Mapper complex. 

Suppose we want to find traits that uniquely define $C_1$. We write 

<img width="178" alt="Screenshot 2023-07-06 at 13 57 54" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/0853b2af-ab95-4853-851d-aa57460810c1">


 where $p_k$ is the $k$th data point that creates the first community, $C_1 $ Then, to find the traits of $C_1$ we proceed as follows.

First, we divide the dataset into two groups. Data points in the first group form $C_1$, and the rest of the data points except $C_1$ form another group. Then, for each feature in the dataset, we examine two different distributions. The first distribution has feature values of $C_1$, the second distribution has the feature values of the rest. Recall that these feature values belong to just one feature(column) of the whole dataset. Then, these two distributions are tested using a Kolmogorov-Smirnov(KS) test. The output of the KS test is a $p$-value that shows the similarity between the two distributions. A larger $p$-value indicates that the two distributions are statistically similar. If a $p$-value of a feature is smaller than 0.05, then that feature is a unique characteristic of $C_1$ and we can say that feature represents that community.

From a practical viewpoint, I have computed $p$-values for each feature in one community and I only considered $p$-values less than $0.05$. Then, I sorted them in ascending order in a list, the first element of the list representing the most representative feature of that community. 

The pseudo-code of how to find unique features of $C_1$ is written below:

<img width="569" alt="Screenshot 2023-07-06 at 13 58 26" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/502841b1-f14a-450b-9c14-1a34c26b6406">


In this algorithm, $KS$ denotes the Kolmogorov-Smirnov test.  I used the two-sample Kolmogorov-Smirnov test from the [Scipy-stats library](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ks_2samp.html) to test the importance of a feature. The above algorithm is done for each community < $C_1$ , $C_2$ , ..., $C_t$ >. In the end, I calculated statistically important features for each community (with $p$-value less than $0.05$).
 
# 5. Results
Each community in the Mapper complex represents a different group in the data. There are four different communities detected by Mapper which are shown in Fig.8. We see three different groups composed of female patients only and one group of only male patients. The genders among communities are heterogeneous since the gender column is used as the first filter function for Mapper. Female groups are named as $F_1$ (n=205), $F_2$ (n=204), $F_3$ (n=77) and the male group is called $M_1$ (n=205). We have analyzed the cognitive, emotional, and motor skills in each group, and identified which features make these groups different. Clinical features specific to groups are listed below where $p$-values are in ascending order from top to bottom i.e. first row in the tables is the most representative feature of that community.




<img width="489" alt="Screenshot 2023-07-06 at 14 09 15" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/85b149e1-47e1-4392-9b61-d0df54360b59">




#### Group $F_1$
Three variables are specific to this group: (i) Language/vocabulary comprehension, (ii) fluid intelligence, and (iii) language/reading decoding. Language/vocabulary comprehension tests the ability of a person to correctly match words with images. In the test, an audio record of a word is given to the person and she/he picks an image out of four images that are most related to the word. The mean value for participants in $F_1$ in this test is $105.52$. Other groups' average scores are $108.86$, $106.01$, and $105.39$, respectively. The mean values are similar but this feature becomes a discriminative property for $F_1$.

Fluid intelligence aims to capture how successful a given person is at thinking and finding reasons in an abstract way and solving problems. This female group was found to be better in this test compared to other patients. Language/reading decoding measures the ability of the patient to accurately read and pronounce the words in both English and Spanish. The $F_1$ group has a disjunctive characteristic of this feature with a mean value of $105.5$. No feature only defines female groups but not males.

----------------------------------------------------------------------
----------------------------------------------------------------------
<img width="499" alt="Screenshot 2023-07-06 at 14 11 01" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/c4043e0c-27f4-4e94-9e14-d8bd50d7663b">



#### Group $M_1$
Interestingly, we have one male group in the resulting communities. Men in this group are linked with two specific emotional categories: Sadness and loneliness. These features do not appear in female groups. Surprisingly, men in the dataset seem to have issues with sadness and loneliness. It makes sense that loneliness appears with sadness because they are related emotions. Patients in $M_1$ share the verbal episodic memory with patients in $F_2$, and spatial orientation feature with patients in $F_1$.

----------------------------------------------------------------------
----------------------------------------------------------------------

<img width="487" alt="Screenshot 2023-07-06 at 14 11 43" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/3b245e1d-3349-4313-8a71-9faf0fdcd615">


#### Group $F_2$
This female group is detected by the variables "Negative Affect-Anger" and "Executive Function/ Inhibition". Anger is described as the attitude of a person about cynicism, hostility, and frustrating experiences. People in $F_2$ are linked to anger problems. The other feature that is specific to $F_2$ is "executive function/inhibition". Participants' attention and inhibitory control are tested. Our results indicate that the $F_2$ group is characterized by attention skills.

----------------------------------------------------------------------
----------------------------------------------------------------------
<img width="494" alt="Screenshot 2023-07-06 at 14 13 22" src="https://github.com/CemBirbiri/Topological-data-analysis-of-human-brain-2022-internship/assets/46814542/393d9b38-315f-49ac-aab6-7316b9e27d1a">



#### Group $F_3$

This group has no specific feature that only belongs to itself. Besides the common features of the four groups, $F_3$ is linked to stress and self-efficacy. However, this variable is also shared by $F_1$ and $M_1$. I interpreted this group as an average female group for which there is no diverse trait.

----------------------------------------------------------------------
----------------------------------------------------------------------
Among the four groups, gender and strength (grip strength dynamometry) are the first two common features that define these communities with the smallest $p$-values ($<0.05$). The grip strength dynamometry test is adopted from the American Society of Hand Therapy, where the participants sit on a chair, bend their elbows at $90$ degrees, and are asked to squeeze to dynamometer as hard as they can with their right and left hands. The result of the test provides a digital score of force in pounds. This motor skill shows how strong the person is. The average value of strength in the women groups is $92.31$, $87.54$, and $84.23$ in $F_1$, $F_2$ and $F_3$, respectively. The average strength in the male group is larger than in the female groups, with a value of $122.28$. In general, physical power is greater in men than women, which is why strength became a key feature to represent these groups.

There are also three other features shared by all of the groups, namely dexterity (9-hole Pegboard), endurance (2 minutes walk test), and episodic memory. Dexterity is a test where participants are asked to put and remove 9 plastic pegs into a pegboard with each hand. The score is the time taken by the participants (3-85 years old) to finish the task. An endurance test is used to test the sub-maximal cardiovascular endurance. Participants walk a 50-foot long lane (back and forth) in 2 minutes and the score is the distance they were able to achieve. On the other hand, episodic memory measures how well one can remember previous experiences within the context of location, time, or emotions. Participants are asked to recall the sequence of objects that are in a specific order shown on the computer screen. If they can remember correctly the order of two objects they get one point. All these three tests resulted to be common features to represent the four groups. Here, it is normal to have common features among groups, yet the important part of TDA analysis is to find specific characteristics that are not shared.









# 6. Conclusion
In this internship, The HCB dataset was used which includes mostly cognitive, motor, and emotional characteristics. I have used two topological data analysis methods to analyze these data. The first one is using the Sparse-Rips complex to convert brain connectivity matrices of patients into scalar features. The second method is the Mapper algorithm. Mapper converts patients' data into a Mapper complex with some filter functions, gain, and resolution parameters. Filters are scalar vectors that are used to map the data points and divide them into different communities. There are many filter options such as $L_p$ norm, $L$-infinity centrality, first or second components of SVD, or just a column in the data. I have used the gender column and the $L$-infinity centrality as filter functions. Topological features of the Mapper complex have resulted in the identification of the different communities. It is a hard task to tune the parameters in the Mapper to have a meaningful result. I have used the GUDHI library for the topological functions. After finding the communities, I used Kolmogorov-Smirnov(KS) test to statistically prove representative features in each community. If the $p$-value of the KS test was less than $0.05$ for a feature, I have considered it as a specific variable to that community. Then I analyzed different groups and identified the specific characteristics of each group. The male group turned out to be linked with loneliness and sadness, while female groups seemed more linked to cognitive skills.



