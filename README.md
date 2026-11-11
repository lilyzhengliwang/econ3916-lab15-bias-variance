# econ3916-lab15-bias-variance

# The Bias-Variance Tradeoff: Choosing How Flexible a Model Should Be

## Objective
I wanted to see how the flexibility of a polynomial model changes its error, and how to choose and honestly score a degree when I only have 60 data points.

## Methodology
- Simulated a world with a known curve, sin(1.4x) + 0.25x, observed with noise (sd 0.45), and drew 60 training points.
- Split the error into bias squared and variance for degrees 1, 2, 3, 5, 9 and 15, using 200 fresh samples of 60 points at each degree.
- Compared training error with error on 2,000 new points for degrees 1 to 15, and found the degree with the lowest held-out error.
- Chose a degree with 5-fold cross-validation for degrees 1 to 11, and looked at how much the error varied from fold to fold.
- Ran nested cross-validation (an inner loop to pick the degree, an outer fold used only for scoring) to check whether the smallest CV error is a fair estimate of the error on new data.
- Wrote a function, `kfold_mse(x, y, deg, k)`, that returns the k-fold CV error for any degree and number of folds. I checked it against my 5-fold result, then used it to try 10 folds.
- Challenge: fitted minimum-norm polynomials in a Legendre basis with degrees up to 1000, far past the point where the model has as many coefficients as data points.

## Key Findings
- Bias squared fell from 0.50 at degree 1 to about 0.0002 at degree 9, and then stayed near zero. Variance rose from 0.0180 at degree 5 to 0.0994 at degree 15.
- The lowest held-out error was at degree 5. Training error fell at every step, so it cannot be used to choose a degree.
- 5-fold CV chose degree 4 with a CV MSE of 0.1401. Degree 5 scored 0.1510, and the fold-to-fold spread was 0.0223 at degree 4 and 0.0267 at degree 5, so I treat the two as close to tied.
- The nested CV estimate was 0.2070, higher than the smallest CV error of 0.1401. Picking the best of eleven scores makes that score too optimistic, so I would report the nested figure as the expected error on new data.
- With 10 folds, the CV MSE was 0.1409 for degree 4 and 0.1457 for degree 5. Degree 4 still ranks ahead, but the gap is smaller than with 5 folds.
- Past the interpolation point, held-out error did not keep rising. It peaked at about 10^24 for degrees near 51, then came back down to 0.32 at degree 991. That is still higher than the 0.2284 that degree 4 scored on the same 2,000 points, so a very large model did not beat a well-chosen small one.
