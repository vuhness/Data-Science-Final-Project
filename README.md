# Final Project Data-Management-Final-Project

Please upload pcos_diagnosis(2).ipynb in Jupyter notebook. All packages and XGBoost should be installed upon running the cells. The csv files in this repository was the data produced upon my run. The data produced when you run it may be different then the csv files generated but is functionally the same. 






















AUTHORS THOUGHT PROCESS (OPTIONAL READ)
generating randomized data 

- don't want to generate static values that have no relationships 
- need to implement biases and weights inspired by clinical literature 
- example: higher levels of insulin resistance in different demographics 
- knowing baseline medical ranges 
- disparities: prevalence (frequency), diagnostic delay (how long to get diagnosis) 
- black women have higher prevalence of PCOS + more severe metabolic symptoms like higher BMI/insulin resistance, often diagnoses less frequently/later than white women 
- reduce probability of diagnosis for specific groups to simulate UNDERDIAGNOSIS 
- races: 'Black', 'White', 'Hispanic', 'Asian', 'NHPI', 'AIAN'




SIMULATING THE DIAGNOSTIC GAP 
- threshold for black and hispanic women to get dianosed is much higher than their white counterparts, even if they exhibit the same symptoms 
- typically leaves black/hispanic women undiagnosed, issues are systemically ignored 
- black/hispanic women may not get True diagnosis until BMI is higher
- this will generate false negatives, where people HAVE PCOS, but are labeled otherwise 
- in the future, can recalibrate model to look at symptoms more objectively, regardless of label 

MEDICAL RESEARCH 
- black women with pcos often have higher BMI, 3-5 points higher, and higher insulin resistance than white women 
- hirsutisim (hair growth): hispanic/middle eastern women have higher baseline scores for hair growth 
- black and hispanic women likely diagnosed 1-3 years AFTER first presenting 
symptoms 
- insulin resistance: high insulin simulates ovaries to produce EXCESS ANDROGENS, which cause acne, hair growth (hirsutism), menstrual irregularity, weight gain, increased risk of type 2 diabetes 
- increased insulin resistance --> higher androgen production via ovary simulation 

DATA CLEANING COMPETENCY 
- I wanted to demonstrate my competency in data cleaning and also wanted to simulate real life dirty data 
- added randomized duplicated rows, outliers, etc. 
- cleaned this data via iqr calculation, deleting nans, feature datatype conversion, etc. 


FEATURE ENGINEERING 

ROTTERDAM INDICATOR (count symptoms) 
- eligible for clinical diagnosis by exhibiting 2/3 major markers
   - markets: menstrual irregularity, high testosterone, high_afc 
   - high testosterone = > 55 
- high antral follicle count: 20 or more follicles measuring 2-9mm in at least one ovary 
- things to consider: don't have category for hirsutism, multifollicular ovaries are common in puberty, not reliable 

GROUP-NORMALIZED BMI
-we know that black and hispanic women have higher baseline BMIs, so calculating the z-score per race can be helpful 
- "how high is this woman's bmi compared to others of her SAME race" 
- good bias indicators

BMI-to-Age ratio
- younger patient with higher BMI can be a stronger indicator for PCOS than older patient with the same bmi 


initial scores:
              precision    recall  f1-score   support

           0       0.82      0.86      0.84        37
           1       0.80      0.74      0.77        27

    accuracy                           0.81        64
   macro avg       0.81      0.80      0.81        64
weighted avg       0.81      0.81      0.81        64

ROC-AUC Score: 0.8498
Overall Accuracy: 0.8125
AUC-ROC Score: 0.8498

Accuracy for Black Women: 0.7857
Accuracy for White Women: 0.8000
Accuracy for Hispanic Women: 0.8750
Accuracy for Asian Women: 0.7500
Accuracy for NHPI Women: 0.8571
Accuracy for AIAN Women: 0.8000

initial model; 
xgb_model = xgb.XGBClassifier(
    n_estimators=100,      # num trees 
    max_depth=4,           # depth of each tree 
    learning_rate=0.1,     # step size shrinkage 
    subsample=0.8,         # use 80% of data per tree, prevent overfitting MAY CHANGE 
    use_label_encoder=False,
    eval_metrics='logloss'
)


IMPLEMENTATION OF GRID SEARCH: 
using grid search to find the best parameters after receiving these initial baseline results
- caveat: grid search takes too long, random samplinginstead 

IMPLEMENTATION OF RANDOM SAMPLING:
