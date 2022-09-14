# Submission for [Geographic Named Entity Recognition challenge](https://github.com/1712n/challenge/issues/65)

The main idea of this solution is to use an ensemble of BERT models to extract geolocation from the text. Models such as BERT heavily rely on context, which is almost non-existent in this task, yet they are powerful enough to determine geolocations correctly in many cases. However, due to the lack of context, some tricks were also used. So, the solution is divided into several stages:

 - Cleaning: The text is cleared of unnecessary characters and emoji
 
 - Coordinates (like 50°36'24.5"S 165°58'21.2"E or 10.509633,-67.604186) are searched using regular expressions
 
 - Three models are applied: [mBERT base cased](https://huggingface.co/Davlan/bert-base-multilingual-cased-ner-hrl), [BERT base uncased](https://huggingface.co/dslim/bert-base-NER) and [mBERT cased wikineural](https://huggingface.co/Babelscape/wikineural-multilingual-ner).
 
 - If at least one model has found a geolocation, predictions are checked for inclusion in the set. At the moment, this is a set of cities (file cities.csv), although it could be also states, countries, etc.
 
 - If predictions not in the set, weighted decision is made. It depends on the number of models that found the geolocation, and whether the models predict the same result. See 'make_average_prediction' function docs for details.
 
 The result is stored in 'prediction.csv' file. It looks like this:
 
 ![](https://user-images.githubusercontent.com/77696343/190137628-923c1053-e99f-4954-a090-8b4ff6916a45.png)
 
To apply this pipeline to another dataset, just specify the correct paths after the libraries import section.
