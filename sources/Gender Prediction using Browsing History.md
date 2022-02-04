This is a paper that does something rather interesting to obtain a totally biased result.

The take-away is related to how the authors perform the [[feature preprocessing]] of browsing history in order to fit a [[binary classifier]].

- Do Viet Phuong, Tu Minh Phuong. _Gender Prediction using Browsing History_. 2014. [paper](https://www.researchgate.net/publication/278399485_Gender_Prediction_Using_Browsing_History).

## Notes

### Feature engineering

- Browsing category (o bé ja la tenen, o fan [[Tf-Idf]] + [[Naive Bayes]] sobre el contingut amb un classificador ja entrenat)
    
- Topic modelling dels continguts ([[LDA|LDA]])
    
- Time features (només hora però en algun lloc també he vist que hi posen el dia de la setmana)
    
- Sequential features (a partir de [[n-grams]] de webs visitades consecutivament + mutual information)

Cada vector de features és __normalitzat__.


### Machine Learning

-   [[SVM]] amb varis sabors
-   [[Grid search]]
-   Evaluació amb [[F1-score]].