Assignment 3 - Part 1 - Assessing voice in schizophrenia
--------------------------------------------------------

Individuals with schizophrenia (SCZ) tend to present voice
atypicalities. Their tone is described as “inappropriate” voice,
sometimes monotone, sometimes croaky. This is important for two reasons.
First, voice could constitute a direct window into cognitive, emotional
and social components of the disorder, thus providing a cheap and
relatively non-invasive way to support the diagnostic and assessment
process (via automated analyses). Second, voice atypicalities play an
important role in the social impairment experienced by individuals with
SCZ, and are thought to generate negative social judgments (of
unengaged, slow, unpleasant interlocutors), which can cascade in more
negative and less frequent social interactions.

Several studies show *significant* differences in acoustic features by
diagnosis (see meta-analysis in the readings), but we want more. We want
to know whether we can diagnose a participant only from knowing the
features of their voice.

The corpus you are asked to analyse is a relatively large set of voice
recordings from people with schizophrenia (just after first diagnosis)
and matched controls (on gender, age, education). Each participant
watched several videos of triangles moving across the screen and had to
describe them (so you have several recordings per person). We have
already extracted the pitch once every 10 milliseconds as well as
several duration related features (e.g. number of pauses, etc).

N.B. For the fun of it, I threw in data from 3 different languages: 1)
Danish (study 1-4); 2) Mandarin Chinese (Study 5-6); 3) Japanese (study
7). Feel free to only use the Danish data, if you think that Mandarin
and Japanese add too much complexity to your analysis.

In this assignment (A3), you will have to discuss a few important
questions (given the data you have). More details below.

*Part 1 - Can we find a difference in acoustic features in
schizophrenia?* 1) Describe your sample number of studies, number of
participants, age, gender, clinical and cognitive features of the two
groups. Furthemore, critically assess whether the groups (schizophrenia
and controls) are balanced. N.B. you need to take studies into account.

1.  Describe the acoustic profile of a schizophrenic voice: which
    features are different? E.g. People with schizophrenia tend to have
    high-pitched voice, and present bigger swings in their prosody than
    controls. N.B. look also at effect sizes. How do these findings
    relate to the meta-analytic findings?

2.  Discuss the analysis necessary to replicate the meta-analytic
    findings Look at the results reported in the paper (see
    meta-analysis in the readings) and see whether they are similar to
    those you get. 3.1) Check whether significance and direction of the
    effects are similar 3.2) Standardize your outcome, run the model and
    check whether the beta’s is roughly matched (matched with hedge’s g)
    which fixed and random effects should be included, given your
    dataset? E.g. what about language and study, age and gender? Discuss
    also how studies and languages should play a role in your analyses.
    E.g. should you analyze each study individually? Or each language
    individually? Or all together? Each of these choices makes some
    assumptions about how similar you expect the studies/languages to
    be. *Note* that there is no formal definition of replication (in
    statistical terms).

Your report should look like a methods paragraph followed by a result
paragraph in a typical article (think the Communication and Cognition
paper)

*Part 2 - Can we diagnose schizophrenia from voice only?* 1) Discuss
whether you should you run the analysis on all studies and both
languages at the same time You might want to support your results either
by your own findings or by that of others 2) Choose your best acoustic
feature from part 1. How well can you diagnose schizophrenia just using
it? 3) Identify the best combination of acoustic features to diagnose
schizophrenia using logistic regression. 4) Discuss the “classification”
process: which methods are you using? Which confounds should you be
aware of? What are the strength and limitation of the analysis?

Bonus question: Logistic regression is only one of many classification
algorithms. Try using others and compare performance. Some examples:
Discriminant Function, Random Forest, Support Vector Machine, Penalized
regression, etc. The packages caret and glmnet provide them. Tidymodels
is a set of tidyverse style packages, which take some time to learn, but
provides a great workflow for machine learning.

Learning objectives
-------------------

-   Critically design, fit and report multilevel regression models in
    complex settings
-   Critically appraise issues of replication

Overview of part 1
------------------

