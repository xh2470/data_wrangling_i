Tidy data
================

``` r
pulse_df = 
  read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

    ## Warning in FUN(X[[i]], ...): strings not representable in native encoding will
    ## be translated to UTF-8

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C4>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00D6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E4>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00F6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00DF>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E6>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00D8>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00F8>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00C5>' to native encoding

    ## Warning in FUN(X[[i]], ...): unable to translate '<U+00E5>' to native encoding

Let’s try to pivot

``` r
pulse_tidy = 
  pulse_df %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bdi"
  ) %>% 
  mutate(
    visit = replace(visit, visit == "bl","00m"),
    visit = factor(visit)
  )
```

## pivot\_wider

``` r
analysis_df = 
  tibble(
    group = c("treatment", "treatment", "control", "control"),
    time = c("a","b","a","b"),
    group_mean = c(4,8,3,6)
  )

analysis_df %>% 
  pivot_wider(
    names_from = "time",
    values_from = "group_mean"
  ) %>% 
  knitr::kable()
```

| group     |   a |   b |
|:----------|----:|----:|
| treatment |   4 |   8 |
| control   |   3 |   6 |
