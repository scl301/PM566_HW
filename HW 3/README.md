Assignment 3: Web Scraping and Text Mining
================
Stephanie Lee
2023-05-01

## APIs

### Number of Papers

``` r
# Downloading the website
website <- xml2::read_html("https://pubmed.ncbi.nlm.nih.gov/?term=sars-cov-2+trial+vaccine")

# Finding the counts
counts <- xml2::xml_find_first(website, "/html/body/main/div[9]/div[2]/div[2]/div[1]")

# Turning it into text
counts <- as.character(counts)

# Extracting the data using regex
stringr::str_extract(counts, "[0-9,]+")
```

    ## [1] "4,644"

### IDs

``` r
query_ids <- GET(
  url   = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi",
  query = list(db= "pubmed", term= "sars-cov-2 trial vaccine", retmax= 250)
)

ids <- httr::content(query_ids)
```

``` r
ids <- as.character(ids)
ids <- stringr::str_extract_all(ids, "<Id>[[:digit:]]+</Id>")[[1]]
ids <- stringr::str_remove_all(ids, "</?Id>")
```

``` r
publications <- GET(
  url   = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi",
  query = list(
    db      = "pubmed",
    id      = paste(ids, collapse = ","),
    rettype = "abstract"
    )
)
publications <- httr::content(publications)
publications_txt <- as.character(publications)
```

``` r
# Details
pub_char_list <- xml2::xml_children(publications)
pub_char_list <- sapply(pub_char_list, as.character)

# Titles
titles <- str_extract(pub_char_list, "<ArticleTitle>(\\n|.)+</ArticleTitle>")
titles <- str_remove_all(titles, "</?[[:alnum:]]+>")
titles <- str_replace_all(titles, "\\s+"," ")

# Abstracts
abstracts <- str_extract(pub_char_list, "<Abstract>[[:print:][:space:]]+</Abstract>")
abstracts <- str_remove_all(abstracts, "</?[[:alnum:]- =\"]+>")
abstracts <- str_replace_all(abstracts, "[[:space:]]+", " ")

# Journals
journals <- str_extract(pub_char_list, "<Title>(\\n|.)+</Title>")
journals<- str_remove_all(journals, "</?[[:alnum:]]+>")
journals<- str_replace_all(journals, "\\s+"," ")

# Dates
dates <- str_remove_all(pub_char_list, "\\n") %>%
    str_extract("<PubDate>.*</PubDate>")
dates <-str_remove_all(dates, "</?[[:alnum:]]+>")
##remove new line and spaces
dates <- str_replace_all(dates, "\\s+"," ")

# Create Database
database <- data.frame(
  PubMedID = ids,
  Title    = titles,
  Journal = journals,
  Abstract = abstracts,
  Date = dates
)
knitr::kable(database[1:2,], caption = "Sars-cov-2 Trial Vaccine Publications")
```

