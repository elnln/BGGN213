Class 11: Intro to Genome Informatics
================
Elena

- <a
  href="#q5.-what-proportion-of-the-mexican-ancestry-in-los-angeles-sample-population-mxl-are-homozygous-for-the-asthma-associated-snp-gg"
  id="toc-q5.-what-proportion-of-the-mexican-ancestry-in-los-angeles-sample-population-mxl-are-homozygous-for-the-asthma-associated-snp-gg">Q5.
  What proportion of the Mexican Ancestry in Los Angeles sample population
  (MXL) are homozygous for the asthma associated SNP (G|G)?</a>
- <a
  href="#q13.-read-this-file-into-r-and-determine-the-sample-size-for-each-genotype-and-their-corresponding-median-expression-levels-for-each-of-these-genotypes."
  id="toc-q13.-read-this-file-into-r-and-determine-the-sample-size-for-each-genotype-and-their-corresponding-median-expression-levels-for-each-of-these-genotypes.">Q13.
  Read this file into R and determine the sample size for each genotype
  and their corresponding median expression levels for each of these
  genotypes.</a>
- <a
  href="#q14-generate-a-boxplot-with-a-box-per-genotype-what-could-you-infer-from-the-relative-expression-value-between-aa-and-gg-displayed-in-this-plot-does-the-snp-effect-the-expression-of-ormdl3"
  id="toc-q14-generate-a-boxplot-with-a-box-per-genotype-what-could-you-infer-from-the-relative-expression-value-between-aa-and-gg-displayed-in-this-plot-does-the-snp-effect-the-expression-of-ormdl3">Q14:
  Generate a boxplot with a box per genotype, what could you infer from
  the relative expression value between A/A and G/G displayed in this
  plot? Does the SNP effect the expression of ORMDL3?</a>

## Q5. What proportion of the Mexican Ancestry in Los Angeles sample population (MXL) are homozygous for the asthma associated SNP (G\|G)?

14.1%

``` r
mxl <- read.csv("373531-SampleGenotypes-Homo_sapiens_Variation_Sample_rs8067378.csv")
```

``` r
table(mxl$Genotype..forward.strand.)
```


    A|A A|G G|A G|G 
     22  21  12   9 

``` r
table(mxl$Genotype..forward.strand.) / nrow(mxl) * 100
```


        A|A     A|G     G|A     G|G 
    34.3750 32.8125 18.7500 14.0625 

## Q13. Read this file into R and determine the sample size for each genotype and their corresponding median expression levels for each of these genotypes.

- A/A: Sample Size 108, Median Expression 31.2

- A/G: Sample Size 233, Median Expression 25.1

- G/G: Sample Size 121, Median Expression 20.1

``` r
expr <- read.table("rs8067378_ENSG00000172057.6.txt")
head(expr)
```

       sample geno      exp
    1 HG00367  A/G 28.96038
    2 NA20768  A/G 20.24449
    3 HG00361  A/A 31.32628
    4 HG00135  A/A 34.11169
    5 NA18870  G/G 18.25141
    6 NA11993  A/A 32.89721

``` r
nrow(expr)
```

    [1] 462

``` r
table(expr$geno)
```


    A/A A/G G/G 
    108 233 121 

``` r
library(dplyr)
expr.aa <- expr %>% filter(geno=="A/A")
expr.ag <- expr %>% filter(geno=="A/G")
expr.gg <- expr %>% filter(geno=="G/G")
```

``` r
median(expr.aa$exp)
```

    [1] 31.24847

``` r
median(expr.ag$exp)
```

    [1] 25.06486

``` r
median(expr.gg$exp)
```

    [1] 20.07363

## Q14: Generate a boxplot with a box per genotype, what could you infer from the relative expression value between A/A and G/G displayed in this plot? Does the SNP effect the expression of ORMDL3?

G/G has a lower expression value relative to A/A, and A/G has an
intermediate expression value; it seems that G reduces the expression of
ORMDL3. From this observation, I would infer that the SNP does affect
the expression of ORMDL3.

``` r
library(ggplot2)
ggplot(expr) + 
  aes(x=geno,y=exp,fill=geno) + 
  geom_boxplot(show.legend=F, notch=T) +
  geom_jitter(show.legend=F, alpha=0.3) +
  labs(x="Genotype",y="Expression")
```

![](class11_files/figure-gfm/unnamed-chunk-11-1.png)
