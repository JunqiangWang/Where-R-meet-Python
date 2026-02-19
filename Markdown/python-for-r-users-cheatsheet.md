# Python for R Users --- Cheat Sheet

Quick correspondences for common data, wrangling, stats, viz, and
modeling tasks.\
*Updated Feb 18, 2026.*

------------------------------------------------------------------------

## 1) Core data structures

  Concept          Python                  R
  ---------------- ----------------------- -----------------------------
  Numeric vector   `list`, `numpy.array`   `c()`
  Matrix           `numpy.array (2D)`      `matrix()`
  Data frame       `pandas.DataFrame`      `data.frame()` / `tibble()`
  1D labeled       `pandas.Series`         named vector
  Categorical      `pandas.Categorical`    `factor()`
  Missing value    `numpy.nan` / `pd.NA`   `NA`

------------------------------------------------------------------------

## 2) Data wrangling: pandas ↔ dplyr

  -------------------------------------------------------------------------------------------------
  Task            Python (pandas)                         R (dplyr / tidyr)
  --------------- --------------------------------------- -----------------------------------------
  Select columns  `df[['a','b']]`                         `select(df, a, b)`

  Filter rows     `df[df.a > 5]`                          `filter(df, a > 5)`

  Add/transform   `df['c'] = ...`                         `mutate(df, c = ...)`
  column                                                  

  Group +         `df.groupby('a').agg(...)`              `df %>% group_by(a) %>% summarise(...)`
  summarize                                               

  Sort            `df.sort_values('a')`                   `arrange(df, a)`

  Join            `pd.merge(x, y, on='id', how='left')`   `left_join(x, y, by='id')`

  Distinct        `df.drop_duplicates()`                  `distinct(df)`

  Rename          `df.rename(columns={'old':'new'})`      `rename(df, new = old)`

  Pivot wider     `df.pivot(...)`                         `pivot_wider(...)`

  Pivot longer    `pd.melt(df, ...)`                      `pivot_longer(...)`
  -------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 3) Stats: common tests & summaries

  ----------------------------------------------------------------------------------------
  Thing                     Python                                R
  ------------------------- ------------------------------------- ------------------------
  Mean / SD                 `np.mean(x)` / `np.std(x, ddof=1)`    `mean(x)` / `sd(x)`

  Median / quantile         `np.median(x)` / `np.quantile(x, q)`  `median(x)` /
                                                                  `quantile(x, probs=q)`

  Correlation               `scipy.stats.pearsonr(x,y)`           `cor(x,y)`

  t-test                    `scipy.stats.ttest_ind(x,y)`          `t.test(x,y)`

  Chi-square                `scipy.stats.chi2_contingency(tbl)`   `chisq.test(tbl)`
  ----------------------------------------------------------------------------------------

**Linear regression**

``` python
statsmodels.formula.api.ols('y ~ x1 + x2', data=df).fit()
```

``` r
lm(y ~ x1 + x2, data=df)
```

**Logistic regression**

``` python
statsmodels.formula.api.logit('y ~ x', data=df).fit()
```

``` r
glm(y ~ x, data=df, family=binomial())
```

------------------------------------------------------------------------

## 4) Visualization: matplotlib / seaborn ↔ ggplot2

  -------------------------------------------------------------------------------------------------------------
  Plot                      Python                                     R
  ------------------------- ------------------------------------------ ----------------------------------------
  Scatter                   `sns.scatterplot(data=df, x='x', y='y')`   `ggplot(df, aes(x, y)) + geom_point()`

  Line                      `plt.plot(x, y)`                           `ggplot(df, aes(x, y)) + geom_line()`

  Histogram                 `plt.hist(x, bins=30)`                     `geom_histogram(bins=30)`

  Boxplot                   `sns.boxplot(data=df, x='g', y='y')`       `geom_boxplot()`
  -------------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 5) Machine learning: scikit-learn ↔ caret / tidymodels

  -------------------------------------------------------------------------------------------------
  Task                      Python                                 R
  ------------------------- -------------------------------------- --------------------------------
  Train/test split          `train_test_split()`                   `initial_split()` /
                                                                   `createDataPartition()`

  Fit model                 `model.fit(X_train, y_train)`          `fit()` / `train(...)`

  Predict                   `model.predict(X_test)`                `predict(model, newdata=test)`

  Random forest             `RandomForestClassifier` / `Regressor` `ranger` / `randomForest`

  Cross-validation          `cross_val_score(model, X, y, cv=5)`   `vfold_cv(data, v=5)`
  -------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## 6) File I/O

  ------------------------------------------------------------------------------------------------------
  Task                      Python                                   R
  ------------------------- ---------------------------------------- -----------------------------------
  Read CSV                  `pd.read_csv('file.csv')`                `read.csv()` / `readr::read_csv()`

  Write CSV                 `df.to_csv('file.csv', index=False)`     `write.csv(..., row.names=FALSE)`

  Read Excel                `pd.read_excel('file.xlsx')`             `readxl::read_excel()`

  Read JSON                 `json.load(open('file.json'))`           `jsonlite::fromJSON()`

  Save object               `pickle.dump(obj, open('x.pkl','wb'))`   `saveRDS(obj, 'x.rds')`

  Load object               `pickle.load(open('x.pkl','rb'))`        `readRDS('x.rds')`
  ------------------------------------------------------------------------------------------------------

------------------------------------------------------------------------

## Handy mental mapping

-   `%>%` pipe ≈ method chaining (`df.assign(...).query(...)`) or
    `.pipe()`
-   `tibble` ≈ DataFrame (print-friendly, column-first)
-   `y ~ x` formulas ≈ statsmodels formula strings
-   NA handling: watch `NaN` vs `pd.NA` dtype behavior in pandas

------------------------------------------------------------------------

*Tip:* If you tell me what packages you use most in R, this can be
tailored to your workflow.
