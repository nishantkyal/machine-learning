# Machine Learning Engineer Nanodegree
## Capstone Proposal
Nishant Kyal  
August 31st, 2017

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

As suggested by the name, the MSD contains following fields for a million songs (https://labrosa.ee.columbia.edu/millionsong/pages/field-list). The tracks by nature are of varied lengths and each track has been broken into segments which are based on'quasi-stable music event' - roughly contiguous sections of the audio with similar perceptual quality. This leads to each track having a different number of segments. This makes the input data set variable in size. We can either choose to truncate data (keep songs with more than X segments and use only first X segments of each song) or bucket them into multiple datasets based on number of segments as descibed in (https://github.com/CSTR-Edinburgh/mlpractical/blob/mlp2016-7/coursework3-4/notebooks/09b_Music_genre_classification_with_the_Million_Song_Dataset.ipynb).

### Solution Statement

The solution can be broken into following steps: -

* Data preparation: Extract relevant features and prepare a dataset for learning. Based on my research about the topic and dataset, segments features are intuitively very representative of kind of music because they encapsulate the timbre (indicative of instruments) and pitch (indicative of notes). Other features like loudness, beats, artist and danceability are also good features. We'll keep the artist out of feature set though it makes an assumption that an artist sticks to a genre always which is not true (http://www.rollingstone.com/music/news/10-artists-who-switched-genres-20130521).

* Since the number of features per track are going to be high, (each segment has 12 chroma and 12 MFCC features and there will be many segments per track), we'll train a CNN on the feature set with the known labels as targets. We'll not be able to augment the data by perturbing it because of the nature of the data (these are not actual audio samples but features extracted from original audio). This approach doesn't take into account order in which segments appear but I don't know how to factor that in. We can also try to add more data to the set using mentioned technique (https://labrosa.ee.columbia.edu/millionsong/pages/can-i-add-my-audio-dataset)

* Split train, validation and test data. 

* Success metric of the solution is performance on test set. It'll be important to disambiguate the targets since the GenreDS allows ambiguity (as per their documentation). 


### Benchmark Model

The problem of genre detection on the MSD has been attempted and solved before. Here are a few relevant benchmarks and projects that are relevant: -

* http://www.ee.columbia.edu/~dliang/files/FINAL.pdf (citation: Music Genre Classification with the Million Song Dataset 15-826 Final Report by Dawen Liang,† Haijie Gu,‡ and Brendan O’Connor‡† School of Music, ‡ Machine Learning Department, Carnegie Mellon University , December 3, 2011)
Uses MSD only for tag information and extracts all musical information on it's own by using other methods like Genre-HMM combined with a bag of word on the lyrics to determine genre. They were able to achieve 28-35% accuracy on genre detection using a combination of features. They use tags specified in the MSD while I think they should have looked at other sources for better tag information as the tags in MSD are for artists and not songs.

* https://github.com/CSTR-Edinburgh/mlpractical/blob/mlp2016-7/coursework3-4/notebooks/09b_Music_genre_classification_with_the_Million_Song_Dataset.ipynb
This is part of Edinburgh University coursework and uses two sources of genre tags in conjugation with MSD. This is run as private contest on Kaggle and results are not available. The methdology for generating the feature set is described which is something we can reuse here and adding more relevant features to the set which seem to have been missed.

* http://dl.acm.org/citation.cfm?id=2348480 (citation: Yajie Hu and Mitsunori Ogihara. 2012. Genre classification for million song dataset using confidence-based classifiers combination. In Proceedings of the 35th international ACM SIGIR conference on Research and development in information retrieval (SIGIR '12). ACM, New York, NY, USA, 1083-1084. DOI: https://doi.org/10.1145/2348283.2348480)

This approach uses a combination of classifiers like SVM, NN and Naive-Bayes to achieve a 83.66% accuracy. Each classifier is trained on a different set of features and classfiers are combined based on classification authority and confidence.

* There are other attempts although on different datasets
https://www.mitacs.ca/en/projects/automatic-genre-detection-intelligent-audio-tools
https://www.researchgate.net/publication/3333877_Musical_genre_classification_of_audio_signals_IEEE_Trans_Speech_Audio_Process
I don't have access to this material and haven't used hence not cited.


### Evaluation Metrics

The project will be evaluated on accuracy achieved on genre classification. The best benchmark available is able to predict genres with an accuracy of 83.66%. The benchmark models listed above differ in feature set as well as models used to classify genres. The findings of the experiment will be compared to the benchmarks subjectively and I'll try to come up with reasons why a certain model performs better compared to others.

### Project Design

The project will be broken into following steps: -

* Data preparation
    * Extract relevant features from MSD. Relevant features list based on initial intuition is segment, beats, loudness, danceability, energy, tempo, artist, year of release. Artist and year will be one hot encoded. Segment comprise two 12 dimensional vectors of chroma and timbre features.
    * Extract labels for each track from GenreDS. According to documentation, the GenreDS has multiple tags to allow for ambiguity since they're compiled from various other data sources. We'll have to define how we disambiguate and assign one genre label to each track.

* Data analysis
We'll first analyze data to see if there is correlation between features that we've included by intuition like year, artist. If no correlaton is found, they can be eliminated from further processing.

* Define train, test and validation data sets
Once dataset is available, we'll separate it into train, validation and test sets. 

* Design a CNN
The network will comprise several convolution layers followed by pooling layers and then fully connected layers. It'll finally terminate in softmax classifiers (size equal to number of labels ~15)
By intuition, following features define the genre characteristics of music. Pitch (notes played), timbre (instruments used), loudness, beats (percussion) and danceability (could be a function of tempo and/or beats). So I'll start with 4 convolution layers expecting each of them to learn one each of the above features.
Artist and year is found relevant will be used to train a supervised learning classifier independently and results of the two experiments combined to define the final prediction.


-----------

**Before submitting your proposal, ask yourself. . .**

- Does the proposal you have written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Solution Statement** and **Project Design**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your proposal?
- Have you properly proofread your proposal to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
