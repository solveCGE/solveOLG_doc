# solveOLG model documentation
Short documentation of the AK-OLG-model solved in `solveCGE/solveOLG_...` codes.

Compile solveOLG_doc.qmd as follows
```
quarto render solveOLG_doc.qmd
```
## Implementations

- in pure R: [solveOLG_closed_R](https://github.com/solveCGE/solveOLG_closed_R)
- in pure R (without income effects): [solveOLG_closed_noinceff](https://github.com/solveCGE/solveOLG_closed_noinceff)
- in pure R (without income effects, GFT algorithm): [solveOLG_GFT_closed_noinceff](https://github.com/solveCGE/solveOLG_GFT_closed_noinceff)
- in Matlab: [solveOLG_closed_Matlab](https://github.com/solveCGE/solveOLG_closed_Matlab)
- in Python (using `numpy`): [solveOLG_closed_Python](https://github.com/solveCGE/solveOLG_closed_Python)
- in Python (using `numpy` and `numba`): [solve_OLG_closed_Python_v2](https://github.com/solveCGE/solveOLG_closed_Python_v2)
- in Julia (using global scope): [solveOLG_closed_Julia](https://github.com/solveCGE/solveOLG_closed_Julia)
- in Julia (passing data as structs): [solveOLG_closed_Julia_v2](https://github.com/solveCGE/solveOLG_closed_Julia_v2)
- in Julia (passing cohort data as slices): [solveOLG_closed_Julia_v3](https://github.com/solveCGE/solveOLG_closed_Julia_v3)
- in R/C++ (using `RcppArmadillo`, nesting different specifications): [solveOLG_closed_Rcpp](https://github.com/solveCGE/solveOLG_closed_Rcpp) and [pkgsolveOLG](https://github.com/solveCGE/pkgsolveOLG)

## Performance comparison of different implementations

In the following I compare runtimes of implementations along different dimensions:
- implementing language: R, R/C++, Julia, Python, Matlab
- model choice: income effects vs. no income effects of labor supply (the latter implies a much more trivial household problem)
- algorithm: tatonnement (i.e. iteration over prices) vs. Generalized Fair-Taylor (GFT, i.e. iteration over agents' expectations)
- scoping: all variables in global scope vs. collecting all data into a structure that is passed between functions vs. passing individual cohort variables explicitly between functions for the household problem
- number of threads: 1, 4 or 20 (not all implementations can be parallalized)

Execution time of the transition solving algorithm for the default shock (change in mortality and number of newborns) run on an Intel(R) Xeon(R) Platinum 8180 CPU @ 2.50GHz:

| implementation | income eff. | algorithm   | scoping              | threads | time in sec |
| :-------------- | ----------- | ----------- | -------------------- | -------: | -----: |
| Rcpp           | yes         | tatonnement | struct pass/unpack    | 20      | 0.32  |
| Rcpp           | yes         | tatonnement | global               | 20      | 0.35  |
| Rcpp           | yes         | tatonnement | struct pass/unpack    | 4       | 0.64  |
| Rcpp           | yes         | tatonnement | global               | 4       | 0.64  |
| Julia          | yes         | tatonnement | individ. slices pass | 20      | 1.35  |
| Rcpp           | yes         | tatonnement | global               | 1       | 1.68  |
| Rcpp           | yes         | tatonnement | struct pass/unpack    | 1       | 1.69  |
| Julia          | yes         | tatonnement | individ. slices pass | 4       | 1.69  |
| Julia          | yes         | tatonnement | struct pass/unpack    | 20      | 1.71  |
| Julia          | yes         | tatonnement | global               | 20      | 2.32  |
| Julia          | yes         | tatonnement | individ. slices pass | 1       | 2.33  |
| Julia          | yes         | tatonnement | struct pass/unpack    | 4       | 2.41  |
| Julia          | yes         | tatonnement | global               | 4       | 3.49  |
| Julia          | yes         | tatonnement | struct pass/unpack    | 1       | 4.22  |
| Python/numba   | yes         | tatonnement | partial pass         | 1       | 4.75 |
| R              | no          | tatonnement | global               | 1       | 5.25  |
| Julia          | yes         | tatonnement | global               | 1       | 6.94  |
| Matlab         | yes         | tatonnement | global               | 1       | 11.74 |
| R              | yes         | tatonnement | global               | 1       | 15.16 |
| Python         | yes         | tatonnement | global               | 1       | 27.54 |
| R              | no          | GFT         | global               | 1       | 50.67 |

*Note: Time is median time of 5 consecutive runs. The Rcpp solution in the table used fixed-dimension vectors/matrices and was compiled with `g++ -O3`. The Matlab implementation was run on a different CPU. Time was then rescaled using the relative difference of the results of the R implementation on the two different architectures.*

## Author
Philip Schuster

