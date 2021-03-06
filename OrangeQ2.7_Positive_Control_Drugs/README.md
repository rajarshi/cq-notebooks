## Orange Team CQ#2.7

### Query: 
What HSCT conditioning agents are seen significantly less often in EHR FA populations?
  
### Goal:
FA patients tolerate certain Hematopoietic Stem Cell Transplant (HCST) pre-conditioning drugs poorly. This set of queries aims to find drugs used to treat HCST, but that are used less often in FA patients. The output of this query might be used as input (i.e., potentially poor-tolerated drugs) for [CQ#2.4](https://github.com/NCATS-Tangerine/cq-notebooks/tree/master/OrangeQ2.4_Drug_Gene_Pathway).
  
### Data Types, Sources, and Routes:
1. **Drugs indicated to treat HCST** - from DrugBank [DrugBank API]?
2. **EHR data** - from Synthetic data, [Synthetic data API]? 
3. **CUI-RxNorm and CUI-ICD connection** - from UMLS, [UMLS API](https://documentation.uts.nlm.nih.gov/rest/home.html)
  
### Sub-Queries/Tasks:
   
**Input:** procedure names in Set 1a and condition name in Set2a, *NOTE: steps can be completed for HCST procedures and Fanconi Anemia*

  1. retrieve **drugs** used to treat HSCT --> Set1b (cpt code | cui)
  2. retrieve **patients** from EHR that have procedure in Set1a and drug prescription in Set1b -> Set2b (EHR id | patient id | cpt code | cui | rxnorm code ) 
  3. retrieve **condition** in 2a from EHR in Set2b -> Set2c (EHR id | patient id | cpt code | cui | rxnorm code | condition yes/no) 
  4. execute **Fisher's exact test** for each drug to compare two independent population proportions (i.e., FA vs non-FA patients) --> Set3 (EHR id | cpt code | cui | rxnorm code | p-value)

**Output:** Set of drugs and their EHR population-specific p-values for the Fisher's exact test.
  
From here drugs used to treat HSCT that are seen significantly less often in FA patients can be used as input for other analyses e.g., [CQ#2.4](https://github.com/NCATS-Tangerine/cq-notebooks/tree/master/OrangeQ2.4_Drug_Gene_Pathway).

--------

*Note that two major assumptions are made with the synthetic dataset: (a) FA is captured with an ICD code, and (b) drug prescriptions for the treatment of HSCT occurs at the same point in time that the procedure is recorded in the EHR.*
