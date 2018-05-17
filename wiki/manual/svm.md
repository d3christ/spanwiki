SVM-RFE data analysis (Support Vector Machines with Recursive Feature Elimination; De Martino et al., 2008)

Motivation: Imagine you have whole brain data (temporally resolved or not) and want to classify something (e.g., a future choice within individuals, a diagnosis across individuals), but do not want to assume anything about where or when brain features will classify your variable of interest (half-jokingly called "model-free analysis"). You would need a method that can: 
1) Analyze all the brain features for classification, 
2) Identify the most predictive features for selection, and 
3) Backproject the selected features into space and time for interpretation 
Thanks to the magic of machine learning analyses, such model-free techniques exist! One of them is called "Support Vector Machines with Recursive Feature Elimination (or SVM-RFE). The general goal of SVM-RFE is to maximize classification while minimizing features (which addresses statistical problems in which p>>n). Here, in addition to this overview of the procedure, we provide relevant code for implementing SVM-RFE on whole brain data, and also include a sample write-up for describing results. 

Code: 
TBA

Sample Results (cf. Ferenczi et al., 2016, Science, supplement): 
"Support vector machine classification analysis with recursive feature elimination
To obtain a model-free confirmation of the effects of pharmacological agents on optogenetic
midbrain stimulation, we applied a support vector machine classification analysis with recursive
feature elimination (SVM-RFE; (39) using scikit-learn (92)). Features were defined as activity
values in each voxel (within a brain) at each time point during a trial. These analyses allowed us
to visualize and verify both where and when BOLD activity reliably discriminated between
optogenetic midbrain stimulation versus non-stimulation trials. To find the optimal classification
parameters, we first applied support vector machine classifiers with recursive feature
elimination (5% per iteration until < 1% were remaining) to volume-masked whole brain data for
all acquisitions following stimulation or not (i.e., 26 TRs spanning 13 sec / trial) at varying C-values
(representing the robustness of the separation margin between the two classification
categories) ranging from .0001 to 1000 by orders of magnitude, and then tested classification
performance using "leave one run out" cross-validation. We then visualized which C-value and
number of features provided the best classification of optogenetic midbrain stimulation versus
non-stimulation trials. Next, we reran the SVM-RFE using these peak classification parameters,
and back-projected features that significantly classified midbrain stimulation versus not onto a
four-dimensional representation of brain volumes over time in order to visually verify that
predicted features responded to midbrain stimulation or not at the expected time points (Fig. S6).
Cross-validation at the optimal C-value and back-projection of peak classification features onto
brain volumes were also performed for the pharmacological manipulation datasets and for YFP-control
subjects for direct comparison of classification rates. Using the drug-free dataset, SVMRFE
revealed that 2.5% of the features at a classification parameter of C = 1.0 (where C defines
the robustness of the separation margin between classification categories) best classified
midbrain stimulation versus no stimulation at a peak value of 87% accuracy (cross-validated
across runs and subjects). For visual stimulation, 2.5% of the features at a classification
parameter of C = 0.001 best classified stimulation versus no stimulation at a peak value of 82%
accuracy."

Sample Figures (cf. Ferenczi et al., 2016, Science, supplement): 



Common mistakes (cf. : 
* Not normalizing data within voxels before classification
* Not filtering the data for time-dependent confounds (e.g., using high pass filtering)
* Not warping all the data into the same space (e.g., Talairach space)
* Not initially selecting potentially relevant features (e.g., with a gray matter mask)
* Not using both training and testing samples (with a held-out sample)
* Not sampling classes to have even numbers of cases within subject (down- or up-)
