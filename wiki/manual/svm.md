Analyzing your data with a support vector machine with recursive feature elimination (SVM-RFE; De Martino et al., 2008)

Motivation: Imagine you have whole brain data (temporally resolved or not) and want to classify something (e.g., a future choice within individuals, a diagnosis across individuals), but do not want to rely on any assumptions about where or when brain features will classify your variable of interest (we have been calling this "model-free" analysis). You would need a method that can: 
1) Analyze all the brain features for classification, 
2) Identify the most predictive features for selection, and 
3) Backproject the selected features into space and time for interpretation. 
Thanks to the magic of machine learning analyses, such techniques exist, one of which is called "Support Vector Machines with Recursive Feature Elimination (or SVM-RFE)! The general goal is to maximize classification while minimizing features (which deals with the statistical problem of p>>n). This page provides an overview of the procedure, provides some relevant code for implementing it on whole brain data, and also includes a sample write-up for describing the results. 

Code: 


Results description (from the supplement of Ferenczi et al., 2016): 


Common mistakes: 
* Not normalizing data within voxels before classification
* Not filtering the data for time-dependent confounds (e.g., using high pass filtering)
* Not warping all the data into the same space (e.g., Talairach space)
* Not initially selecting potentially relevant features (e.g., with a gray matter mask)
* Not using both training and testing samples (with a held-out sample)
* Not sampling classes to have even numbers of cases within subject (down- or up-)
