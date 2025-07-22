# AI-Guided-Gene-Discovery
This Repo focuses on improving precision medicine for triple-negative breast cancer (TNBC) through machine learning (ML)-guided gene expression analysis. We aimed to identify key genes (biomarkers) that can be used for diagnosis, prognosis, and potential drug targets in TNBC, a particularly challenging form of cancer. We utilized publicly available genomic data from GEO and TCGA databases, applying differential gene expression algorithms and various ML-based feature selection methods to pinpoint significant genes. 

**Reference**

Ghazal, Hany; El-Absawy, El-Sayed A.; Ead, Waleed; and Hasan, Mohamed E. (2024) "Machine Learning-
Guided Differential Gene Expression Analysis Identifies A Highly-Connected Seven-Gene Cluster in Triple-
Negative Breast Cancer,"
BioMedicine: Vol. 14 : Iss. 4 , Article 2.
DOI: 10.37796/2211-8039.1467

## Wokflow 

The researchers followed a detailed workflow to identify a highly-connected seven-gene cluster in Triple-Negative Breast Cancer (TNBC) and build machine learning (ML) models for precision oncology
. Here's a step-by-step breakdown of their methodology:

• **1. Data Wrangling and Acquisition:**

    ◦ Goal: To gather and prepare transcriptomic data for analysis.

    ◦ Process:

        ▪ The study utilized transcriptomic data from two major repositories: the Gene Expression Omnibus (GEO) of the NCBI and The Cancer Genome Atlas (TCGA).

        ▪ Six GEO microarray datasets (GSE7904, GSE21653, GSE43358, GSE45827, GSE65194, and GSE76275) were initially used for differential gene expression analysis.

        These datasets were selected based on specific inclusion criteria: Homo sapiens organism, cases of TNBC and non-TNBC, primary tumors without treatment, RNA samples for transcriptome analysis, and expression profiling by array study type.

        ▪ TCGA-BRCA project data was also downloaded for a more precise and augmented analysis.

        ▪ Raw “.cel” files from GEO were normalized using the Robust Multi-chip Average (RMA) algorithm to ensure background correction and remove technical variation.

        ▪ Feature data was cleaned by removing probes without Entrez gene identifiers and annotating those with identifiers but missing gene symbols.

        ▪ Initially, the merged data had 1974 samples (915 from GEO and 1059 from TCGA).

 To optimize ML model performance and prevent overfitting, the sample count was extended by including additional GEO datasets (GSE83937, GSE95700, GSE103091, GSE135565, GSE157284, and GSE167213), bringing the total to 2690 samples (1315 TNBC and 1375 non-TNBC samples).

• **2. Identification of Target Groups:**


    ◦ Goal: To define the distinct patient groups for comparison.

    ◦ Process: The phenotype data for each dataset provided information to identify three groups: TNBC, non-TNBC, and healthy groups.
 For the differential expression analysis, healthy samples were dropped, focusing on a binary classification problem between TNBC and non-TNBC samples.

• **3. Differential Expression Analysis (DEA):**

    ◦ Goal: To identify genes that show significant differences in expression levels between TNBC and non-TNBC groups.


    ◦ Process:

        ▪ For GEO microarray data, the LIMMA (Linear Model for Microarray Analysis) algorithm was used.

 This involved constructing a design matrix, making a contrast to represent group differences, calculating array weights to handle outliers, fitting the data with the LIMMA model, and finally applying the eBayes fit to compute moderated statistics for differential gene expression.


        ▪ For TCGA data, the edgeR algorithm was used.


        ▪ Statistical criteria for gene selection were an adjusted P-Value (adj.P.Value) less than 0.01 and a log fold change (logFC) greater than 1.5.
 The Benjamini-Hochberg method was used for p-value adjustment, controlling the False Discovery Rate (FDR).


        ▪ Duplicate gene symbols were removed after filtration to preserve the most informative Differentially Expressed Genes (DEGs).

        ▪ The top 20 DEGs were selected from each of the six initial GEO datasets, and from the TCGA dataset, resulting in 89 unique DEGs.

        ▪ A total of 798 unique genes were identified as common between the GEO LIMMA analysis and TCGA edgeR analysis, which were then prepared for the ML workflow.