| PubMedID | Title                                                                                                                                                                                                    | Journal                   | Abstract                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Date     |
|:---------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------|
| 37124230 | Fighting cytokine storm and immunomodulatory deficiency: By using natural products therapy up to now.                                                                                                    | Frontiers in pharmacology | A novel coronavirus strain (COVID-19) caused severe illness and mortality worldwide from 31 December 2019 to 21 March 2023. As of this writing, 761,071,826 million cases have been diagnosed worldwide, with 6,879,677 million deaths accorded by WHO organization and has spread to 228 countries. The number of deaths is closely connected to the growth of innate immune cells in the lungs, mainly macrophages, which generate inflammatory cytokines (especially IL-6 and IL-1β) that induce “cytokine storm syndrome” (CSS), multi-organ failure, and death. We focus on promising natural products and their biologically active chemical constituents as potential phytopharmaceuticals that target virus-induced pro-inflammatory cytokines. Successful therapy for this condition is currently rare, and the introduction of an effective vaccine might take months. Blocking viral entrance and replication and regulating humoral and cellular immunity in the uninfected population are the most often employed treatment approaches for viral infections. Unfortunately, no presently FDA-approved medicine can prevent or reduce SARS-CoV-2 access and reproduction. Until now, the most important element in disease severity has been the host’s immune response activation or suppression. Several medicines have been adapted for COVID-19 patients, including arbidol, favipiravir, ribavirin, lopinavir, ritonavir, hydroxychloroquine, chloroquine, dexamethasone, and anti-inflammatory pharmaceutical drugs, such as tocilizumab, glucocorticoids, anakinra (IL-1β cytokine inhibition), and siltuximab (IL-6 cytokine inhibition). However, these synthetic medications and therapies have several side effects, including heart failure, permanent retinal damage in the case of hydroxyl-chloroquine, and liver destruction in the case of remdesivir. This review summarizes four strategies for fighting cytokine storms and immunomodulatory deficiency induced by COVID-19 using natural product therapy as a potential therapeutic measure to control cytokine storms. Copyright © 2023 Mohammed.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | 2023     |
| 37123709 | Media Reporting Relating to COVID-19 Vaccination as a Driver of Vaccine Hesitancy Prior to the Second Wave of the COVID-19 Pandemic in India: A Content Analysis of Newspaper and Digital Media Reports. | Cureus                    | Background Over 2,40,000 deaths were attributed to the SARS-CoV-2 Delta (B.1.617.2) variant in India during the second wave of the pandemic from April to June 2021 with most deaths occurring in the unvaccinated population. High levels of coronavirus disease 2019 (COVID-19) vaccine hesitancy contributed to significantly reduced vaccination coverage in the eligible population especially among healthcare workers, comorbid and older people. The existing global evidence suggests misinformation through social media to accentuate, while newspaper and mainstream media reporting to be protective against vaccine hesitancy during the COVID-19 pandemic. Content analysis of regular press coverage of COVID-19 vaccination in India during the period of initial deployment and until the onset of the second wave of the pandemic can provide useful learnings and strengthen preparedness for addressing potential vaccine hesitancy concerns during future pandemics. Therefore, we conducted this inductive content analysis of press coverage related to the COVID-19 vaccine hesitancy in India prior to the second (Delta) wave of the COVID-19 pandemic. Methods We examined news reports related to COVID-19 vaccination in India for the period from 1st January 2021 to 28 February 2021 from a high circulation English language daily (Hindustan Times), Hindi (vernacular) language daily (Dainik Jagran), and English language news reports from selected digital news portals. The inclusion criterion was any news report related to COVID-19 vaccination including editorials and guest opinion pieces that could potentially generate COVID-19-related vaccine hesitancy. The news items were classified depending on their potential to drive vaccine hesitancy by either avoiding reporting of positive information related to COVID-19 vaccines, or attributing directly or indirectly, negative or misleading commentary relating to vaccine safety or efficacy. Reports with possible risk of increasing vaccine hesitancy were further analyzed based on content, source of information, and the extent of fact-checking. Results Most of the published newspaper reports examined in this study echoed official news sources and views from government health agencies promoting COVID-19 vaccine acceptance and dispelling doubts on concerns regarding vaccine safety. There were eight unique newspaper reports after excluding duplicated bilingual entries and four news items from online digital Indian news sources that met our criterion of reports with possible contribution to vaccine hesitancy. The reports possibly contributed to vaccine hesitancy were grouped into two themes: (i) Doubts on the safety and efficacy of local manufactured vaccines: most of these reports focused on the granting of emergency use authorization for Covaxin (BBV152) in ‘clinical trial mode’ without the completion and publication of Phase-3 efficacy data (ii). Doubts on vaccine requirement considering high seroprevalence and reduced virus transmission. Conclusions Concerns about the efficacy and safety of Covaxin (BBV152), safety of the Covishield vaccine, and questioning the necessity of immunizing all adults with COVID-19 vaccines were observed in multiple press reports with attempts at politicization of vaccination-related decisions. The press reporting with potential for contributing to significant COVID-19 vaccine hesitancy since launch and until the Delta wave of the pandemic in India has important lessons in future pandemic preparedness. Copyright © 2023, Basu et al. | 2023 Mar |

Sars-cov-2 Trial Vaccine Publications

## Text Mining

``` r
if (!file.exists("pubmed.csv")){
  download.file("https://raw.githubusercontent.com/USCbiostats/data-science-data/master/03_pubmed/pubmed.csv", "pubmed.csv", method="libcurl", timeout = 60)
}
pubmed <- read.csv("pubmed.csv")
pubmed <- as_tibble(pubmed)
```

**1. Tokenize the abstracts and count the number of each token. Do you
see anything interesting? Does removing stop words change what tokens
appear as the most frequent? What are the 5 most common tokens for each
search term after removing stopwords?**

``` r
pubmed %>%
  unnest_tokens(token, abstract) %>%
  count(token, sort = TRUE) # %>%
```

    ## # A tibble: 20,567 × 2
    ##    token     n
    ##    <chr> <int>
    ##  1 the   28126
    ##  2 of    24760
    ##  3 and   19993
    ##  4 in    14653
    ##  5 to    10920
    ##  6 a      8245
    ##  7 with   8038
    ##  8 covid  7275
    ##  9 19     7080
    ## 10 is     5649
    ## # ℹ 20,557 more rows

``` r
  #top_n(20, n) %>%
  #ggplot(aes(n, fct_reorder(token, n))) +
  #geom_col()
```

The most frequently occurring tokens are stop words.