In the course of this part 1 of Assignment 3 you have to: - combine the
different information from multiple files into one meaningful dataset
you can use for your analysis. This involves: extracting descriptors of
acoustic features from each pitch file (e.g. mean/median, standard
deviation / interquartile range), and combine them with duration and
demographic/clinical files - describe and discuss your sample - analyze
the meaningful dataset to assess whether there are indeed differences in
the schizophrenic voice and compare that to the meta-analysis

There are three pieces of data:

1- Demographic data
(<a href="https://www.dropbox.com/s/6eyukt0r5du0xif/DemoData.txt?dl=0" class="uri">https://www.dropbox.com/s/6eyukt0r5du0xif/DemoData.txt?dl=0</a>).
It contains

-   Study: a study identifier (the recordings were collected during 6
    different studies with 6 different clinical practitioners in 2
    different languages)
-   Language: Danish, Chinese and Japanese
-   Participant: a subject ID
-   Diagnosis: whether the participant has schizophrenia or is a control
-   Gender
-   Education
-   Age
-   SANS: total score of negative symptoms (including lack of
    motivation, affect, etc). Ref: Andreasen, N. C. (1989). The Scale
    for the Assessment of Negative Symptoms (SANS): conceptual and
    theoretical foundations. The British Journal of Psychiatry, 155(S7),
    49-52.
-   SAPS: total score of positive symptoms (including psychoses, such as
    delusions and hallucinations):
    <a href="http://www.bli.uzh.ch/BLI/PDF/saps.pdf" class="uri">http://www.bli.uzh.ch/BLI/PDF/saps.pdf</a>
-   VerbalIQ:
    <a href="https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale" class="uri">https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale</a>
-   NonVerbalIQ:
    <a href="https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale" class="uri">https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale</a>
-   TotalIQ:
    <a href="https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale" class="uri">https://en.wikipedia.org/wiki/Wechsler_Adult_Intelligence_Scale</a>

1.  Articulation.txt
    (<a href="https://www.dropbox.com/s/v86s6270w39g0rd/Articulation.txt?dl=0" class="uri">https://www.dropbox.com/s/v86s6270w39g0rd/Articulation.txt?dl=0</a>).
    It contains, per each file, measures of duration:

-   soundname: the name of the recording file
-   nsyll: number of syllables automatically inferred from the audio
-   npause: number of pauses automatically inferred from the audio
    (absence of human voice longer than 200 milliseconds)
-   dur (s): duration of the full recording
-   phonationtime (s): duration of the recording where speech is present
-   speechrate (nsyll/dur): average number of syllables per second
-   articulation rate (nsyll / phonationtime): average number of
    syllables per spoken second
-   ASD (speakingtime/nsyll): average syllable duration

1.  One file per recording with the fundamental frequency of speech
    extracted every 10 milliseconds (excluding pauses):
    <a href="https://www.dropbox.com/sh/b9oc743auphzxbg/AAChUsvFc6dIQSlM9eQTL53Aa?dl=0" class="uri">https://www.dropbox.com/sh/b9oc743auphzxbg/AAChUsvFc6dIQSlM9eQTL53Aa?dl=0</a>

-   time: the time at which fundamental frequency was sampled
-   f0: a measure of fundamental frequency, in Herz

NB. the filenames indicate: - Study: the study, 1-6 (1-4 in Danish, 5-6
in Mandarin Chinese) - D: the diagnosis, 0 is control, 1 is
schizophrenia - S: the subject ID (NB. some controls and schizophrenia
are matched, so there is a 101 schizophrenic and a 101 control). Also
note that study 5-6 have weird numbers and no matched participants, so
feel free to add e.g. 1000 to the participant ID in those studies. - T:
the trial, that is, the recording ID for that participant, 1-10 (note
that study 5-6 have more)

### Getting to the pitch data

You have oh so many pitch files. What you want is a neater dataset, with
one row per recording, including a bunch of meaningful descriptors of
pitch. For instance, we should include “standard” descriptors: mean,
standard deviation, range. Additionally, we should also include less
standard, but more robust ones: e.g. median, iqr, mean absoluted
deviation, coefficient of variation. The latter ones are more robust to
outliers and non-normal distributions.

Tip: Load one file (as a sample) and: - write code to extract the
descriptors - write code to extract the relevant information from the
file names (Participant, Diagnosis, Trial, Study) Only then (when
everything works) turn the code into a function and use map\_df() to
apply it to all the files. See placeholder code here for help.

``` r
library(pacman)
p_load(tidyverse)

# Setting working directory
setwd("C:/Users/louis/OneDrive - Aarhus universitet/AU Onedrive - RIGTIG/- 3. Semester/Experimental Methods III/Classes/Assignment-3")

read_pitch <- function(filename) {
    # load data
    filename <- paste("Pitch/", filename, sep="")
    pitch <- read_tsv(filename)
    # parse filename to extract study, diagnosis, subject and trial
    Study <- filename %>% str_extract("Study\\d") %>% gsub("Study", "", .)
    Diagnosis <- filename %>% str_extract("D\\d") %>% gsub("D", "", .)
    Participant <- filename %>% str_extract("S\\d+") %>% gsub("S", "", .)
    Trial <- filename %>% str_extract("T\\d+") %>% gsub("T", "", .)
    # extract pitch descriptors (mean, sd, iqr, etc)
    Mean <- mean(pitch$f0)
    IQR <- IQR(pitch$f0)
    SD <- sd(pitch$f0)
    # combine all this data in one dataset
    return(tibble(Study, Participant, Trial, Diagnosis, Mean, IQR, SD))
}

# test it on just one file while writing the function
# test_data <- read_pitch("Pitch/Study1D0S101T1_f0.txt")

# when you've created a function that works, you can
pitch_data <- list.files(path = "Pitch/",pattern = ".txt") %>% purrr::map_df(read_pitch)
```

    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )
    ## Parsed with column specification:
    ## cols(
    ##   time = col_double(),
    ##   f0 = col_double()
    ## )

``` r
# Writing csv
write_csv(pitch_data, "pitch_data.csv")
```

### Now you need to merge demographic/clinical, duration and pitch data

``` r
# Let's start with the demographic and clinical data
demo <- read_delim("DemographicData.csv", delim = ";")
```

    ## Parsed with column specification:
    ## cols(
    ##   Study = col_double(),
    ##   Language = col_character(),
    ##   Diagnosis = col_character(),
    ##   Participant = col_double(),
    ##   Gender = col_character(),
    ##   Age = col_double(),
    ##   Education = col_double(),
    ##   SANS = col_double(),
    ##   SAPS = col_double(),
    ##   VerbalIQ = col_double(),
    ##   NonVerbalIQ = col_double(),
    ##   TotalIQ = col_double()
    ## )

``` r
# then duration data
art <- read_csv("Articulation.txt")
```

    ## Parsed with column specification:
    ## cols(
    ##   soundname = col_character(),
    ##   nsyll = col_double(),
    ##   npause = col_double(),
    ##   `dur (s)` = col_double(),
    ##   `phonationtime (s)` = col_double(),
    ##   `speechrate (nsyll/dur)` = col_double(),
    ##   `articulation rate (nsyll / phonationtime)` = col_double(),
    ##   `ASD (speakingtime/nsyll)` = col_double()
    ## )

    ## Warning: 26 parsing failures.
    ##  row                      col expected        actual               file
    ## 1066 ASD (speakingtime/nsyll) a double --undefined-- 'Articulation.txt'
    ## 1073 ASD (speakingtime/nsyll) a double --undefined-- 'Articulation.txt'
    ## 1394 ASD (speakingtime/nsyll) a double --undefined-- 'Articulation.txt'
    ## 1437 ASD (speakingtime/nsyll) a double --undefined-- 'Articulation.txt'
    ## 1579 ASD (speakingtime/nsyll) a double --undefined-- 'Articulation.txt'
    ## .... ........................ ........ ............. ..................
    ## See problems(...) for more details.

``` r
# Finally the pitch data
pitch <- read_csv("pitch_data.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   Study = col_double(),
    ##   Participant = col_double(),
    ##   Trial = col_double(),
    ##   Diagnosis = col_double(),
    ##   Mean = col_double(),
    ##   IQR = col_double(),
    ##   SD = col_double()
    ## )

``` r
# Preprocessing to be able to merge
# Demo
demo$Diagnosis <- ifelse(demo$Diagnosis == "Control", 0, 1)

# Art
art$Participant <-
    art$soundname %>% 
    str_extract("S\\d+") %>%
    gsub("S", "", .) %>% 
    as.integer()

art$Study <-
    art$soundname %>% 
    str_extract("Study\\d") %>%
    gsub("Study", "", .)

art$Diagnosis <-
    art$soundname %>% 
    str_extract("D\\d") %>%
    gsub("D", "", .)

art$Trial <-
    art$soundname %>% 
    str_extract("T\\d+") %>% 
    gsub("T", "", .) %>% 
  as.integer()

# Now we merge the data
df_1 <- merge(demo, pitch, by = c("Participant", "Study", "Diagnosis"), all = T)
df <- merge(df_1, art, by = c("Participant", "Study", "Diagnosis", "Trial"), all = T)

# Making sure that all participants have a unique participant ID (right now they can have the same across studies and diagnoses)
df$UniqueID <- paste("P", as.character(df$Participant), "S", as.character(df$Study), "D",  as.character(df$Diagnosis), sep = "")

df$UniqueID <- as.integer(as.factor(df$UniqueID))

# Unique pair ID
df$UniquePair <- paste("P", as.character(df$Participant), "S", as.character(df$Study), sep = "")

df$UniquePair <- as.integer(as.factor(df$UniquePair))

# Now we save them
write_csv(df, "A3_data_merged.csv")
```

Now we need to describe our sample
----------------------------------

First look at the missing data: we should exclude all recordings for
which we do not have complete data. Then count the participants and
recordinsgs by diagnosis, report their gender, age and symptom severity
(SANS, SAPS and Social) Finally, do the same by diagnosis and study, to
assess systematic differences in studies. I like to use group\_by()
%&gt;% summarize() for quick summaries

``` r
# Subsetting the studies of interest (keeping the Danish studies)
df <- subset(df, Language == "Danish")

# Changing the name of variables to make them sensible
colnames(df)[14] <- "PitchMean"
colnames(df)[15] <- "PitchIQR"
colnames(df)[16] <- "PitchSD"

# Describing the sample by diagnosis and excluding all recordings for which we do not have complete data
df %>% 
  group_by(Diagnosis) %>% 
  summarise(N = n(),
  mAGE = mean(Age, na.rm = T),
  sdAGE = sd(Age, na.rm = T),
  IQRAGE = IQR(Age, na.rm = T),
  minAGE = min(Age, na.rm = T),
  maxAGE = max(Age, na.rm = T),
  femaleN = sum(Gender == "F", na.rm = T),
  maleN = sum(Gender == "M", na.rm = T),
  mSANS = mean(SANS, na.rm = T),
  sdSANS = sd(SANS, na.rm = T),
  mSAPS = mean(SAPS, na.rm = T),
  sdSAPS = sd(SAPS, na.rm = T),
  mVerbalIQ = mean(VerbalIQ, na.rm = T),
  sdVerbalIQ = sd(VerbalIQ, na.rm = T),
  mNonVerbalIQ = mean(NonVerbalIQ, na.rm = T),
  sdNonVerbalIQ = sd(NonVerbalIQ, na.rm = T),
  mTotalIQ = mean(TotalIQ, na.rm = T),
  mPitchMean = mean(PitchMean, na.rm = T),
  mPitchIQR = mean(PitchIQR, na.rm = T),
  mPitchSD = mean(PitchSD, na.rm = T)
  )
```

    ## # A tibble: 2 x 21
    ##   Diagnosis     N  mAGE sdAGE IQRAGE minAGE maxAGE femaleN maleN mSANS
    ##       <dbl> <int> <dbl> <dbl>  <dbl>  <dbl>  <dbl>   <int> <int> <dbl>
    ## 1         0  1007  26.4  8.92      6     18     62     432   575 0.392
    ## 2         1   932  26.6  8.88      7     18     67     402   530 9.65 
    ## # ... with 11 more variables: sdSANS <dbl>, mSAPS <dbl>, sdSAPS <dbl>,
    ## #   mVerbalIQ <dbl>, sdVerbalIQ <dbl>, mNonVerbalIQ <dbl>,
    ## #   sdNonVerbalIQ <dbl>, mTotalIQ <dbl>, mPitchMean <dbl>,
    ## #   mPitchIQR <dbl>, mPitchSD <dbl>

``` r
# Describing the sample by diagnosis and study
df%>% 
  group_by(Study, Diagnosis) %>% 
  summarise(N = n(),
  mAGE = mean(Age, na.rm = T),
  sdAGE = sd(Age, na.rm = T),
  IQRAGE = IQR(Age, na.rm = T),
  minAGE = min(Age, na.rm = T),
  maxAGE = max(Age, na.rm = T),
  femaleN = sum(Gender == "F", na.rm = T),
  maleN = sum(Gender == "M", na.rm = T),
  mSANS = mean(SANS, na.rm = T),
  sdSANS = sd(SANS, na.rm = T),
  mSAPS = mean(SAPS, na.rm = T),
  sdSAPS = sd(SAPS, na.rm = T),
  mVerbalIQ = mean(VerbalIQ, na.rm = T),
  sdVerbalIQ = sd(VerbalIQ, na.rm = T),
  mNonVerbalIQ = mean(NonVerbalIQ, na.rm = T),
  sdNonVerbalIQ = sd(NonVerbalIQ, na.rm = T),
  mTotalIQ = mean(TotalIQ, na.rm = T),
  mPitchMean = mean(PitchMean, na.rm = T),
  mPitchIQR = mean(PitchIQR, na.rm = T),
  mPitchSD = mean(PitchSD, na.rm = T)
  )
```

    ## # A tibble: 8 x 22
    ## # Groups:   Study [4]
    ##   Study Diagnosis     N  mAGE sdAGE IQRAGE minAGE maxAGE femaleN maleN
    ##   <dbl>     <dbl> <int> <dbl> <dbl>  <dbl>  <dbl>  <dbl>   <int> <int>
    ## 1     1         0   348  22.7  3.19   3.25     18     30     159   189
    ## 2     1         1   337  22.8  3.11   3        18     31     160   177
    ## 3     2         0   184  23.7  3.54   5        18     34      56   128
    ## 4     2         1   179  23.3  3.86   5        18     33      47   132
    ## 5     3         0   242  36.6 12.9   22        20     62     113   129
    ## 6     3         1   176  39.4 12.7   19        19     67      96    80
    ## 7     4         0   233  24.4  4.51   6        18     34     104   129
    ## 8     4         1   240  24.8  3.68   6        18     35      99   141
    ## # ... with 12 more variables: mSANS <dbl>, sdSANS <dbl>, mSAPS <dbl>,
    ## #   sdSAPS <dbl>, mVerbalIQ <dbl>, sdVerbalIQ <dbl>, mNonVerbalIQ <dbl>,
    ## #   sdNonVerbalIQ <dbl>, mTotalIQ <dbl>, mPitchMean <dbl>,
    ## #   mPitchIQR <dbl>, mPitchSD <dbl>

Now we can analyze the data
---------------------------

If you were to examine the meta analysis you would find that the
differences (measured as Hedges’ g, very close to Cohen’s d, that is, in
standard deviations) to be the following - pitch variability (lower,
Hedges’ g: -0.55, 95% CIs: -1.06, 0.09) - proportion of spoken time
(lower, Hedges’ g: -1.26, 95% CIs: -2.26, 0.25) - speech rate (slower,
Hedges’ g: -0.75, 95% CIs: -1.51, 0.04) - pause duration (longer,
Hedges’ g: 1.89, 95% CIs: 0.72, 3.21). (Duration - Spoken Duration) /
PauseN

We need therefore to set up 4 models to see how well our results compare
to the meta-analytic findings (Feel free of course to test more
features)

Describe the acoustic profile of a schizophrenic voice *Note* in this
section you need to describe the acoustic profile of a schizophrenic
voice and compare it with the meta-analytic findings (see 2 and 3 in
overview of part 1).

N.B. the meta-analytic findings are on scaled measures. If you want to
compare your results with them, you need to scale your measures as well:
subtract the mean, and divide by the standard deviation. N.N.B. We want
to think carefully about fixed and random effects in our model. In
particular: how should study be included? Does it make sense to have all
studies put together? Does it make sense to analyze both languages
together? Relatedly: does it make sense to scale all data from all
studies together? N.N.N.B. If you want to estimate the studies
separately, you can try this syntax: Feature \~ 0 + Study +
Study:Diagnosis + \[your randomEffects\]. Now you’ll have an intercept
per each study (the estimates for the controls) and an effect of
diagnosis per each study

-   Bonus points: cross-validate the models and report the betas and
    standard errors from all rounds to get an idea of how robust the
    estimates are.

``` r
# Scaling (by hand - but can be done more esay by using the scale() function)
df$ScaledPitchIQR <- (df$PitchIQR-mean(df$PitchIQR, na.rm = T))/sd(df$PitchIQR, na.rm = T)

df$ScaledSpokenProb <- ((df$`phonationtime (s)`/df$`dur (s)`) - mean(df$`phonationtime (s)`/df$`dur (s)`, na.rm=T))/sd((df$`phonationtime (s)`/df$`dur (s)`), na.rm=T)

df$ScaledSpeechRate <- (df$`speechrate (nsyll/dur)`-mean(df$`speechrate (nsyll/dur)`, na.rm = T))/sd(df$`speechrate (nsyll/dur)`, na.rm = T)

# Number of NAs
which(df$npause == NA) # There are no NAs, so we can change 0s to NA, and then NA to 0.
```

    ## integer(0)

``` r
# Changing 0s in npause to NAs (we cannot divide by 0)
df$npause[df$npause == 0] <- NA

df$ScaledPauseDur <- (((df$`dur (s)`-df$`phonationtime (s)`)/df$npause) - mean((df$`dur (s)`-df$`phonationtime (s)`)/df$npause, na.rm = T)) / sd((df$`dur (s)`-df$`phonationtime (s)`)/df$npause, na.rm = T)

df$npause[is.na(df$npause)] <- 0
df$ScaledPauseDur[is.na(df$ScaledPauseDur)] <- 0

# Loading package
p_load(lmerTest)

# Making models
m1 <- lmer(ScaledPitchIQR ~ Diagnosis + (1 |UniqueID), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m1)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledPitchIQR ~ Diagnosis + (1 | UniqueID)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4572.0   4594.2  -2282.0   4564.0     1892 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -5.0739 -0.1844 -0.0751  0.0202 13.0747 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  UniqueID (Intercept) 0.5114   0.7151  
    ##  Residual             0.4987   0.7062  
    ## Number of obs: 1896, groups:  UniqueID, 221
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)   
    ## (Intercept)   0.13600    0.07014 219.01222   1.939  0.05380 . 
    ## Diagnosis    -0.26631    0.10175 218.92009  -2.617  0.00948 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.689

``` r
m1_pair <- lmer(ScaledPitchIQR ~ Diagnosis + (1 + Diagnosis|UniquePair), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m1_pair)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledPitchIQR ~ Diagnosis + (1 + Diagnosis | UniquePair)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4437.0   4470.3  -2212.5   4425.0     1890 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -5.5564 -0.2575 -0.1108  0.0149 14.3060 
    ## 
    ## Random effects:
    ##  Groups     Name        Variance Std.Dev. Corr 
    ##  UniquePair (Intercept) 0.9574   0.9785        
    ##             Diagnosis   1.0517   1.0255   -0.99
    ##  Residual               0.4985   0.7060        
    ## Number of obs: 1896, groups:  UniquePair, 122
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)   
    ## (Intercept)   0.13879    0.09359 115.65658   1.483  0.14081   
    ## Diagnosis    -0.26907    0.10056 117.97447  -2.676  0.00852 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.962

``` r
# These models give beta estimates of around -0.27, which indicates that pitch variability is lower in people diagnosed with schizophrenia than the controls. Meaning, they speak in a more monotone voice. That is, a significant effect in the same direction as the effect reported in the meta-analysis (-0.55) - roughly compared to Hedges' g, it looks similar, and is within its confidence interval.

m2 <- lmer(ScaledSpokenProb ~ Diagnosis + (1 |UniqueID), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m2)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledSpokenProb ~ Diagnosis + (1 | UniqueID)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4428.4   4450.6  -2210.2   4420.4     1892 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -5.0270 -0.5552  0.0311  0.5446  4.2248 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  UniqueID (Intercept) 0.5466   0.7393  
    ##  Residual             0.4546   0.6742  
    ## Number of obs: 1896, groups:  UniqueID, 221
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)  
    ## (Intercept)   0.09013    0.07196 220.63285   1.252   0.2117  
    ## Diagnosis    -0.18994    0.10439 220.55031  -1.820   0.0702 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.689

``` r
m2_pair <- lmer(ScaledSpokenProb ~ Diagnosis + (1 + Diagnosis|UniquePair), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m2_pair)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledSpokenProb ~ Diagnosis + (1 + Diagnosis | UniquePair)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4427.9   4461.2  -2207.9   4415.9     1890 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -5.0207 -0.5572  0.0311  0.5563  4.2493 
    ## 
    ## Random effects:
    ##  Groups     Name        Variance Std.Dev. Corr 
    ##  UniquePair (Intercept) 0.4739   0.6884        
    ##             Diagnosis   0.9026   0.9501   -0.57
    ##  Residual               0.4546   0.6743        
    ## Number of obs: 1896, groups:  UniquePair, 122
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)  
    ## (Intercept)   0.08654    0.06742 115.94660   1.284   0.2018  
    ## Diagnosis    -0.18301    0.09691 112.41263  -1.888   0.0615 .
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.573

``` r
# These models give beta estimates of -0.19 and -0.18, which indicates that the proportion of spoken time is a bit smaller when people diagnosed with schizophrenia talk, compared to controls. However, these effects are very small, not significant but in the same direction as the effect reported in the meta-analysis (-1.26). Meaning that the results do not compare that well with the significant negative effect of the paper.

m3 <- lmer(ScaledSpeechRate ~ Diagnosis + (1 |UniqueID), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m3)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledSpeechRate ~ Diagnosis + (1 | UniqueID)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4615.5   4637.7  -2303.7   4607.5     1892 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -4.5289 -0.5851 -0.0140  0.5545  4.2505 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  UniqueID (Intercept) 0.4627   0.6802  
    ##  Residual             0.5175   0.7194  
    ## Number of obs: 1896, groups:  UniqueID, 221
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)   
    ## (Intercept)   0.15416    0.06722 221.04291   2.293  0.02278 * 
    ## Diagnosis    -0.31691    0.09751 220.94034  -3.250  0.00133 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.689

``` r
m3_pair <- lmer(ScaledSpeechRate ~ Diagnosis + (1 + Diagnosis|UniquePair), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m3_pair)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledSpeechRate ~ Diagnosis + (1 + Diagnosis | UniquePair)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   4614.4   4647.7  -2301.2   4602.4     1890 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -4.5038 -0.5846 -0.0106  0.5579  4.2481 
    ## 
    ## Random effects:
    ##  Groups     Name        Variance Std.Dev. Corr 
    ##  UniquePair (Intercept) 0.4142   0.6436        
    ##             Diagnosis   0.7249   0.8514   -0.57
    ##  Residual               0.5176   0.7194        
    ## Number of obs: 1896, groups:  UniquePair, 122
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)    
    ## (Intercept)   0.15082    0.06398 116.23990   2.358  0.02007 *  
    ## Diagnosis    -0.30801    0.08883 114.82719  -3.468  0.00074 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.574

``` r
# These models give beta estimates of respectively -0.32 and -0.31, which indicates that speech rate is slower in people diagnosed with schizophrenia than in controls. In both models, the effect is significant, like the one in the paper (-0.75) and the estimates are in the same direction as the effect reported in the paper.

m4 <- lmer(ScaledPauseDur ~ Diagnosis + (1 |UniqueID), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m4)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledPauseDur ~ Diagnosis + (1 | UniqueID)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   5049.1   5071.4  -2520.6   5041.1     1935 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -3.2605 -0.3873 -0.1469  0.1849 12.8456 
    ## 
    ## Random effects:
    ##  Groups   Name        Variance Std.Dev.
    ##  UniqueID (Intercept) 0.2017   0.4492  
    ##  Residual             0.6786   0.8238  
    ## Number of obs: 1939, groups:  UniqueID, 264
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)   
    ## (Intercept)  -0.11259    0.04838 260.66358  -2.327  0.02073 * 
    ## Diagnosis     0.22573    0.06933 268.69126   3.256  0.00128 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.698

``` r
m4_pair <- lmer(ScaledPauseDur ~ Diagnosis + (1 + Diagnosis|UniquePair), data = df, REML = F, lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE))
summary(m4_pair)
```

    ## Linear mixed model fit by maximum likelihood . t-tests use
    ##   Satterthwaite's method [lmerModLmerTest]
    ## Formula: ScaledPauseDur ~ Diagnosis + (1 + Diagnosis | UniquePair)
    ##    Data: df
    ## Control: lmerControl(optimizer = "nloptwrap", calc.derivs = FALSE)
    ## 
    ##      AIC      BIC   logLik deviance df.resid 
    ##   5013.2   5046.6  -2500.6   5001.2     1933 
    ## 
    ## Scaled residuals: 
    ##     Min      1Q  Median      3Q     Max 
    ## -3.5556 -0.3956 -0.1420  0.1941 12.6991 
    ## 
    ## Random effects:
    ##  Groups     Name        Variance Std.Dev. Corr 
    ##  UniquePair (Intercept) 0.06499  0.2549        
    ##             Diagnosis   0.27666  0.5260   -0.02
    ##  Residual               0.67912  0.8241        
    ## Number of obs: 1939, groups:  UniquePair, 138
    ## 
    ## Fixed effects:
    ##              Estimate Std. Error        df t value Pr(>|t|)    
    ## (Intercept)  -0.11040    0.03484 130.08775  -3.168 0.001910 ** 
    ## Diagnosis     0.21488    0.06173 132.58254   3.481 0.000677 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Correlation of Fixed Effects:
    ##           (Intr)
    ## Diagnosis -0.343

``` r
# These models give beta estimates of 0.23 and 0.21, which indicates that the duration of pauses when people diagnosed with schizophrenia speak, is a bit longer compared to controls. The effects are significant, and in the same direction as the reported Hedges's g (1.89). It is, however, smaller and outside the confidence intervals.



# Choosing the best model
# Since the variables above are scaled, we can compare the beta-values. The best model being the model which has the greatest absolute value (and does not cross 0, when the standard errors are taken into account).
# That is, m3 with an estimate of -0.32 (SE = 0.10). So, the feature (of these four), which is described the best by diagnosis, is speech rate.
```

N.B. Remember to save the acoustic features of voice in a separate file, so to be able to load them next time
-------------------------------------------------------------------------------------------------------------

``` r
# Saving the features in a separate file
write_csv(df, "data_af")
```

Reminder of the report to write
-------------------------------

Part 1 - Can we find a difference in acoustic features in schizophrenia?

1.  Describe your sample number of studies, number of participants, age,
    gender, clinical and cognitive features of the two groups.
    Furthemore, critically assess whether the groups (schizophrenia and
    controls) are balanced. N.B. you need to take studies into account.

2.  Describe the acoustic profile of a schizophrenic voice: which
    features are different? E.g. People with schizophrenia tend to have
    high-pitched voice, and present bigger swings in their prosody than
    controls. N.B. look also at effect sizes. How do these findings
    relate to the meta-analytic findings?

3.  Discuss the analysis necessary to replicate the meta-analytic
    findings Look at the results reported in the paper (see
    meta-analysis in the readings) and see whether they are similar to
    those you get. 3.1) Check whether significance and direction of the
    effects are similar 3.2) Standardize your outcome, run the model and
    check whether the beta’s is roughly matched (matched with hedge’s g)
    which fixed and random effects should be included, given your
    dataset? E.g. what about language and study, age and gender? Discuss
    also how studies and languages should play a role in your analyses.
    E.g. should you analyze each study individually? Or each language
    individually? Or all together? Each of these choices makes some
    assumptions about how similar you expect the studies/languages to
    be.

-   Your report should look like a methods paragraph followed by a
    result paragraph in a typical article (think the Communication and
    Cognition paper)
