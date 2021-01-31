# speech_analsis_merkel_steinmeir

DESCRIPTION:

In this repository, you will find the analyzes of the press release of some political parties in Germany Also, you will find models to predict Which press release belongs to which political party

DATA:

Speeches of Frau Merkel: https://www.bundeskanzlerin.de/bkin-de/aktuelles/866580!search

Speeches of Herr Steinmeir: https://www.bundespraesident.de/DE/Bundespraesident/Reden-und-Interviews/Reden/reden-node.html

examples of some important used methods:

#giving unkown label for unkown localtion
	steinmeier['meta'] = steinmeier.meta.apply(lambda x: 'Unkown, ' + x.split(', ')[0] if len(x.split(', ')) == 1 else x)

#obtaining month values from meta column
	steinmeier['month']= steinmeier.meta.apply(lambda x: x.split(',')[1].split(' ')[1])

#pivot table for months and years
	table = steinmeier.groupby(['year','month']).count().pivot_table(columns=['month'], index=['year'], aggfunc='sum')['meta']


#trying to determine best split point by chosen value
	merkel[merkel.meta.apply(lambda x: True if len(x.split('\nOrt:\n'))==2 else False )]

#obtaining places where Frau Merkel gave speeches from Meta column, and given Unkown otherwise
	merkel['place']=merkel.meta.apply(lambda x: 'Unkown' if '\nOrt:\n' not in x else x.split('\nOrt:\n')[1])

#using feature importance for nlp	
	feature_imp = pd.Series(model_log.coef_[0],
                        index=vectorizer.get_feature_names()).sort_values(ascending=False).head(10)
	sns.barplot(x=feature_imp, y=feature_imp.index)
	plt.title("Feature Importance Logistic Reg")
	plt.show()


Some insights:
1. In order to access data I used Selenium

2. Since time of being in power is different for two politicians, the volume of data for each politicians was not same

3. For each speech there are some sentences as salutations. That's why I preferred to drop them.

4. According to collected Meta data for each politician;
		- Both leader gave less speeches in 2020 than before, it may be because of Corona. 

		- While Herr Steinmeir gives his speeches mosthly in Juni and October, Frau Merkel gives in Mai and Novenmber. Moreover, the number of her speeches has increased from Monday to Wendsday, and then it decreased stedealy. 

		- After Berlin, Herr Steinmeir frequently gives  his speeches as following places; Hamburg, München, Frankfurt am Main. On the other hand Merkel gives her speechs after Berlin and Unkown places, Hannover, Frankfurt am Main, München. 
		
		-Moreover, Herr Steinmer made 5 speeches in Poland, 4 in Itaily, 3 in Spain, USA, and Kazakhistan, whereas Frau Merkel made 10 in New York, 7 in Davos, 6 in Nairobi, and Pekin

5. Speeches of Frau Merkel is longer than Herr Steinmeir

6. Overall, Logistic Regression predicts better than Random and Xbost Classeifrs

7. Predection for the whole textes was more than 90%, whereas it was reduced around 80%.

8. When Word Embeding method is used;
		- The context of speeched shows many insights; for example,the closest three word to "Demoratie"(democracy) is "Gesellschaft"(society), "mitzureden"(speaking together), and "Verantwortung"(responsibility)

		- We can alsa apply word calculation; for example if we extract democracy from germany and add power, the model predict exil. 

Limitation & Further:

1. With more data, and deployment of different models better results can be obtained for sentence prediction.
2. Dropping more unnecessary and repeated words might also provide better results