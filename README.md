# Player_Role_Classifier_on_Australian_Football_League
This project builds a supervised machine learning model to predict an Australian football player’s on‑field role (Defender, Midfield, Forward, Ruck) from their match statistics.

Project overview
Load three CSV datasets:

Player metadata (PlayerId, name, height, weight, original position, origin club).​

Match‑level information (game ID, year, round, venue, weather, scores).​

Per‑game player stats (disposals, kicks, marks, handballs, goals, hit‑outs, tackles, clearances, contested/uncontested possessions, etc.).​

Merge stats with player information using PlayerId to attach a position label to each stats row.​

Aggregate per‑game stats into per‑player per‑game averages to create a single feature vector per player‑position.​

Clean the raw position strings into four roles using a simple rule‑based function (simplifyposition).​

Train and evaluate two models:

Random Forest classifier.

XGBoost multi‑class classifier.​

The main notebook is main_xgboost.ipynb, which contains the full pipeline and final model. main.ipynb shows an earlier version focused on Random Forest.​

Data and features
From the merged stats, the model uses per‑player averages of numeric match stats as features, including:​

Disposals, kicks, marks, handballs

Goals, behinds, hit‑outs

Tackles, rebounds, inside 50s, clearances

Contested and uncontested possessions

Contested marks, marks inside 50

One‑percenters, bounces, goal assists

Time on ground (approximated via Played)​

The target is a simplified role label:

Defender

Midfield

Forward

Ruck

Multi‑position strings such as “Midfield, Forward” are mapped to a single role using a fixed priority order (Defender → Midfield → Forward → Ruck).​

Modeling approach
Split: stratified train/test split on players with 80% train and 20% test to preserve class balance.​

Preprocessing: standardize features using StandardScaler (fit on train, transform on test).​

Models:

Random Forest

Trained on scaled features with class balancing.

Achieves about 0.74 accuracy on the test set.​

XGBoost (final model)

XGBClassifier with:

n_estimators=200, learning_rate=0.05, max_depth=5

objective='multi:softmax', num_class=4

subsample=0.8, colsample_bytree=0.8

eval_metric='mlogloss', random_state=42​

Results
On the held‑out test set (345 players), the final XGBoost model achieves:​

Overall accuracy: ≈ 0.78

Per‑role performance:

Role	Precision	Recall	F1	Support
Defender	0.83	0.80	0.82	123
Forward	0.76	0.76	0.76	104
Midfield	0.76	0.76	0.76	101
Ruck	0.62	0.76	0.68	17
Compared to the Random Forest baseline (≈ 0.74 accuracy), XGBoost offers a clear improvement in overall accuracy and competitive performance across all four roles.​

Repository structure
main_xgboost.ipynb – Final end‑to‑end pipeline with XGBoost (recommended notebook to read first).​

main.ipynb – Earlier version with Random Forest baseline and first XGBoost experiments.​

players.csv – Player metadata (not included in the public repo if distribution is restricted).

games.csv – Match‑level information (not included if distribution is restricted).

stats CSV – Per‑game player stats (not included if distribution is restricted).​

If the original AFL dataset has licensing restrictions, only the notebooks and code are uploaded, and instructions are provided for users to obtain the raw data from the original source. This respects intellectual property and copyright of the data provider.​
