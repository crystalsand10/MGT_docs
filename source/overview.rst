.. _overview:

***********************************
MGT overview
***********************************

MGT stands for multilevel genome typing, a novel technique to stably characterize bacteria (Payne *et al.* 2020). MGT is based on multilocus sequence typing (MLST). MGT provides multiple MLST-like schemes within the one system - thus enabling studying isolates at different levels of relatedness (such as closely related or distantly related). 

Each isolate is assigned a sequence type (ST) at each of the MGT-levels, and the assigned ST at each level is the isolates genome type (GT). For example, an isolate SRR1966720 (as shown in the table in the screenshot below), is assigned an ST at each MGT level such as: 19-9-28-37-39-40-5344-6819-7219 - this is the GT of this isolate (similar to a barcode).

The identifier is assigned based on variations in certain grouped sets of genes (9 in this case, MGT1 to MGT9, hence the 9 numbers in the identifier). The groups are defined based on mutational rates, where the earlier (/smaller) groups depict slower mutational rate changes compared to the later (/larger) ones.

A MGT barcode assigned to a bacterial genome is stable and can be used to uniquely and consistently identify strains with a particular genome type. Isolates which are similar can be identified by the same ST-values at the MGT levels. The same STs have the same colours in the table (thus enabling easy visual identification). Additionally, to identify strains which are closely related, clonal clusters (CCs) or outbreak detection clusters (ODCs) can be used. In this clustering approach, STs which have atmost one allele difference for CCs, from any other STs are grouped together. ODCs are similar to CCs, however, here the maximum number of allele differences are indicated by the number following the ODC. E.g. ODC10 allows a maximum of 10 allele differences.

.. image:: images/sts_v2.png
  :width: 800
  :alt: MGT STs.

This screenshot of MGTdb shows GTs of a few *Salmonella* Typhimurium isolates. 

.. image:: images/ccs_v2.png
  :width: 800

In this figure, the same strains as in the previous figure are shown, but instead of the stable MGT STs, shown are an unstable CCs at each MGT level, and ODCs which are calculated for the largest MGT level. CCs and ODCs enable further relatedness analysis.  


References
----------

Payne M, Kaur S, Wang Q, Hennessy D, Luo L, Octavia S, Tanaka MM, Sintchenko V, Lan R. Multilevel genome typing: genomics-guided scalable resolution typing of microbial pathogens. *Eurosurveillance*. 2020 May 21;25(20):1900519.

