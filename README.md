#Application #Method #Analysis

Cayde Bruce

Code: https://github.com/caydebruce/bible_analysis

# Abstract
Bible translations differ in wording, which can subtly shape sentiment and tone, but there hasn’t been much research quantifying these differences. This project uses sentiment analysis to compare emotional trends across translations, looking at how shifts in language impact the way meaning is perceived. By combining lexicon-based and deep-learning methods, I analyze sentiment polarity, distribution changes, and key theological terms. Early results suggest that modern translations tend to tone down emotionally intense language, shifting sentiment distributions compared to older versions like the KJV. This project aims to shed light on how the language of religious texts evolves, even when they’re often seen as unchanging and to explore the challenges of applying NLP to texts with cultural and theological significance. Culminating with attempting to train a model to predict which translation a specific block of text comes from showing that each translation is different enough to be classified. 

# Project Motivation


# What this project is about
Religious texts shape how people think about culture and theology, but the way they’re translated can subtly change their meaning—sometimes just through word choice and tone. This project looks at how sentiment shifts across different Bible translations, measuring the linguistic and emotional differences between older versions like the King James Version (KJV) and newer ones like the World English Bible (WEB). These shifts matter because translation choices can affect how readers interpret the text, what stands out emotionally, and even which theological ideas get emphasized.

The goal here is to use sentiment analysis to see how the overall emotional tone, whether positive, negative, or neutral, varies across translations. I’ll explore whether modern versions tend to soften emotionally intense language and track how key theological terms like “wrath,” “grace,” “sin,” and “salvation” change in sentiment over time. This study focuses on broad trends across multiple English Bible translations rather than debating theological accuracy.

To do this, I’ll use both lexicon based sentiment models (like VADER and SentiWordNet) and deep learning approaches (DistilBERT and fine-tuned DistilBERT). The dataset comes from publicly available Bible texts via Project Gutenberg, Open Bible APIs, and other sources. The analysis involves comparing sentiment scores for aligned verses, looking at how sentiment distributions shift across translations, and measuring sentiment polarity agreement (SPA). I’ll also do some deep dives into key theological passages to get a richer understanding of the trends. Additionally, I will create a model to try and predict what translation a verse is from given random verse from a translation.

Early results suggest that modern translations tend to soften negative sentiment in certain passages, reflecting broader linguistic and cultural shifts. By blending quantitative and qualitative analysis, this project sheds light on how language in religious texts evolves and highlights the challenges of applying NLP to texts with deep cultural and theological weight.

# Progress made so far
#### Unexpected Challenges
* As it turns out, most translations of the bible are copyrighted making gathering their data difficult and/or unethical.
* The bible, while large, actually isn't a ton of data (at least in the realm of NLP) leading to potential under-training.
* Few-Shot sentiment analysis: I could not get my Open-AI API key to work, so I opted to use the TogetherAI APIs.

#### Experiment Progress
* Data procurement: I gathered a great dataset from kaggle with 6 different translation of the Bible that are all covered under free use.
* Sentiment Exploration with Lexicon Based Methods: I have successfully utilize multiple lexicon based methods to explore the dataset and show general trends throughout the years on the sentiment of various parts of the bible. I focused on three distinct lists of books within each translation—Old Testament, New Testament, and theologically relevant books excluding the gospels ("Genesis", "Psalms", "Isaiah", "John", "Romans", "Revelation") as these make up the majority of theologically relevant teachings throughout various faiths that rely on the Bible.
* Sentiment Analysis with Pre-Trained Models: Using pretrained word2vec embeddings, I waas able to track the semantic drift of a few key biblical terms across the years of translation. Interestingly, the 'path' that the words take seems to be nearly identical regardless of the word in question. This is expected because all translations share a common theological and linguistic structure, meaning that word relationships are adjusted in a globally consistent manner rather than in isolated shifts. Since t-SNE preserves local structures, it captures this uniform transformation, reinforcing that translations maintain a cohesive semantic framework rather than diverging unpredictably. It shows a consistent translation methodology which, while unsurprising, I find extremely interesting.
* Few-Shot sentiment Analysis: The differences between zero-shot and few-shot learning became increasingly clear. Initially, it seemed that zero-shot classification might be sufficient for sentiment analysis, but as more ambiguous verses were tested, its limitations—such as incomplete responses and misinterpretations—became evident. With the introduction of few-shot examples, the model's accuracy and consistency improved significantly. This led to a full analysis of the sentiment of the key theological books.
* Translation Classification: To gain additional insights, I developed an LSTM classifier to distinguish between different Bible translations. Through experimentation, I discovered that t_kjv accounted for the majority of classification errors, primarily being misclassified as t_wbt or t_asv. Removing t_kjv improved the classifier’s performance by 17%, revealing an unexpected but intriguing insight: these three translations follow strikingly similar linguistic patterns. Further refinement (removing t_wbt) led to another 9% improvement, reinforcing the idea that these translations have undergone minimal change over time. While this wasn’t my original goal, it’s fascinating that these experiments still uncovered strong linguistic patterns across translations.

