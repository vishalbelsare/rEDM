#### rEDM NEWS

2024-04-05 version 1.15.4 <JosephPark@IEEE.org>

---

##### NOTES:
- It is required as of version 1.15 to use functions: `Simplex`, `SMap`, `CCM`, `Embed`, `Multiview`, `EmbedDimension`, `PredictInterval`, `PredictNonlinear`, `ComputeError` instead of the legacy version 0.7 signatures. See Version 1.3 notes.
- [Rcpp](https://CRAN.R-project.org/package=Rcpp) imposes a 20 parameter limit on functions. The rEDM wrapper of [cppEDM](https://github.com/SugiharaLab/cppEDM#empirical-dynamic-modeling-edm) therefore does not invoke the full cppEDM API. Users requiring the full API are referred to the [pyEDM](https://pypi.org/project/pyEDM/) wrapper.
- `SMap` linear system solver regularization: The R [glmnet](https://CRAN.R-project.org/package=glmnet) package does not seperate the model from the data. This prevents integration in rEDM. Users requiring `SMap` regularization are referred to the [pyEDM](https://pypi.org/project/pyEDM/) wrapper.

---

##### Version 1.15
- `SMap()` `ignoreNan` parameter added. If `ignoreNan` is `TRUE` (default) the library is redefined to ignore embedding vectors with nan. If `ignoreNan` is `FALSE` no change is made, the user can manually specify library segments in `lib`.
- `SMap()` return list includes data.frame of SVD singular values.
- `noTime` parameter added, default `FALSE`. If `noTime` is `TRUE` the first column of the data is not required to be a time vector. A row index vector will be inserted and passed to cppEDM. 
- `const_pred` parameter removed due to Rcpp 20 parameter limit.
- `CCM()` `replacement` parameter removed.
- Legacy overload functions removed.
- Version 1.15.1 `ignoreNan` added in `PredictNonlinear()`. Replace unicode in pLot labels with mathplot expression. cppEDM initialize `nanFound` in DataFrame.h for UBSAN. Sync with cppEDM 1.15.1.
- Version 1.15.2 Allow `columns` names with spaces. If the `columns` argument is a string use the "," delimiter to separate names. Remove `SMap` warning for disjoint library.
- Version 1.15.3 Allow `columns` and `target` names with spaces in CCM.
- Version 1.15.4 Move `SMap` warning for NaN into `verbose` mode. cppEDM `ComputeError` NaN removal improved, require more than 5 observation : prediction pairs.

##### Version 1.14
- cppEDM core added `generateLibrary` parameter to `Simplex()` and `SMap()`.  If `TRUE` the state-space library has newly generated points added. Not available due to Rcpp 20 parameter limit. 
- Version 1.14.2 Remove CCM multivariable warning, positive tau warning. Add Embedding application. 

##### Version 1.13
- Adds `embedded` and multivariate embedding to `CCM()`.
- Parameters `pathOut`, `predictFile` are removed from `CCM` to accomodate the Rcpp 20 parameter limit.
- Version 1.13.1 cppEDM DateTime H:M:S fix. Allow first column data.frame characters. Set target to columns[0] if empty.

##### Version 1.12
- Adds `exclusionRadius` and `validLib` to `EmbedDimension()`, `PredictInterval()` and `PredictNonlinear()`. 
- Version 1.12.2 Multiview return data.frame, correct SMap coefficient labels. 
- Version 1.12.2.1 Rcpp character encoding workaround on Windows for DataFrame column names.
- Version 1.12.3 cppEDM DateTime regex removed to avoid UTF-8 gcc issue in Windows.

##### Version 1.11
- Removes `nan` from `SMap` `columns` and `target`. Warning generated.

##### Version 1.10
- Adds the `generateSteps` parameter to `Simplex` and `SMap` implementing generative feedback prediction.
- Adds the `parameterList` argument to `Simplex`, `SMap`, `CCM` and `Multiview`.
- Parameters `pathOut`, `predictFile` are removed from `SMap`, `Multiview` to accomodate the Rcpp 20 parameter limit.
- Version 1.10.1 converts `parameterList` values to numerics.
- Version 1.10.2 is a bug fix for `Tp < 1` in generative mode.
- Version 1.10.3 `SMap` `dgelss` error message. `CCM` `libSize` limits `Tp < 0`.

##### Version 1.9
- Adds the `validLib` parameter to `Simplex` and `SMap`. `validLib` is a boolean vector with the same number of elements as input data rows.  For `validLib` elements that are `false`, the correspoding data row will not be included in the state-space library.
- Version 1.9.1 Requires .csv dataFiles to have column names.
- Version 1.9.2 is a bug fix for `CCM` parameter validation with `tau > 0`.
- Version 1.9.3 is a bug fix for `CCM` parameter validation with `Tp < -1`.

##### Version 1.8
- Removes the deletion of partial embedding data rows.
- Adds the `deletePartial` argument to `MakeBlock`.
- Bug fix in disjoint library indexing.

##### Version 1.7
- Updates nearest neighbors to better align results with legacy code.
- Bug fixes in `SMap`, `CMM` `includeData`, and, the use of disjoint libraries.

##### Version 1.6
- Attempts to label `SMap` coefficients with names from the `columns` and `target` parameters.
- Adds exclusionRadius to `CCM`.

##### Version 1.5
- Implemented an object oriented design in the core cppEDM.

##### Version 1.3
- A major rewrite of the 'rEDM' package as an Rcpp wrapper for the [cppEDM](https://github.com/SugiharaLab/cppEDM#empirical-dynamic-modeling-edm) library providing a unified computation engine for EDM algorithms across C++, Python and R implementations.  The revised package provides improved alignment between observed and forecast data rows, handling of date time vectors, and, strict exclusion of partial data vectors.

- To align with cppEDM and pyEDM, function names and signatures have changed from versions 0.7 and earlier. **It is recommended to use the new functions: `Simplex`, `SMap`, `CCM`, `Embed`, `Multiview`, `EmbedDimension`, `PredictInterval`, `PredictNonlinear`, `ComputeError`.** See [EDM Documentation](https://sugiharalab.github.io/EDM_Documentation/) or the package documentation.

- A legacy function interface is provided to emulate function signatures of rEDM 0.7, *but does not have complete coverage*.  It also has slightly different return values since nested data.frames are not returned.  Return values are either a data.frame, or, a named list of data.frames, as noted in the man pages.  Implemented functions' include: `simplex`, `s_map`, `block_lnlp`, `ccm`, `multiview`, `make_block`, `compute_stats` and `make_surrogate_data`.  Functions `ccm_means`, `tde_gp`, `block_gp` and `test_nonlinearity` are deprecated.
