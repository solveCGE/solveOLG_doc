# solveOLG model documentation
Short documentation of the AK-OLG-model solved in `solveCGE/solveOLG_...` codes.

Compile solveOLG_doc.qmd as follows
```
quarto render solveOLG_doc.qmd
```
## Implementations

- in pure R: [solveOLG_closed_R](https://github.com/solveCGE/solveOLG_closed_R)
- in pure R (without income effects): [solveOLG_closed_noinceff](https://github.com/solveCGE/solveOLG_closed_noinceff)
- in Matlab: [solveOLG_closed_Matlab](https://github.com/solveCGE/solveOLG_closed_Matlab)
- in Python (using `numpy`): [solveOLG_closed_Python](https://github.com/solveCGE/solveOLG_closed_Python)
- in Julia (using global scope): [solveOLG_closed_Julia](https://github.com/solveCGE/solveOLG_closed_Julia)
- in Julia (passing data as structs): [solveOLG_closed_Julia_v2](https://github.com/solveCGE/solveOLG_closed_Julia_v2)
- in R/C++ (using `RcppArmadillo`, nesting different specifications): [solveOLG_closed_Rcpp](https://github.com/solveCGE/solveOLG_closed_Rcpp) and [pkgsolveOLG](https://github.com/solveCGE/pkgsolveOLG)


## Performance comparison of different implementations

Execution time of the tatonnement algorithm (i.e. solving for the transition path) of the 'closed economy with income effects'-case (default shock) run on an Intel(R) Xeon(R) Platinum 8180 CPU @ 2.50GHz:

| implementation       | threads | time in secs  |
| :------ | ------: | -----: |
| Rcpp           | 40      | 0.26  |
| Rcpp           | 20      | 0.35  |
| Rcpp           | 4       | 0.67  |
| Rcpp           | 1       | 1.67  |
| Julia_v2       | 40      | 1.68  |
| Julia_v2       | 20      | 1.72  |
| Julia          | 40      | 1.91  |
| Julia          | 20      | 2.10  |
| Julia_v2       | 4       | 2.67  |
| Julia          | 4       | 3.98  |
| Julia_v2       | 1       | 4.19  |
| Julia          | 1       | 6.89  |
| Matlab         | 1       | 11.49 |
| R              | 1       | 13.99 |
| Python         | 1       | 25.91 |

*Note: Time is median time of 5 consecutive runs. The Rcpp solution in the table used fixed dimension vectors/matrices and all variables in global scope. Other implementations (e.g. non-fixed dimensions or passing struct references instead of using global scope) had no significant effect on execution time. The Matlab implementation was run run on a different CPU. Time was then rescaled using the relative difference of the results of the R implementation on the two different architectures.*

## Author
Philip Schuster

