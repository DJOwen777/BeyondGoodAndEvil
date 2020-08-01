Project: Beyond Good and Evil 
Student: Dakota Owen 
Student Number: 15200586
Supervisor: Tony Veale

SETUP 

While the full Hatebase API is included in the repository for easy use, if you wish to redownload the Hatebase API yourself you will need to contact them in order to be given a API key which you can use to access the API. Due to my license with them, I am sadly not able to publish my own downloaded copy of their data to a public service like GitHub. The rest of this project is able to be published however. 

To use the Stanford NLP Client the repository uses, you will need to do the following: 
1. Have Java installed on your system and the Path environment variable pointing to where it is installed.  
2. Install the Levenshtein module to your Python environment with: pip install python-Levenshtein
3. If you wish to download Davidson’s test data yourself instead of use the included copy, you can do so at his GitHub repository (note: it is no longer actively maintained):
https://github.com/t-davidson/hate-speech-and-offensive-language 
4. While the amplifiers list has been custom made for this project, a large portion of it comes from a list of swear words located here: 
https://www.noswearing.com/dictionary
5. If you do not wish to use the CoreNLP included in the repository or you wish to update it, you will need to download the latest Stanford CoreNLP from: 
https://stanfordnlp.github.io/CoreNLP/download.html 
6. If you wish to use a GPU to assist the client, you will need to install CUDA Python from: 
https://developer.nvidia.com/how-to-cuda-python 

DESCRIPTION 

While all possible prerequisite data is included, so is code to re-obtain it if you wish.  The Code is broken into three notebooks: 

Hatebase API Download: 

This notebook contains all the code you will need to download the complete Hatebase API if you have obtained an API key.  It loops through the pages of results, recording the JSON for each term along the way. 
NOTE: There is a query limit of 100 queries per 24 hours, and while the entire API can be obtained in approximately 18 (depending on if the size of the API has changed since this was written), the limit should still be mentioned.  
The JSON recorded is then converted into a usable .csv format for later use.  
Once in a .csv format, the notebook splits the database into two easily usable csv files based on the ambiguity of each term from the API. This split is important for ease of use later. 
The final part of the notebook also trims the amplifier list to make sure there are no duplicate terms already contained in the data recorded from the Hatebase API. 

DavidsonData: 
This notebook takes the data obtained from Davidson’s work (which includes nearly 25,000 sample tweets and crowd ratings for each split into hate speech, offensive speech, or neither) and cleans it. 
The code removes all user tags from twitter, ascii codes for emojis, as well as some discrepancies unique to these samples.  The cleaned data is then exported to a csv to be used as our samples.  
The end of the notebook splits the data into a few smaller sample sizes, largely based on what category the data was originally given in Davidson’s work by the crowd workers. 

Core NLP Raw: 
This notebook is the meat of the project.  First are some custom functions used to save lines of code, as well as make things simpler to understand. 
Next all relevant data is read in and constants are initialized for use in the NLP Client. 
Then we run the client.  Here the current message is scanned for offensive words, and any dependency tags related to those words that the client discovers are recorded.  Information for the offending words are taken and analysed and checked for how offensive they are, as well as determining if the detected dependencies include amplifiers which imply an even higher offensiveness. 
Based on the results a message is returned to the user, describing how offensive the message is, and what the offensive content is targeting.  Depending on the intensity of the hate speech detected, the message is either determined to be free of hate speech and printed as normal, somewhat hateful and the offending words are replaced, or so egregious that the entire message is blocked.  
Finally the results of all tested messages are recorded and exported to a csv for records and ease of analysis.  