``` r
pubmed %>%
  unnest_tokens(word, abstract) %>%
  count(word, sort = TRUE) %>%
  anti_join(stop_words, by = c("word")) %>%
  top_n(5, n)
```

    ## # A tibble: 5 × 2
    ##   word         n
    ##   <chr>    <int>
    ## 1 covid     7275
    ## 2 19        7080
    ## 3 patients  4674
    ## 4 cancer    3999
    ## 5 prostate  3832

After the stop words are removed, there is definitely a change in the
most frequent tokens. The top 5 tokens are now “covid”, “19”,
“patients”, “cancer”, and “prostate”.

**2. Tokenize the abstracts into bigrams. Find the 10 most common bigram
and visualize them with ggplot2.**

``` r
pubmed %>%
  unnest_ngrams(bigram, abstract, n=2) %>%
  separate(bigram, into = c("first", "second"), sep = " ",remove = FALSE) %>%
  anti_join(stop_words, by = c(first = "word")) %>%
  anti_join(stop_words, by = c(second = "word")) %>%
  count(bigram, sort = TRUE) %>%
  top_n(10, n) %>%
  ggplot(aes(n, fct_reorder(bigram, n))) +
  geom_col()+
  labs(y = "bigram", title = "Top 10 Most Frequent Bigrams (Without Stop Words)") 
```

![](README_files/figure-gfm/top%2010%20bigrams-1.png)<!-- -->

**3. Calculate the TF-IDF value for each word-search term combination.
(here you want the search term to be the “document”) What are the 5
tokens from each search term with the highest TF-IDF value? How are the
results different from the answers you got in question 1?**

``` r
term_table <-pubmed %>%
  group_by(term) %>% 
  unnest_tokens(word, abstract) %>%
  count(word, sort = TRUE) %>%
    bind_tf_idf(word, term, n)

term_table %>%
    top_n(5,tf_idf) %>% 
    arrange(desc(term)) %>%
knitr::kable(digits =4, align=c("l", "c", "c", "c","c","c"), caption = "Top 5 TF-IDFs by Search Term")
```

| term            |      word       |  n   |   tf   |  idf   | tf_idf |
|:----------------|:---------------:|:----:|:------:|:------:|:------:|
| prostate cancer |    prostate     | 3832 | 0.0312 | 1.6094 | 0.0502 |
| prostate cancer |    androgen     | 305  | 0.0025 | 1.6094 | 0.0040 |
| prostate cancer |       psa       | 282  | 0.0023 | 1.6094 | 0.0037 |
| prostate cancer |  prostatectomy  | 215  | 0.0017 | 1.6094 | 0.0028 |
| prostate cancer |   castration    | 148  | 0.0012 | 1.6094 | 0.0019 |
| preeclampsia    |    eclampsia    | 2005 | 0.0143 | 1.6094 | 0.0230 |
| preeclampsia    |  preeclampsia   | 1863 | 0.0133 | 1.6094 | 0.0214 |
| preeclampsia    |    pregnancy    | 969  | 0.0069 | 0.5108 | 0.0035 |
| preeclampsia    |    maternal     | 797  | 0.0057 | 0.5108 | 0.0029 |
| preeclampsia    |   gestational   | 191  | 0.0014 | 1.6094 | 0.0022 |
| meningitis      |   meningitis    | 429  | 0.0092 | 1.6094 | 0.0148 |
| meningitis      |    meningeal    | 219  | 0.0047 | 1.6094 | 0.0076 |
| meningitis      |       csf       | 206  | 0.0044 | 0.9163 | 0.0040 |
| meningitis      | pachymeningitis | 149  | 0.0032 | 1.6094 | 0.0051 |
| meningitis      |    meninges     | 106  | 0.0023 | 1.6094 | 0.0037 |
| cystic fibrosis |    fibrosis     | 867  | 0.0176 | 0.5108 | 0.0090 |
| cystic fibrosis |     cystic      | 862  | 0.0175 | 0.5108 | 0.0090 |
| cystic fibrosis |       cf        | 625  | 0.0127 | 0.9163 | 0.0117 |
| cystic fibrosis |      cftr       |  86  | 0.0018 | 1.6094 | 0.0028 |
| cystic fibrosis |      sweat      |  83  | 0.0017 | 1.6094 | 0.0027 |
| covid           |      covid      | 7275 | 0.0371 | 1.6094 | 0.0597 |
| covid           |    pandemic     | 800  | 0.0041 | 1.6094 | 0.0066 |
| covid           |   coronavirus   | 647  | 0.0033 | 1.6094 | 0.0053 |
| covid           |      sars       | 372  | 0.0019 | 1.6094 | 0.0031 |
| covid           |       cov       | 334  | 0.0017 | 1.6094 | 0.0027 |

Top 5 TF-IDFs by Search Term

Factoring the TF-IDF values provides greater information on the
relevancy of a term. These terms differ from those in question 1, as
they are more specific to terminology and conditions rather than simply
the frequency of all words independently.
