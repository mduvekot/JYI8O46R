# JYI8O46R

Files come from two sources: canlii and a paper by August & Mah 

The canlii dataset for onltb cases has two parts, the fist part contains the caseIds, a very basic dataset that just has 6 variables: 

$ databaseId     <chr> "onltb", "onltb", "onltb", "onltb", "onltb", "onltb", "…
$ title          <chr> "Champagne v Pinheiro Salles", "Walters v Toronto Commu…
$ citation       <chr> "2026 ONLTB 26877 (CanLII)", "2026 ONLTB 19857 (CanLII)…
$ caseId.en      <chr> "2026onltb26877", "2026onltb19857", "2026onltb19864", "…
$ aiContentId.en <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…
$ caseId.fr      <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,…

We're limited to 10,000 results per query, so we can get the caseIds with a query like this:

https://api.canlii.org/v1/caseBrowse/en/onltb/?offset=0&resultCount=10000&api_key=<api_key>

These results have been combined and stored as cases_raw.csv (it is up-to_date to 2026-05-30)

#

The title can (sometimes) be split into the name of the  organization that filed the case the organization on whose behalf the case was filed and the person or organization who is the subject of the case.

We split those on <landlord> c/o <property manager> v <tenant>. 
We normalize (canonicalize) the names of the landlords and propery managers to eliminate variations in company names. Canonicalized names are stored in a column with the "_can" suffix.

We also try to infer from the name of the landlord/property manager if a case has been filed by or gainst a corporate landlord, based on the occurnce of strings like Ltd. or Inc. in which case we set plaintiff_is_corp or defendant_is_corp to TRUE.

$ title                <chr> "Miller v Kapenda", "Helu v Antonis", "Ottawa Com…
$ plaintiff_can        <chr> "Miller", "Helu", "Ottawa Community Housing", "To…
$ defendant_can        <chr> "Kapenda", "Antonis", "Kabeya", "Khachapuridze", …
$ property_manager_can <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, "Skyline Livi…
$ year                 <dbl> 2026, 2026, 2026, 2026, 2026, 2026, 2026, 2026, 2…
$ plaintiff_is_corp    <lgl> FALSE, FALSE, TRUE, TRUE, TRUE, FALSE, TRUE, FALS…
$ defendant_is_corp    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, …
$ type                 <chr> "indiv v. indiv", "indiv v. indiv", "corp v. indi…

We store that as cases_clean.csv 

## 

Once we have the caseIds, we can look up the metadata for each case. 

To get the metadata for a case, we use a query like:

https://api.canlii.org/v1/caseBrowse/en/onltb/2026onltb23231/?api_key=<api_key>

We're limited to 5000 quaries per day, so we query the canlii database in batches of 4000, and store the results as 

results_start_end.csv

###############################################################################
a second source of data is a paper by August & Mah
https://doi.org/10.1080/02723638.2025.2531934


Table 1 is T0001-10.1080_02723638.2025.2531934.csv
Table 2 is T0002-10.1080_02723638.2025.2531934.csv

Supplemental Material is 
rurb_a_2531934_sm2240.docx
rurb_a_2531934_sm2238.docx

that have been transcribed as 
rurb_a_2531934_sm2240.csv
rurb_a_2531934_sm2238.csv


