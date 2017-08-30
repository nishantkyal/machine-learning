# Machine Learning Engineer Nanodegree
## Capstone Proposal
Nishant Kyal  
August 21st, 2017

## Proposal

### Domain Background

Music discovery is a challenge for almost everybody who loves music because of the sheer amount on music that is produced every day. Because there is so much music available, it's safe to assume that there's enough music available to suit everybody's tastes if only music could be categorized efficiently. There are a number of companies working on this but there is still a lot to be done.

Due to the very subjective nature of taste, it's hard to define metrics that will sufficiently capture a person's taste in music. Genre is a simple metric to define the kind of music which brings us closer to defining the kind of music in a song while cutting across other factors that influence choice of music like artist, country etc. 

### Problem Statement

The problem to solve is making music discovery simpler by analyzing existing labelled data as well as raw music files to identify music that sounds and feels similar. This can be used to build a system which can recommend new music given somebody's listening history and other feedback like likes/dislikes (direct feedback) or playback behavior (playing on repeat). For this I'll attempt to predict genre of a music file given past music data with genre assigned. This is a multi-label classification problem.

### Datasets and Inputs

I'll be using following publicly available datasets for the project: -

* Million song dataset (referred as MSD henceforth) - https://aws.amazon.com/datasets/million-song-dataset/ (citation: Thierry Bertin-Mahieux, Daniel P.W. Ellis, Brian Whitman, and Paul Lamere. The Million Song Dataset. In Proceedings of the 12th International Society for Music Information Retrieval Conference (ISMIR 2011), 2011.)

* Genre dataset (GenreDS) - http://www.tagtraum.com/msd_genre_datasets.html (citation: http://www.tagtraum.com/download/schreiber_msdgenre_ismir2015.pdf)

MSD contains metadata like artist, title as well as musical properties. Each track has segments for which musical properties like chroma features and MFCC-like timbre features are provided. Additional musical properties like beats, loudness, danceability are also provided.

### Solution Statement

The solution can be broken into following steps: -

* Data preparation: Extract relevant features and prepare a dataset for learning. Based on my research about the topic and dataset, segments features are intuitively very representative of kind of music because they encapsulate the timbre (indicative of instruments) and pitch (indicative of notes). Other features like loudness, beats, artist and danceability are also good features. We'll keep the artist out of feature set though it makes an assumption that an artist sticks to a genre always which is not true (http://www.rollingstone.com/music/news/10-artists-who-switched-genres-20130521).

* Since the number of features per track are going to be high, (each segment has 12 chroma and 12 MFCC features and there will be many segments per track), we'll train a CNN on the feature set with the known labels as targets. We'll not be able to augment the data by perturbing it because of the nature of the data (these are not actual audio samples but features extracted from original audio). This approach doesn't take into account order in which segments appear but I don't know how to factor that in. We can also try to add more data to the set using mentioned technique (https://labrosa.ee.columbia.edu/millionsong/pages/can-i-add-my-audio-dataset)

* Split train, validation and test data. 

* Success metric of the solution is performance on test set. It'll be important to disambiguate the targets since the GenreDS allows ambiguity (as per their documentation). 


### Benchmark Model

The problem of genre detection on the MSD has been attempted and solved before. Here are a few relevant benchmarks and projects that are relevant: -

* http://www.ee.columbia.edu/~dliang/files/FINAL.pdf

* https://github.com/CSTR-Edinburgh/mlpractical/blob/mlp2016-7/coursework3-4/notebooks/09b_Music_genre_classification_with_the_Million_Song_Dataset.ipynb

* http://dl.acm.org/citation.cfm?id=2348480 (don't have access but could contain benchmark information)

* https://www.researchgate.net/publication/254464824_Genre_classification_for_million_song_dataset_using_confidence-based_classifiers_combination

* https://www.quora.com/How-do-I-work-with-the-Million-Song-Dataset - This link describes a researcher's approach of using MSD to augment actual audio instead of using MSD as the only source.

Based on first paper, it is possible to get 28-35% accuracy by using lyrics information in conjugation with MSD.

### Evaluation Metrics
_(approx. 1-2 paragraphs)_

In this section, propose at least one evaluation metric that can be used to quantify the performance of both the benchmark model and the solution model. The evaluation metric(s) you propose should be appropriate given the context of the data, the problem statement, and the intended solution. Describe how the evaluation metric(s) are derived and provide an example of their mathematical representations (if applicable). Complex evaluation metrics should be clearly defined and quantifiable (can be expressed in mathematical or logical terms).

### Project Design
_(approx. 1 page)_

In this final section, summarize a theoretical workflow for approaching a solution given the problem. Provide thorough discussion for what strategies you may consider employing, what analysis of the data might be required before being used, or which algorithms will be considered for your implementation. The workflow and discussion that you provide should align with the qualities of the previous sections. Additionally, you are encouraged to include small visualizations, pseudocode, or diagrams to aid in describing the project design, but it is not required. The discussion should clearly outline your intended workflow of the capstone project.

-----------

**Before submitting your proposal, ask yourself. . .**

- Does the proposal you have written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Solution Statement** and **Project Design**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your proposal?
- Have you properly proofread your proposal to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
