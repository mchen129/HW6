# Project for Stata II (Intermediate)


## Project aim: 
Use public data to explore the significance of “self-reported health” as a health indicator.
  - This project will be kept working for the next 3 weeks. Other collaborators are welcome to contribute by adding their inputs to `read.me` file in [my PROJECT repository](https://raw.githubusercontent.com/mchen129/project/main/) and putting a comment while committing changes.


## Data Source:
  We will use public datasets from [National Health and Nutrition Examination Survey (NHANES)](https://www.cdc.gov/nchs/nhanes/index.htm) for this project including:
  - [NHANES Survey Data](https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT)
  - [NHANES Mortality Followup Data](https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/)
    - Use `NHANES_1999_2000_MORT_2019_PUBLIC.dat`

	
## Codes Development:
### 1. Download, edit, and upload the `Stata_ReadInProgramALlSurveys.do` from NHANES to the github.
  - This do file imports and prepares the mortality data needed for further analysis.
  - Please Refer to the followup.do to see edits.
  ```stata
cat https://ftp.cdc.gov/pub/HEALTH_STATISTICS/NCHS/datalinkage/linked_mortality/Stata_ReadInProgramAllSurveys.do   
```
### 2. Data merging 
  ```stata
global repo "https://github.com/mchen129/project/main/"
do ${repo}followup.do
save followup, replace
import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/DEMO.XPT", clear
merge 1:1 seqn using followup
lookfor follow
```


## Prepare key Parameters for Week 7s Analysis
  - Self-Reported Health Assessment:
    - Import the specific health questionnaire data and prepare it for analysis in Week 7: [HUQ dataset](https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.XPT)
   ```stata
lookfor mortstat permth_int eligstat 
keep if eligstat==1
capture g years=permth_int/12
codebook mortstat
stset years, fail(mortstat)
sts graph, fail
save demo_mortality, replace 
import sasxport5 "https://wwwn.cdc.gov/Nchs/Nhanes/1999-2000/HUQ.XPT", clear 
merge 1:1 seqn using demo_mortality, nogen
sts graph, by(huq010) fail
stcox i.huq010