• **4. AI-based Feature Selection:**

    ◦ Goal: To optimize ML model performance by selecting the most relevant genes (features) and prevent overfitting.

    ◦ Process:

        ▪ The merged data of 798 unique DEGs from GEO and TCGA, along with the extended samples, was fed into the ML workflow.

        ▪ Data preprocessing involved feature scaling (using the standard scaler method) and label-encoding the categorical target classes (TNBC/non-TNBC).

        ▪ Four ML-based feature selection algorithms were applied:

            • Random Forest Classifier (RFC): An embedded method that calculates feature importance during model training.

            • Recursive Feature Elimination with Cross-Validation (RFECV): A wrapper method that recursively removes the least important features based on an estimator model (here, RFC).

            • Mutual Information Classifier (MIC): A filter method that measures the mutual information gain between each feature and the target variable.

            • Minimal Redundancy Maximal Relevance (MRMR): A filter method that selects features with maximal correlation to the target variable and minimal correlation among themselves.

        ▪ These four models selected varying numbers of features: 206 by RFC, 225 by RFECV, 400 by MRMR, and 400 by MIC.

        ▪ An extended list of all 1231 probes selected by these models was created.
 Features were ranked by their presence percentage across the models.

        ▪ The 92 highest-ranked probes (representing the most relevant genes) were those selected by all four ML models.
        
        ▪ Finally, these 92 ML-selected probes were intersected with the top LIMMA-ranked probes (from the initial 89 unique DEGs) to yield 27 final, highly informative genes.
 Non-informative probes with multiple Entrez identifiers or duplicate gene symbols were removed at this stage.


• **5. AI Classifier Model Building:**
    ◦ Goal: To construct interpretable, high-accuracy AI models for precise diagnosis and patient stratification.

    ◦ Process:

        ▪ The 27 selected genes were used to train eight different ML classifier models for the binary classification task (TNBC vs. non-TNBC):

            • Random Forest Classifier (RFC)
            • Support Vector Machine Classifier (SVC)
            • Gradient Boosting Classifier (GBC)
            • HistGradientBoosting Classifier (HGBC)
            • Extreme Gradient Boosting Classifier (XGBC)
            • CatBoost Classifier (CTBC)
            • AdaBoost Classifier (ADBC)
            • Multi-Layer Perceptron Classifier (MLP)
        ▪ Hyperparameter optimization for each model was performed using the Optuna Python library to enhance performance scores.

• **6. Model Validation:**
    ◦ Goal: To evaluate the performance and reliability of the trained models on unseen data.

    ◦ Process:
        ▪ The optimized models were trained, tested, and evaluated using various performance metrics.

        ▪ They were then validated using an unseen GEO dataset, GSE61724.

        ▪ Performance metrics included accuracy, precision, recall, F1-score, and Area Under the Curve (AUC).

        ▪ CTBC, XGBC, RFC, and MLP showed the highest ROC-AUC values and best performance scores in both testing and validation.

• **7. GO and Reactome Enrichment Analysis:**
    ◦ Goal: To understand the biological functions, processes, and pathways associated with the 27 selected genes.

    ◦ Process:
        ▪ Gene Ontology (GO) and pathway enrichment analysis were performed using the R packages ClusterProfiler and ReactomePA.

        ▪ Enrichment focused on Biological Processes (BP) and Molecular Functions (MF), with no Cellular Component (CC) enrichment observed.

        ▪ Key enriched BP terms included gland development, cell fate commitment, and miRNA transcription.

        ▪ Key enriched MF terms included transcription coactivator binding, dystroglycan binding, steroid binding, and DNA-binding transcription activator activity.

        ▪ Most enriched Reactome pathways included estrogen-dependent gene expression, ESR-mediated signaling, and signaling by nuclear receptors.

• **8. Protein-Protein Interaction (PPI) Network Analysis:**
    ◦ Goal: To identify interacting proteins among the selected genes and find highly connected "hub genes".

    ◦ Process:
        ▪ A PPI network was constructed from the top 27 genes using the STRING database.
 Only 25 genes were mapped (BLACAT1 and LINC00993 were not mapped).

        ▪ The network was visualized in Cytoscape.

        ▪ Clustering and module analysis were performed using the MCODE (Molecular Complex Detection) plugin, which yielded one cluster with 6 nodes (genes) and 14 edges.

        ▪ Hub genes were identified using the Cytohubba plugin by intersecting results from the three most important metrics: Mean Clique Centrality (MCC), node degree, and closeness scores.

        ▪ This analysis identified seven hub genes: ESR1, FOXA1, GATA3, XBP1, GREB1, AR, and AGR2.
 Six of these were intramodular, forming the core of the MCODE cluster, and AGR2 was an extramodular hub gene.

• **9. Survival Analysis of Hub Genes:**
    ◦ Goal: To validate the identified hub genes as potential prognostic biomarkers.

    ◦ Process:
        ▪ The Kaplan-Meier plotter web-based tool was used to measure the correlation between the expression of the seven hub genes and survival in breast tumor samples.

        ▪ The analysis revealed that high expression of ESR1, GATA3, XBP1, GREB1, AR, and AGR2, and low expression of FOXA1 contributed to extended disease-free survival (DFS).

        ▪ This provided evidence that these hub genes can be considered reliable prognostic biomarkers.

This comprehensive workflow, combining traditional differential gene expression analysis with advanced machine learning techniques, allowed the researchers to identify a significant gene cluster for TNBC, with implications for diagnosis, prognosis, and potential drug target development.


