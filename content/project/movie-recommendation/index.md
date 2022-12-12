+++
date = 2022-11-06T18:30:00Z
external_link = ""
summary = "In Jupyter notebook using Python and Pandas"
tags = ["Pandas", "Python", "Recommendation-System", "Data-Analysis"]
title = "Movie Recommendation System"
url_code = "https://github.com/tanmaychk/movies-rec"
url_pdf = ""
url_slides = ""
url_video = ""
[image]
caption = "Screengrab by Tanmay"
focal_point = "Smart"

+++
Used Python and Pandas to create a scoring system that displays movie commendations based on comparable users. Used the 25m dataset provided by the Movie Lens and used the Ipywidgets library. The solution first finds the Term Frequency and Inverse document frequency (TFIDF) matrix using scikit-learn. It then compares the searched movie title to available titles using Cosine distance based collaborative filtering. Lastly, it finds similar users who liked the same movie and presentls the top-10 results.
