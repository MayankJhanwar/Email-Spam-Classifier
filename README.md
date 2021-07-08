# Email Spam Classifier

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Spamming is one of the major attacks that accumulate many compromised machines by sending unwanted messages, viruses, and phishing through E-Mails. It conveys the principal aim behind choosing this project under with. There are many people who try to fool you, just by sending you fake e-mails! Some of them have the messages like - You have won 1000 dollars, or we have deposited this much amount to your account. Once you open this link, then they will track you and try to hack your information. **The scope for this project is to identify all spam e-mails with the help of Machine Learning Algorithms.**

## Project Features

Major highlights of our product’s functionality are: 
 - This product will help identify Spam E-Mails similar to the Spam encountered earlier, which are stored in a vast library of Spam E-Mails.   
 - This product will also help in identifying new Potential Spam E-Mails from known & unknown sources. This what is going to  be a speciality of our product.

## Making Project Functional (Linking Machine Learning Model with Frontend)

This section discusses in detail, about how we made our Project Functional with Graphical User Interface: 
 - **Front-end:** The project's GUI has been made through web pages written in HTML and design elegantly with CSS and JavaScript for it to be attractive. 
 - **Back-end Framework**: Flask helps in implementing a machine learning application in Python that can be easily plugged, extended and deployed as a web application. 
 - **Machine Learning Model:** The model detects, if a text message/mail is spam(1) or ham(0) using techniques like tokenizaton, multinomial naive bayes classifier, etc. 
 - **Web Server**: All these functional units are connected by establishing a server on the web by hosting it on Python Everywhere, which helps us in running the project successfully.

## Embedding Flask into app.py

#### Step 1: Importing libraries

    from flask import Flask,render_template,url_for,request
    import pandas as pd 
    import pickle from sklearn.feature_extraction.text import CountVectorizer 
    from sklearn.naive_bayes import MultinomialNB 
    import joblib

#### Step 2: Instantiate Flask

    app = Flask (__name__)

#### Step 3: Setting up Routes

    @app.route('/') 
    def home(): 
        return render_template('index.html')
    
    @app.route('/predict',methods=['POST'])
    def predict(): 
	    df= pd.read_csv("spam_ham_dataset.csv") 
	    df_data = df[["text","label_num"]]
	
	#Features and Labels
	df_x = df_data['text'] 
	df_y = df_data.label_num
	
	#Extract Feature With CountVectorizer 
	corpus = df_x 
	cv = CountVectorizer() 
	X = cv.fit_transform(corpus) # Fit the Data 
	from sklearn.model_selection import train_test_split 
	X_train, X_test, y_train, y_test = train_test_split(X, df_y, test_size=0.33, random_state=42)
	
	#Naive Bayes Classifier 
	from sklearn.naive_bayes import MultinomialNB 
	clf = MultinomialNB() 
	clf.fit(X_train,y_train) 
	clf.score(X_test,y_test)
	
	#Alternative Usage of Saved Model 
	#ytb_model = open("spam_mail_detection.py","rb") 
	#clf = joblib.load(ytb_model)

#### Step 4: Code to display results on Results.html

    if request.method == 'POST':
	    comment = request.form['comment'] 
	    data = [comment] 
	    vect = cv.transform(data).toarray() 
	    my_prediction = clf.predict(vect) 
	
	return render_template('results.html',prediction = my_prediction)

#### Step 5: Run the Module

    if __name__ == '__main__': 
    app.run(debug=True

#### Step 6: Implement Index.html

  

    <div class="get-start-area">
        <!-- Form Start -->
    		    <form action="{{ url_for('predict')}}" method="POST" class="form-inline">
    		    <textarea name="comment" class="form-control" id="comment" cols="50" rows="4"
		    	 placeholder="Enter Text Message*" required></textarea>
    		    <input type="submit" class="submit" value="Check Spam or Not!">
    		    </form>
        <!-- Form End --> 
    </div>

The key to note is the action attribute. It is set to **"{{ url_for('predict')}}".** So, when a user enters an E-mail and submits it. The POST method stores it in the **name** attribute named "comment" in the above html code & then passes it to the render_template() function.

#### Step 7: Implement Results.html

    {% if prediction == 1%}
    ## Spam
    {% elif prediction == 0%}
    ## Not a Spam!
    {% endif %}

The prediction containers will display the predicted spam or not spam result. These values will be passed in via the render_template() function in the app.py file.

#### Step 8: Run the Application to Localhost
Now, go to the project directory in the CMD and type:

    python app.py

Our model is now running on a web browser on our localhost. The last task is to deploy it on an external server so that the public can access it. For this Project we have, hosted our project on pythonanywhere.com, by uploading the project folders on the web console in the structure mentioned and by installing all the libraries on the bash console. This project can now be accessed by visiting the following url:

> https://emailspamclassifier.pythonanywhere.com/

## Project Members

**Mayank Jhanwar**

*B.Tech | Computer and Communication Engineering | Manipal University Jaipur
mayank.189303179@muj.manipal.edu*

**Aarushi Goyal**

*B.Tech | Electronis and Communication Engineering | IGDTUW Delhi
goyalaarushiag@gmail.com*

**Namit Varshney**

*B.Tech | Computer Science Engineering | Manipal University Jaipur
namit.189303182@muj.manipal.edu*