# Approach
This project applies sentiment analysis and LLMs to quantify emotional tone differences across Bible translations. I use lexicon-based models, fine-tuned transformer models, and foundation LLMs to analyze sentiment polarity (positive, negative, neutral) and emotional intensity. The approach consists of the following steps:

#### Data Collection & Preprocessing
* I compile multiple Bible translations from Project Gutenberg and a Kaggle data set: https://www.kaggle.com/datasets/oswinrh/bible.
* Verses are aligned across translations to ensure direct comparisons.
* The text is tokenized, lemmatized, and preprocessed to remove punctuation and special characters (replacing archaic language in some cases).

#### Sentiment Analysis Methods
* Lexicon-Based Models: I use VADER and SentiWordNet, which assign sentiment scores based on predefined dictionaries.
* Pretrained Models: I use DistilBERT on religious text sentiment datasets to find a sentiment score.
* Trained Models: I use few-shot learning on a Mistral (7B) Instruct v0.2 to extract sentiment scores and analyze what the model thought about verses with theological nuances.
* Sentiment scores are computed at the verse level and aggregated across chapters and books.

#### Evaluation Metrics
* Sentiment Polarity Agreement (SPA): Measures how often different translations assign the same sentiment polarity.
* Precision (more on this later).
* F-1 Score (more on this later).
* Accuracy (more on this later).
* Qualitative Analysis for zero-shot vs few shot.

#### Baselines
* Standard sentiment analysis models (VADER, SentiWordNet) as baselines for lexicon-based methods.
* Unsupervised embeddings (word2vec) to track semantic drift in theological terms.
* Results from "Sermon on the mount sentiment analysis" Paper.

#### Novelty
While sentiment analysis has been applied to the Bible in previous studies (specifically the sermon on the mount passages), I want to focus on the general semantic drift of certain theologically relevant words to see if there is any sort of trend in sentiment over time.

# Experiments

### Lexicon Based Experiments
* Experiments with lexicon based analysis indicates that there is a trend in positive sentiment as each new translation comes out. There are exceptions. However, These baselines will provide good data for the rest of the analysis. To reduce error from archaic language, I have created a set of words that get replaced to their modern counterparts before preprocessing. I used two sentiment frameworks (VADER and SentiWordNet) in order to get better baseline understandings.

