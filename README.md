# NFL Hall of Fame Classification with Career Statistics
![evqjec2dffncakad3gd5](https://user-images.githubusercontent.com/113449546/202634037-de293f0a-1ccf-468f-83ed-c08f41e4a0f0.jpg)
### John Harrigan and Evan Staffen
# Business Understanding
**Stakeholder:** Agents of recently retired NFL players.
The ultimate recognition for an NFL player is to be inducted into the Hall of Fame. An NFL player is eligible for nomination five years after retirement. Only 1.2% of NFL players can call themselves Hall of Famers. While the avid football fan knows more than just this small fraction of players, Hall of Famers are the household names. They have influence beyond their playing days and way outside the sphere of football.
Michael Strahan, former defensive back for the New York Giants is a host on the talk-show 'Live with Michael and Kelly'. Troy Polamalu, one of the all-time great safeties for the Pittsburgh Steelers, is still seen in Head & Shoulder's ads because of his famous, curly hair. Peyton Manning may be one of the prime examples of this. He was just the host of the Country Music Awards, acted in the animated movie 'Ferdinand' and has endorsement deals with various companies.
We set out to create a model that can determine whether or not a player will make it to the Hall of Fame based on their career statistics. Ultimately, NFL player's agents could use our model to predict the likelihood of the players they represent to go to the hall of fame, and therefore maximize their potential value.
> What are the most important metrics for determining whether a player will make it into the hall of fame?
> Who is near the the hall of fame status that is not up for nomination yet that should be targeted?
# Datasets
Multiple datasets from different sources were used for this analysis. All of the datasets can be accessed from the data folder located within this repository.
Two datasets from __[Kaggle](https://www.kaggle.com/datasets/zynicide/nfl-football-player-stats)__ were used. One had basic demographics for 25K+ NFL players from 1950-2017. The other had indvidual game data for each player in the former dataset, with just above 1 million rows.
7 separate datasets were taken from __[Profootballreference's database](https://www.pro-football-reference.com/)__ so we could add the individual accolades for each player. 6 of these datasets were used to compile the MVP, DPOY, DROY, OPOY, OROY, Superbowl MVPs of each year into a new column which was the sum of everyone of those awards a player won in their career. The last dataset allowed us to create a column for our target, whether a player made the hall of fame as 1 or they didn't make the hall of fame as 0.
Individual player data from 2017-2022 was scraped from __[GridIronAI's database](https://gridironai.com/football/)__ and merged with the Kaggle dataset so we had career statistics for all players from 1950-2022.
# Data Understanding and Visualization
> ![HOFbyTeam](https://user-images.githubusercontent.com/113449546/202628147-2ce3b61f-e509-4a7b-91d7-07cd54e8264d.png)
As mentioned above, only 1.2% of players make it to the hall of fame. Above the team with the highest percentage of hall of famers is the Cleveland Browns with 1.74%.
> ![HOFWin%](https://user-images.githubusercontent.com/113449546/202628174-e306f2aa-aaea-4f13-a076-0cac885017d3.png)
Although hall of famers tend to have an average win percentage approximately 10 points higher than the non-hall of famers, there is clearly a significant overlap in their win percentages.
> ![PosHOF%](https://user-images.githubusercontent.com/113449546/202628093-9a14acf4-e766-4767-8bb5-c30a7ebc54fd.png)
It is not surpirsing that quarterbacks and playmakers (runningbacks, wide receivers and tight ends) all have the highest percentage of players going into the hall of fame. While it is surprising that defensive players make up approximately 1/3 of the hall of famers, they also clearly get inducted at a much lower rate.
> ![AwardsPerPlayer](https://user-images.githubusercontent.com/113449546/202628228-204a3cd3-42a0-46fa-b2c3-3de90052a66b.png)
The career accolades gathered such as MVPs, Offensive and Defensive player of the year etc. all undoubtedly play an important role in hall of fame determination. While players with one career award are unlikely to make it into the hall of fame, after one award the odds seem to swing in the players' favor.
> ![PassTDHOF](https://user-images.githubusercontent.com/113449546/202737279-a8702cf3-aa43-4500-b9a6-a769f5486513.png)
> ![RushingTDHOF](https://user-images.githubusercontent.com/113449546/202737384-124185ae-bc5a-4294-a31d-c92eab5237c5.png)
> ![ReceivingTDHOF](https://user-images.githubusercontent.com/113449546/202737320-1ff359f6-d27d-4269-ba35-95eaa8ac509a.png)
As expected, hall of famers clearly produce more touchdowns than their non hall of fame counterparts. Quarterbacks in the hall of fame have a slightly more normal distribution in their touchdowns when compared to non hall of famers. Rushing and receiving touchdowns have a much wider spread amongst the hall of famers. In all these cases, non hall of famers clearly score much less touchdowns than hall of famers.
# Modeling
Since the hall of fame players only make up 1.2% of the dataset, we knew we would be having to acconut for this class imbalance in our models. As well, that inherently means that we want a model that does better than picking a player to not go to the hall of fame 98.8% of the time. All the models created utilized imblearn's SMOTE in order to help account for the severe imbalance. As well, every model used GridSearchCV with a 3 or 5-fold cross-validation to find the best parameters that were then run on the test set.
We decided to focus on maximizing recall with our models, whick is a measure that tries to minimize the number of false negative results we obtain. A false negative in this case would be players who the model predicts to not go to the hall of fame, even though they do. Ultimately, the cost of missing out on a hall of fame caliber player is much worse than taking a chance on one that does not pan out.
1. Baseline Logistic Regression
2. Basic Logistic Regression with SMOTE
3. Logistic Regression with Best Parameters
4. Random Forest Classifier
5. XGBoost Classifier
6. C-SVC
7. KNN (K-Nearest Neighbors)
# Evaluation
## Model Performance and Evaluation
While the basic logistic regression was extremely accurate, it still suffered from a high false negative rate. After applying SMOTE to the model, even though the accuracy decreased by around 6 percentage points, the recall improved significantly. Using GridSearch led to a decreased testing recall in the logistic regression model. The XGBoost model had even worse recall than all the logistic regression models. The SVC model was comparable to the Logistic Regression model, whereas the Random Forest and KNN models clearly were overfitting. We ultimately decided that the SVC model was the best model.
| Model | Training Accuracy | Testing Accuracy | Training Recall | Testing Recall |
| --- | --- | --- | --- | --- |
| Basic Logistic Regression | 0.991 | 0.990| 0.319 | 0.338 |
| Logistic Regression + SMOTE | 0.927 | 0.927| 0.938 | 0.825 |
| Logistic Regression with GridSearch | 0.928 | 0.928| 0.931 | 0.794 |
| XGBoost | 0.990 | 0.989 | 0.269 | 0.221 |
| SVC | 0.925 | 0.923 | 0.975 | 0.809 |
| Random Forest | 0.971 | 0.962 | 0.900 | 0.559 |
| KNN | 0.984 | 0.968 | 1.0 | 0.485 |
Below is a table of the top 10 features from the SVC model. Unsurprisingly, the offensive positions and their statistics seem to be the most important feature for determining the hall of fame status. Weight came back as the second most important factor when determining hall of fame status, which could be explained by there being many defenders and offensive linemen in the hall of fame, who are generally much larger than the average player. Although the strongest correlated predictor as draft year is definitely a surprising outcome and is one that is worth further investigation in the future.
| Coefficients | Column |
| --- | --- |
| 0.849191	| position_QB |
| 0.897590 | total_games |
| 0.923156 | position_PM |
| 0.955432 | punt_return_attempts |
| 1.018602 | punt_return_yards |
| 1.422173 | receiving_yards |
| 1.476975 | kick_return_yards |
| 1.488679 | kick_return_attempts |
| 1.553485 | weight |
| 1.834826 | draft_year |
# Conclusions & Recommendations
## Model Limitations:
One shortcoming of the model is that we tried to create a model that could use any player’s career statistics at any position, by generalizing the position category in the dataset itself into six categories, and then predict their hall of fame status. Ultimately, we could obtain better predictions by creating separate models for players at different groups of positions. For instance, one model could have been made for quarterbacks, one for running backs, wide receivers and tight ends, one for kickers and punters, one for offensive linemen and one for defenders. Since these positional categories career statistics are more easily comparable, they would have significantly improved predictive power.
There are some additions we would like to make to the model in the future. For instance, it is not solely a player’s statistics that determines their likelihood to get into the hall of fame. There are multiple factors such as a player’s popularity or likability, their criminal history or controversial past, or even some other awards we did not consider like the Walter Payton Man of the Year Award, given to a player for their philanthropic endeavors. For example, a player like Antonio Brown would likely be predicted to make it into the Hall of Fame, even though his off the field antics have certainly stripped him of that possibility. Alex Smith, a quarterback who had a devastating, career-threatening leg injury and captured fans hearts by miraculously returing to play again, would have little chance of getting into the Hall of Fame on our model. Another interesting addition would be NCAA Football statistics for all NFL players to determine if there is any significance between things like their college statistics or colleges attended on their Hall of Fame status.
## Recommendations:
> Agents should generally be looking at their players that are either playing quarterback or one of the playmaker positions
> Players who specialize as kick returners on top of their other offensive abilities have a higher chance of making it into the hall of fame
# Next Steps
#### 1. We recommend trying to include data on College Football statistics to incorporate into the model.
#### 2. Incorporate data on player characteristics/popularity.
#### 3. Create different metrics and models to evaluate players by position.
# Repository Structure
    ├── Data
    ├── Images
    ├── .gitattributes
    ├── .gitignore
    ├── NFL Hall of Fame Predictions.pdf
    ├── Hall of Fame Analysis.ipynb
    └── README.md
