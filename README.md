# local_cohomology_mixed
This software computes the local cohomology for a mixed-module. The underlying theory and algorithm are to be announced at CASC 2024 by Hiroshi Teramoto and Katsusuke Nabeshima (In what follows, we denote this paper as the paper.). We thank Prof. Raúl Oset Sinha and Dr. Ignacio Breva Ribes in University of Valencia for providing us many examples to test the code, helping to improve efficiency and to fix bugs of this code. We also thank the Singular developpers for fixing bugs of Singular. For the definition of the mixed-module, we refer [Comprehensive Standard System for Generalized Mixed Module and its Application to Singularity Theory](https://www.worldscientific.com/doi/abs/10.1142/S0219498824502219?journalCode=jaa) by Hiroshi Teramoto and Katsusuke Nabeshima (In what follows, we denote this paper as TN2023.).

To use the libraries, download all the files and place them into a single directory. You will need Singular installed on your machine. We have prepared four examples to demonstrate the libraries:

* [example_codimension_transverse_fold.exe](https://github.com/hiroshi-teramoto/local_cohomology_mixed/blob/main/example_codimension_transverse_fold.md)
  - See 3.1.1 Example (transverse fold) in the paper
* example_codimension_beak_to_beak.exe
  - See 3.1.2 Example (beak to beak) in the paper
* [example_divergent_diagram_j1.exe](https://github.com/hiroshi-teramoto/local_cohomology_mixed/blob/main/example_divergent_diagram_j1.md)
  - See 4.1.1 Example: Mather's lemma
* [example_complete_transversal.exe](https://github.com/hiroshi-teramoto/local_cohomology_mixed/blob/main/example_complete_transversal.md)
  - See 4.1.2 Example: [complete transversal](https://iopscience.iop.org/article/10.1088/0951-7715/10/1/017) (with the degree filtration for class 3 in Table.~1 in TN2023)
* [example_determinacy_Rieger_test.exe](https://github.com/hiroshi-teramoto/local_cohomology_mixed/blob/main/example_A_determinacy.md)
  - Examples to illustrate the computation of the quotients of Ae-tangent space (coming soom!)

To run the codes, in the directory, type 

```
singular example_codimension_transverse_fold.exe
```

We assume you are familiar with Singular. If not please refer to [the singular manual](https://www.singular.uni-kl.de/Manual/4-3-2/index.htm#SEC_Top). You can download Singular from [GitHub](https://github.com/Singular/Singular) To run the code, compile from the latest source as in the instruction in the GitHub page.

