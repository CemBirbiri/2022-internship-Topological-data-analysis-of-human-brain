# Internship-2022-Topological-Data-Analysis
I finished my internship at [INRIA,France in MathNeuro research group](https://team.inria.fr/mathneuro/) under the supervision of [Prof Mathieu Desroches](https://www-sop.inria.fr/members/Mathieu.Desroches/).

I used [the Human-Connectome dataset](https://wiki.humanconnectome.org/display/PublicData/HCP-YA+Data+Dictionary-+Updated+for+the+1200+Subject+Release#HCPYADataDictionaryUpdatedforthe1200SubjectRelease) that consists of 998 patients with cognition, motor, emotion characteristics as well as brain features: volume, thickness, surface area, gray matter volume, white matter volume,...

I applied Topological Data Analysis (more specifically the Mapper algorithm) to cluster the patients according to their age and found different characteristics of subgroups. I found 2 younger group(age: 22-30), 1 older group(age: 31+) and 1 mixed-age group. A summary of the internship is in below:

# What is Topological Data Analysis (TDA) ?

Data gathering and analyses become a fundamental power in all research communities
such as medicine, engineering, social sciences, economy or mathematics, to name but a
few. The amount of data gathered from different areas grows in a gigantic rate therefore it
is essential to find a scientific methods to analyse them and extract from them meaningful
knowledge. Understanding the shape of data brings us to topology, which is a branch of
mathematics. Topology is the field of mathematics that studies how space is “connected”.
It was first studied by Swiss mathematician Leonhard Euler [1] in the 18th century. Over
the last 20 years, topological techniques have been used in various applied problems, one
of these fields is called Topological Data Analysis (TDA). TDA provides detailed information about the characteristics of the data by providing some topological, statistical and
geometrical methods to infer complex topological structures such as connected components, holes, branches or loops. This analysis method is very robust to outliers and noise
where the data is usually represented as point clouds or distance matrices in a Euclidean
metric space.