| VADER | SentiWordNet |
|---------|---------|
| ![image](https://github.com/user-attachments/assets/c2990633-f90d-4dc9-9d06-d412796fd509) | ![image](https://github.com/user-attachments/assets/e395b25c-95e0-4167-91da-8536b2b29c26) |
| ![image](https://github.com/user-attachments/assets/517c8f5a-72f0-457d-a786-f6a80e3fa0e9) | ![image](https://github.com/user-attachments/assets/593c115e-743c-4cc0-bf7f-8aa262e7ff31) |
| ![image](https://github.com/user-attachments/assets/d2302a5d-4665-4382-b7f4-d5309e9c3797) | ![image](https://github.com/user-attachments/assets/16942c61-3cf1-4047-a07d-3a30f6c15692) |

Using our favorite library (Word2Vec) I also trained a Word2Vec model on all the translation to get their closest words in the vector space.

#### Semantic Similarity of Biblical Keywords Across Translations

This table presents the top five most semantically similar words to specific keywords in different Bible translations. Each translation is labeled, followed by a table containing the related words and their similarity scores.

---

#### **King James Version (KJV)**

| Keyword | Similar Word 1 | Score | Similar Word 2 | Score | Similar Word 3 | Score | Similar Word 4 | Score | Similar Word 5 | Score |
|---------|--------------|------|--------------|------|--------------|------|--------------|------|--------------|------|
| **God** | Mercy | 0.947 | Liveth | 0.923 | Glory | 0.923 | Bless | 0.918 | Truth | 0.918 |
| **Sin** | Trespass | 0.915 | Atonement | 0.905 | Peace | 0.893 | Accept | 0.892 | Sacrifice | 0.886 |
| **Mercy** | Grace | 0.965 | Salvation | 0.964 | Bless | 0.962 | Righteousness | 0.957 | Truth | 0.956 |
| **Grace** | Truth | 0.982 | Sinned | 0.982 | Faith | 0.981 | Liveth | 0.973 | Witness | 0.972 |
| **Salvation** | Blessing | 0.980 | Prayer | 0.975 | Bless | 0.975 | Grace | 0.972 | Truth | 0.969 |
| **Jesus** | Balak | 0.929 | Samuel | 0.928 | Prophet | 0.928 | Cried | 0.919 | Rabshakeh | 0.915 |
| **Wrath** | Fury | 0.958 | Shine | 0.953 | Lighten | 0.952 | Dread | 0.951 | Destroy | 0.948 |
| **Hope** | Seeing | 0.990 | Honour | 0.988 | Pleasure | 0.987 | Pardon | 0.987 | Forget | 0.985 |
| **Anger** | Kindled | 0.953 | Wrath | 0.946 | Power | 0.942 | Hands | 0.941 | Provoke | 0.941 |
| **Heaven** | Heavens | 0.895 | Earth | 0.879 | Footstool | 0.810 | Anger | 0.806 | Face | 0.804 |
| **Death** | Cause | 0.933 | Hold | 0.924 | Trust | 0.915 | Wicked | 0.913 | Lest | 0.912 |

---

#### **American Standard Version (ASV)**

| Keyword | Similar Word 1 | Score | Similar Word 2 | Score | Similar Word 3 | Score | Similar Word 4 | Score | Similar Word 5 | Score |
|---------|--------------|------|--------------|------|--------------|------|--------------|------|--------------|------|
| **God** | Glory | 0.945 | Lovingkindness | 0.936 | Salvation | 0.932 | Bless | 0.930 | Righteousness | 0.922 |
| **Sin** | Iniquity | 0.951 | Forgive | 0.948 | Wisdom | 0.946 | Righteous | 0.945 | Righteousness | 0.943 |
| **Mercy** | Salvation | 0.974 | Glad | 0.972 | Gracious | 0.970 | Favor | 0.968 | Truth | 0.967 |
| **Grace** | Loved | 0.991 | Dealt | 0.986 | Oath | 0.985 | Hardened | 0.985 | Faithful | 0.985 |
| **Salvation** | Mercy | 0.974 | Swear | 0.973 | Giveth | 0.972 | Righteousness | 0.969 | Establish | 0.969 |
| **Jesus** | Answered | 0.952 | Balak | 0.936 | Told | 0.934 | Asked | 0.927 | Prophet | 0.926 |
| **Wrath** | Anger | 0.954 | Darkness | 0.934 | Fierceness | 0.932 | Destroy | 0.928 | Dread | 0.926 |
| **Hope** | Reward | 0.994 | Seeing | 0.992 | Almighty | 0.991 | Delight | 0.990 | Perfect | 0.989 |
| **Anger** | Provoke | 0.972 | Kindled | 0.970 | Wrath | 0.954 | Power | 0.949 | Fierceness | 0.942 |
| **Heaven** | Heavens | 0.898 | Earth | 0.883 | Face | 0.858 | Created | 0.826 | Power | 0.824 |
| **Death** | Shame | 0.972 | Hold | 0.938 | Turn | 0.922 | Surely | 0.909 | Wicked | 0.907 |

#### **Bible in Basic English (BBE)**

| Keyword   | Similar Word 1 | Score  | Similar Word 2 | Score  | Similar Word 3 | Score  | Similar Word 4 | Score  | Similar Word 5 | Score |
|-----------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|
| **God**   | Grace       | 0.922  | Saviour     | 0.901  | Salvation    | 0.895  | Asaphgt      | 0.887  | Everything   | 0.887  |
| **Sin**   | Sins        | 0.937  | Wrongdoing  | 0.934  | Forgiveness  | 0.920  | Evildoing    | 0.879  | Pleasure     | 0.858  |
| **Mercy** | Salvation   | 0.971  | Hope        | 0.952  | Grace        | 0.943  | Righteousness | 0.942 | Faith        | 0.937  |
| **Grace** | Saviour     | 0.964  | Salvation   | 0.947  | Mercy        | 0.943  | Hope         | 0.940  | Davidgt      | 0.934  |
| **Salvation** | Mercy   | 0.971  | Hope        | 0.959  | Grace        | 0.947  | Saviour      | 0.945  | Today        | 0.937  |
| **Jesus** | Christ      | 0.915  | Samuel      | 0.886  | Servant      | 0.885  | Isaiah       | 0.876  | David        | 0.865  |
| **Wrath** | Passion     | 0.882  | Haters      | 0.854  | Moving       | 0.854  | Heat         | 0.845  | Moved        | 0.842  |
| **Hope**  | World       | 0.972  | Salvation   | 0.959  | Saviour      | 0.959  | Righteousness | 0.956 | Mercy        | 0.952  |
| **Anger** | Debtor      | 0.471  | Made        | 0.464  | Sin          | 0.462  | Breathingspace | 0.461 | Chaldaea     | 0.453  |
| **Heaven** | Heavens    | 0.924  | Earth       | 0.859  | Glory        | 0.854  | Lifted       | 0.836  | Light        | 0.835  |
| **Death** | Sword      | 0.875  | Cross       | 0.768  | Dead         | 0.745  | End          | 0.724  | Body         | 0.717  |

#### **Young's Literal Translation (YLT)**

| Keyword   | Similar Word 1 | Score  | Similar Word 2 | Score  | Similar Word 3 | Score  | Similar Word 4 | Score  | Similar Word 5 | Score |
|-----------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|
| **God**   | Kindness    | 0.951  | Age         | 0.943  | Salvation    | 0.940  | Blessed      | 0.935  | Truth        | 0.931  |
| **Sin**   | Judgment    | 0.975  | Righteousness | 0.961 | Walk        | 0.954  | Judge        | 0.952  | Iniquity     | 0.947  |
| **Mercy** | Laying      | 0.990  | Restrained  | 0.988  | Planting    | 0.988  | Setting      | 0.987  | Delivereth   | 0.986  |
| **Grace** | Ear         | 0.965  | Fear        | 0.958  | Righteousness | 0.957 | Understanding | 0.956 | Truth        | 0.954  |
| **Salvation** | Truth   | 0.969  | Confess     | 0.968  | Thank       | 0.965  | Honour       | 0.964  | Harden       | 0.964  |
| **Jesus** | Answering   | 0.959  | Answered    | 0.950  | Therefore   | 0.949  | Answer       | 0.943  | Lord         | 0.936  |
| **Wrath** | Fury        | 0.984  | Fierceness  | 0.975  | Burned      | 0.972  | Provoking    | 0.971  | Arm          | 0.969  |
| **Hope**  | Habitually  | 0.994  | Manifested  | 0.991  | Profit      | 0.991  | Happy        | 0.991  | Always       | 0.990  |
| **Anger** | Fury        | 0.961  | Wrath       | 0.950  | Fierceness  | 0.930  | Adversaries  | 0.928  | Retained     | 0.915  |
| **Heaven** | Boweth     | 0.938  | Noise       | 0.933  | Dagon       | 0.918  | Refreshed    | 0.916  | Trance       | 0.915  |
| **Death** | Shame      | 0.835  | Require     | 0.804  | Ranges      | 0.800  | Certainly | 0.790  | Riddle       | 0.783  |

#### **Webster's Bible Translation (WBT)**

| Keyword   | Similar Word 1 | Score  | Similar Word 2 | Score  | Similar Word 3 | Score  | Similar Word 4 | Score  | Similar Word 5 | Score  |
|-----------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|
| **God**   | Mercy       | 0.974  | Liveth      | 0.945  | Bless        | 0.940  | Truth        | 0.936  | Salvation    | 0.934  |
| **Sin**   | Iniquity    | 0.977  | Forgive     | 0.961  | Righteous    | 0.955  | Ways         | 0.951  | Whatever     | 0.951  |
| **Mercy** | God        | 0.974  | Liveth      | 0.974  | Truth        | 0.971  | Bless        | 0.966  | Grace        | 0.963  |
| **Grace** | Faith      | 0.989  | Shown       | 0.987  | Sinned       | 0.987  | Promised     | 0.984  | Honor        | 0.983  |
| **Salvation** | Prayer  | 0.983  | Swear       | 0.982  | Help         | 0.980  | Curse        | 0.976  | Judgment     | 0.976  |
| **Jesus** | Strictly   | 0.938  | Prophet     | 0.930  | Isaiah       | 0.929  | Furthermore  | 0.928  | Spoken       | 0.928  |
| **Wrath** | Destroy    | 0.975  | Shine       | 0.971  | Lift         | 0.970  | Strong       | 0.969  | Fury         | 0.968  |
| **Hope**  | Wickedly   | 0.991  | Seeing      | 0.991  | Lovingkindness | 0.990 | Delight      | 0.990  | Preserve     | 0.989  |
| **Anger** | Provoke    | 0.982  | Execute     | 0.967  | Established  | 0.963  | Kindled      | 0.961  | Forsaken     | 0.959  |
| **Heaven** | Heavens   | 0.908  | Earth       | 0.901  | Face         | 0.890  | Bush         | 0.845  | Power        | 0.805  |
| **Death** | Hold       | 0.937  | Enemies     | 0.928  | Trust        | 0.918  | Wicked       | 0.918  | Wrath        | 0.917  |

#### **World English Bible (WEB)**

| Keyword   | Similar Word 1 | Score  | Similar Word 2 | Score  | Similar Word 3 | Score  | Similar Word 4 | Score  | Similar Word 5 | Score  |
|-----------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|--------------|--------|
| **God**   | GT          | 0.949  | Praise       | 0.948  | Jealous      | 0.947  | Kindness      | 0.943  | Righteousness | 0.943  |
| **Sin**   | Accept      | 0.958  | Peace        | 0.935  | Forgiven     | 0.926  | Sacrifice     | 0.920  | Trespass      | 0.920  |
| **Mercy** | Grace       | 0.995  | Established  | 0.992  | Faith        | 0.992  | Oath          | 0.992  | Gracious      | 0.991  |
| **Grace** | Mercy       | 0.995  | Faith        | 0.992  | Oath         | 0.991  | Promised      | 0.990  | True          | 0.989  |
| **Salvation** | Justice | 0.989  | Bless        | 0.989  | Delight      | 0.985  | Forsake       | 0.985  | Declare       | 0.984  |
| **Jesus** | Balak       | 0.971  | Isaiah       | 0.958  | Prophet      | 0.957  | Balaam        | 0.954  | Answered      | 0.953  |
| **Wrath** | Kindled     | 0.960  | Light        | 0.958  | Fierce       | 0.958  | Darkness      | 0.956  | Anger         | 0.952  |
| **Hope**  | Keeps       | 0.995  | Witness      | 0.994  | Merciful     | 0.994  | Diligently    | 0.993  | Triumph       | 0.993  |
| **Anger** | Kindled     | 0.991  | Provoke      | 0.979  | Provoked     | 0.955  | Worship       | 0.953  | Wrath         | 0.952  |
| **Heaven** | Prayer     | 0.939  | Confession   | 0.939  | Loving       | 0.935  | Glory        | 0.933  | GT            | 0.933  |
| **Death** | Enemies     | 0.933  | Hold         | 0.933  | Wicked       | 0.922  | Avenger       | 0.918  | Kill          | 0.912  |

---

#### Observations
Looking at different Bible translations, you can see how theological emphasis shifts over time. Take "God," for example—in the King James Version (KJV) and Webster’s Bible Translation (WBT), He’s all about "Mercy" and "Truth," a kind and benevolent figure. But in the World English Bible (WEB), "Jealous" shows up, making God seem more possessive and demanding of exclusive worship. Similarly, "Sin" changes depending on the translation. KJV and the American Standard Version (ASV) focus on redemption, linking sin to "Trespass" and "Forgive," while Young’s Literal Translation (YLT) leans into a stricter, more legalistic view, tying sin to "Judgment" and "Righteousness."

Other words show just as much variation. "Jesus" in BBE is associated with "Christ," "Servant," and "David," potentially emphasizing his percieved role as a messianic king. But in YLT, he’s more of a teacher or authority figure, with words like "Answering" and "Lord." "Wrath" is another interesting one. The KJV and YLT go all in on divine destruction, with words like "Fury" and "Destroy," while BBE makes it sound more emotional, associating it with "Passion" and "Heat." Even "Hope" has varying results. In ASV, it’s tied to "Reward" and "Almighty," making it seem like a divine promise, while BBE links it to "World" and "Saviour," making it feel more immediate and practical. One of the weirdest shifts is "Anger". In BBE is oddly connected to "Debtor" and "Made," almost like it's part of a transaction rather than just an emotional state.

### Pre-trained Model Based Experiement 

* Sentiment Results with DistilBERT

### Key Theological Books
![image](https://github.com/user-attachments/assets/e20b8e9e-5a20-48e7-8bf2-a20c0420fa3a)

### Old Testament
![image](https://github.com/user-attachments/assets/3c18289d-fad8-4705-8319-5134a998bd60)

### New Testament
![image](https://github.com/user-attachments/assets/31b1836e-e7f7-4f1e-8317-28de589e6b23)

We consistently see a positive sentiment trend across the Old Testament, Old Testament, and Key Theological Books.

### Trained Model Experiment
* We train a model on the each translation and show the semantic drift amongst related words, utilizing t-SNE to visualize the trends. This gives us a really cool visualization on how certain words are viewed within each translation.

| | |
|---------|---------|
| ![image](https://github.com/user-attachments/assets/693ae15b-4342-4c77-8d30-64d7f0010e76) | ![image](https://github.com/user-attachments/assets/62b0c3c4-5d0e-41f6-bb47-d29924cfbc32) |
![image](https://github.com/user-attachments/assets/88a6ef17-6103-4c4b-96a4-07f0bcaf5bd8) | ![image](https://github.com/user-attachments/assets/f9a5f5e8-6f4a-4a26-8810-22aaaeedd88a) |
![image](https://github.com/user-attachments/assets/9890e4fe-0948-40f3-bc35-6251c3aebdf8) | ![image](https://github.com/user-attachments/assets/472e47c4-646b-4644-95ef-ba7da2e91942) |

#### Takeaways
Words like grace, God, Jesus, sin, and salvation shift in their associations depending on the translation, reflecting theological, linguistic, and cultural changes. For instance, grace in older translations like Young’s Literal Translation (YLT 1862) aligns more with sanctification and atonement, whereas in modern translations like Basic Bible English (BBE 1965), it leans toward kindness and blessing—suggesting a shift from a theological to a more personal, moral interpretation.

Similarly, God appears with distinct associations across translations. The King James Version (KJV 1611) maintains a strong connection to Jehovah, Elohim, and Yahweh, emphasizing proper names and covenantal aspects of God. Meanwhile, YLT 1862 places God near abstract virtues like love, peace, and faith, indicating a more philosophical framing. This pattern suggests that earlier translations leaned heavily on biblical names and titles, while later versions moved toward broader, universal descriptions (although WEB moves back [More on that at the end]). The same phenomenon appears with Jesus, who in older translations is tightly linked to Savior and Messiah, but in modern translations is associated with miracles and divine perfection, focusing more on his actions than doctrinal identity.

Words with moral and doctrinal weight, like sin and salvation, also show interesting shifts. In KJV 1611, sin clusters around righteousness and wickedness, showing a moralistic duality. Then in BBE 1965 emphasizes guilt, punishment, and shame, making it more of a personal, psychological burden. Salvation follows a similar path. YLT 1862 places it among baptism, sanctification, and atonement, suggesting a structured theological process, while KJV 1611 frames it around faith and conversion, indicating an individual spiritual experience. In contrast, BBE 1965 links salvation with resurrection and life, making it more about ultimate destiny than immediate faith.

These shifts suggest that translations not only update language but also reflect changes in religious thought, cultural attitudes, and doctrinal emphasis. Older translations tend to maintain strict theological structures, often emphasizing law, judgment, and covenantal frameworks. In contrast, modern translations move toward interpretations that are more personal, experiential, and emotionally resonant. This being said the World English Bible (WEB 1994) seems to be an odd outlier in many of these observations, often positioning itself between the old and new translations which is interesting because it is the newest translation.


### Zero-shot and Few-Shot analysis
For this experiment, I use Mistral-7B-Instruct-v0.2 with two prompts seen here.

*Zero-Shot*
`Classify the sentiment of the following text. Limit your response to just Positive, Neutral, or Negative: {text}`

*Few-Shot*
`Classify the sentiment of the following text. Limit your response to just Positive, Neutral, or Negative 

    Examples:

    1 "The Lord is my shepherd, I shall not want." → Positive
    2 "Blessed are the meek, for they shall inherit the earth." → Positive
    3 "Fear not, for I am with you; be not dismayed, for I am your God." → Positive
    4 "The Lord is my light and my salvation—whom shall I fear?" → Positive
    5 "For God so loved the world, that he gave his only begotten Son." → Positive
    6 "The joy of the Lord is your strength." → Positive
    7 "I can do all things through Christ who strengthens me." → Positive
    8 "Be strong and courageous; do not be afraid, for the Lord your God goes with you." → Positive
    9 "My God, my God, why have you forsaken me?" → Negative
    10 "Woe to the rebellious children, saith the Lord, that take counsel, but not of me." → Negative
    11 "The wages of sin is death, but the gift of God is eternal life through Jesus Christ our Lord." → Nuetral
    12 "And Judas went and hanged himself." → Negative
    13 "For many are called, but few are chosen." → Negative
    14 "You are of your father the devil, and your will is to do your father’s desires." → Negative
    15 "Cursed is the man who trusts in man and makes flesh his strength." → Negative
    16 "The heart is deceitful above all things, and desperately wicked: who can know it?" → Negative
    17 "Now these are the names of the sons of Israel, who came into Egypt with Jacob." → Neutral
    18 "This is the book of the generations of Adam." → Neutral
    19 "In the beginning, God created the heavens and the earth." → Neutral
    20 "And Noah was six hundred years old when the flood of waters was upon the earth." → Neutral
    21 "Jesus went up to Jerusalem for the Feast of the Passover." → Neutral
    22 "Paul, an apostle of Jesus Christ, by the will of God, to the saints in Ephesus." → Neutral
    23 "And the Lord spoke to Moses, saying, 'Command the Israelites to bring you clear oil for the lampstand.'" → Neutral

    Now analyze this text:
    "{text}" → `

These were qualitative manual classifications. I tried my best to get a good spread of negative vs positive and interesting verses. Below are teh results from 10 test verses. (TODO: Find a better way to test this)

Verse: 
**"And whatever you ask in my name, I will do, so that the Father may be glorified in the Son. If you ask anything of me in my name, I will do it"**  
- **Zero-Shot Prediction:** `.`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"The Lord gave, and the Lord has taken away; blessed be the name of the Lord."**  
- **Zero-Shot Prediction:** `Answer: Positive Ex`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"To everything there is a season... A time to be born, and a time to die."**  
- **Zero-Shot Prediction:** `A time to plant, and a time to re`  
- **Few-Shot Prediction:** `Neutral`  

---

Verse: 
**"Do not think that I have come to bring peace to the earth. I have not come to bring peace, but a sword."**  
- **Zero-Shot Prediction:** `For I have come to turn a man against his`  
- **Few-Shot Prediction:** `Negative`  

---

Verse: 
**"Faithful are the wounds of a friend, but deceitful are the kisses of an enemy."**  
- **Zero-Shot Prediction:** `Negative ### 10`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"For when I am weak, then I am strong."**  
- **Zero-Shot Prediction:** `Positive`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"The greatest among you shall be your servant. Whoever exalts himself will be humbled, and whoever humbles himself will be exalted."**  
- **Zero-Shot Prediction:** `Answer: Neutral`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"Blessed shall he be who takes your little ones and dashes them against the rock!"**  
- **Zero-Shot Prediction:** `Negative`  
- **Few-Shot Prediction:** `Negative`  

---

Verse: 
**"And we know that all things work together for good to those who love God."**  
- **Zero-Shot Prediction:** `Positive`  
- **Few-Shot Prediction:** `Positive`  

---

Verse: 
**"If anyone comes to me and does not hate his father and mother, wife and children, brothers and sisters—yes, even their own life—such a person cannot be my disciple."**  
- **Zero-Shot Prediction:** `Negative`  
- **Few-Shot Prediction:** `Negative`  

---

Verse: 
**"Consider it pure joy, my brothers and sisters, whenever you face trials of many kinds."**  
- **Zero-Shot Prediction:** `James 1:2 Neutral`  
- **Few-Shot Prediction:** `Positive`

#### Insights

This experiment highlighted some key differences between zero-shot and few-shot sentiment analysis, particularly when dealing with nuanced and ambiguous biblical texts. One major takeaway was that few-shot learning provided much more consistent and accurate classifications. By giving the model some examples before asking it to classify a verse, it was able to focus on the task rather than attempting to complete or reinterpret the text. This was evident in cases where zero-shot responses were cut off or included unrelated content, like when it attempted to continue the verse instead of simply labeling it. For example, in *"To everything there is a season... A time to be born, and a time to die,"* the zero-shot model trailed off, whereas the few-shot approach correctly labeled it as neutral.  

Ambiguous verses were particularly challenging, with some classifications varying between zero-shot and few-shot. Take Proverbs 27:6—*"Faithful are the wounds of a friend, but deceitful are the kisses of an enemy."* The zero-shot model classified it as negative, likely focusing on the words “wounds” and “deceitful,” whereas the few-shot model recognized the deeper meaning and labeled it as positive. This suggests that without examples to guide interpretation, the model struggles to capture underlying sentiment in paradoxical or context-heavy statements. However, when sentiment was more extreme, such as in *"Blessed shall he be who takes your little ones and dashes them against the rock!"*, both approaches accurately classified it as negative.  

Ultimately, few-shot learning proved far more reliable, particularly for complex or nuanced religious texts. The additional context helped the model avoid misinterpretations and focus on classification rather than text completion. While zero-shot learning still worked for clearer statements, it often stumbled on verses that required deeper contextual understanding. A logical next step would be testing across different Bible translations or even fine-tuning a model specifically for religious texts to see if accuracy can be further improved.

#### Semantic analysis using Few-Shot

Due to time/compute constraints, I limited the analysis to 200 random verses per book (keeping the indices the same across translations)​, calculating their mean, then plotting again on the X-axis by year. Here are the results:

![image](https://github.com/user-attachments/assets/43183202-36dd-41bf-b067-f6d90bf270e4)

Although t_ylt seems to be more negative that the other older translations, the overall trend is still positive. This aligns with the rest of the results so far. 

### Predictive Model Experiment: Part 1
I wanted to see if it was possible to predict which translation a particular verse was from by using a trained neural network. Due to the similarity in semantics between versions. However, I think this is still worth pursuing.

I chose to use tensorflow's keras for creating a model to classify Bible verses by translation due to it's ease of use and performance on CPUs (I didn't want to pay for Google Colab compute credits). It provides built-in tools for text tokenization, word embeddings, and deep learning architectures, making it well-suited for text classification. Here is a table of the hyperparameters:

<img width="733" alt="Screenshot 2025-03-07 at 4 56 42 PM" src="https://github.com/user-attachments/assets/aba8d2fb-ba59-455c-9389-26aa23fc11ea" />

### Results of best performer (simplest model)
<img width="471" alt="Screenshot 2025-02-24 at 1 17 09 PM" src="https://github.com/user-attachments/assets/1a9ae935-c040-4e33-a768-4c066e039678" />

Consistent with the lexicon based analysis, t_bbe (Bible in Basic English) is the easiest to predict and t_kjv (King James Version) is the hardest for the model to predict. Due to the archaic language in the t_kjv, I would have anticipated the accuracy to be a lot higher. I think this warrant further exploration.

#### 1. Precision
Precision measures how many predicted instances for a class were actually correct.

$$
\text{Precision} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Positives (FP)}}
$$

Example: If the model predicts a verse as t_bbe, precision tells us how often that prediction is correct.

#### 2. Recall (Sensitivity)
Recall measures how many actual instances of a class were correctly identified.

$$
\text{Recall} = \frac{\text{True Positives (TP)}}{\text{True Positives (TP)} + \text{False Negatives (FN)}}
$$

Example: If there are many t_ylt verses, recall tells us how many were correctly classified.

#### 3. F1-Score
The F1-score is the harmonic mean of precision and recall, balancing both metrics.

$$
F1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
$$

Example: If a translation has high precision but low recall, or vice versa, the F1-score reflects this balance.

#### 4. Support
Support refers to the*number of actual instances in each class.

Example: Each translation has around 6,220 examples, meaning we have a balanced dataset.

#### Model Performance on Bible Translations

| Translation | Precision | Recall | F1-Score | Support |
|-------------|------------|--------|---------|---------|
| **t_wbt**  | 0.51 | 0.51 | 0.51 | 6220 |
| **t_asv**  | 0.50 | 0.43 | 0.46 | 6221 |
| **t_web**  | 0.76 | 0.81 | 0.79 | 6220 |
| **t_ylt**  | 0.81 | 0.83 | 0.82 | 6221 |
| **t_bbe**  | 0.94 | 0.95 | 0.95 | 6221 |
| **t_kjv**  | 0.44 | 0.46 | 0.45 | 6221 |



#### Takeaways
- t_bbe (Bible in Basic English) has the best performance with an F1-score of 0.95, meaning it is highly accurate in classification.
- t_ylt (Young’s Literal Translation) and t_web (World English Bible) also perform well, with F1-scores of 0.82 and 0.79.
- t_kjv (King James Version) and t_asv (American Standard Version) perform the worst, with low precision, recall, and F1-scores (~0.45-0.46). This suggests the model struggles to distinguish these translations.
- Overall Accuracy is 66%, meaning the model classifies about two-thirds of verses correctly. Again, underwhelming, but not totally surprising. This failure to correctly distinguish Bible verses from certain translations could be seen as evidence that they are good at keeping with the original meaning of the text and resisting semantic drift.


### Predictive Model Experiment: Part 2
Taking what I had learned from part 1, I wanted to see how high the accuracy could get between different Bible translations. Thinking that there might have been problems I was unaware of with keras, I went ahead and defined an LSTMClassifier that was similar to the ones we saw in the homework. I also thought about they way I was handling the data. Since the Bible is made up of many small verses, the odds of some verses being identical or nearly identical is pretty high. This would result in the model simply guessing between translations when there would be multiple correct answers. To mitigate this, I added two steps to preprocessing. I also decided to focus on confusion matrices in order to better understand where the model might be making mistakes.
* 1. Remove perfect duplicates of verses if they are shared between two or more translations.
* 2. Created a function that takes an $n$ and if the number of words in a verse is $< n$ I append the next verse to the previous until the verse is $> n$ words long.

For the next 3 experiements I used an LSTM with the following hyperparameters:

| Hyperparameter  | Description                                           | Value                |
|-----------------|-------------------------------------------------------|----------------------|
| `vocab_size`    | Size of the vocabulary (number of unique tokens)      | 55163                |
| `num_classes`   | Number of output classes (books)                      | 6                    |
| `padding_idx`   | Index used for padding in the embedding layer         | `0`                  |
| `input_size`    | Dimensionality of input features to LSTM              | 128                  |
| `hidden_size`   | Number of features in the hidden state of LSTM        | 256                  |
| `num_layers`    | Number of LSTM layers                                 | `1` (default)        |
| `batch_first`   | Whether batch is the first dimension in input tensor  | `True`               |
| `enforce_sorted`| Whether sequences need to be sorted by length         | `False`              |


#### 5 training epochs - no extra data cleanup:

![image](https://github.com/user-attachments/assets/e657cb39-764f-40df-9991-a2ddb22fe148)

#### 10 epochs - removing duplicate verses - merging verses if number of tokens $> 20$:

![image](https://github.com/user-attachments/assets/c20b2e94-6ce4-43b2-9a0f-fbb77c447af4)

Test loss: 0.8480, Test accuracy: 0.6438

After analyzing the results, I noticed a clear pattern: the t_kjv translation was causing significant issues for the classifier, accounting for most of the errors. The model frequently misclassified t_kjv as either t_wbt or t_asv nearly two-thirds of the time. Given this, I decided to remove t_kjv from the experiment to better evaluate the classifier's performance under optimized conditions. This decision was made to push the model's capabilities and observe its potential without the confounding factor of t_kjv.

#### Removing KJV with 10 epochs - removing duplicate verses - merging verses if number of tokens $> 20$:

![image](https://github.com/user-attachments/assets/d56cf7af-e88e-455c-9e7b-f7e8798bbe81)

Test loss: 0.8480, Test accuracy: 0.8194

This led to an impressive 17% improvement! As shown in the confusion matrix, t_wbt was responsible for the next largest set of errors, primarily due to misclassification as t_asv. To maintain a consistent methodology, I proceeded with the next experiment by removing t_wbt as well.

#### Removing KJV and WBT with 10 epochs - removing duplicate verses - merging verses if number of tokens $> 20$:

![image](https://github.com/user-attachments/assets/d34b1a59-e07d-457c-8cb3-9038e9efcd2f)

Test loss: 0.5215, Test accuracy: 0.9002

Another 9% improvement! What’s particularly interesting is that, across this entire analysis, t_kjv, t_wbt, and t_asv consistently follow similar trends, regardless of the method used. This, combined with the classifier experiment, suggests that these translations are the most similar to each other and have undergone the least change over the centuries. And while this wasn’t my original goal, it’s fascinating that these experiments still revealed such strong patterns in the data.

# Final Takeaways

This project set out to quantify sentiment differences across Bible translations but ended up uncovering deeper patterns in linguistic consistency and semantic drift.  

1. Sentiment Trends Reflect Linguistic and Cultural Shifts:
Modern translations tend to soften emotionally intense language, showing a shift toward more positive sentiment. This suggests that translation choices reflect broader cultural and theological trends while maintaining core messages.  

2. Semantic Drift is Structured, Not Random:
Key theological terms shift in meaning across translations, but they do so in a systematic way. Instead of independent changes, translations maintain a globally consistent structure, reinforcing linguistic continuity even as language evolves.  

3. Few-Shot Learning Improves Sentiment Classification:
Zero-shot sentiment classification struggled with theological nuance, often producing incomplete or ambiguous results. Few-shot learning significantly improved accuracy, suggesting that structured guidance is crucial for NLP models in religious contexts.  

4. Some Translations Are Nearly Indistinguishable:
The LSTM classifier revealed that **t_kjv, t_wbt, and t_asv** followed strikingly similar trends, making them difficult to differentiate. Removing them improved classification accuracy by **17%** and then **9%**, suggesting minimal linguistic change over time.  

5. NLP Faces Challenges in Religious Texts:
While sentiment analysis and classification models are powerful, they struggle with theological nuance. Some translations are more semantically similar than expected, highlighting the difficulty of defining linguistic "difference" in sacred texts.  

Conclusion:
While not the original goal, this study revealed strong patterns in translation stability and linguistic evolution. The findings suggest that even sacred texts, often seen as unchanging, subtly adapt over time. Future work could explore fine-tuning models for religious texts, analyzing additional translations, or expanding to multilingual comparisons.
