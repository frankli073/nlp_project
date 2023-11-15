# Rhyme Generator Proposal
- [1. Members and Collaboration Plan]
- [2. Introduction]
- [3. Methodology]

### 1. Members and Collaboration Plan: 
Ruiyang Chen: research, writing paper, data engineering(2.c, 2.d, 4.a.iii)

Yuval Medina: data engineering (2.a, 2.b, 3.a, 3.b), hyperparameter tuning for RNN, GPT, compile research

Frank Li: data gathering (1), data engineering(2.c, 2.d), hyperparameters tuning for BERT, LSTM. Project documentation

Steven Zhang: Evaluation analysis design, error analysis design. Theoretical problem expert, data engineering(4.a.i, 4.a.ii, 4.a.iv)

### 2. Introduction:

	We propose a rhyme generator for incomplete verses or lyrics. In NLP, the distinct nature of lyrics is often overlooked, since most of the algorithms focus on the generation of prose texts. Lyrics convey messages through an emotional manner, and the significance of lyrics is often highlighted by how it makes songs more appealing. The distinct characteristic of lyrics is underscored by the rhyming nature that ties together the words at the end of each line. Our algorithm is intended to generate rhymed words for incomplete sentences (or generate rhymed sentences as lyrics).

### 3. Methodology:
	
 	We propose a comprehensive plan to annotate the Poetry Foundation’s publicly available dataset using the CMU Pronouncing Dictionary (using both pronunciation and syllable stress markers) and NLTK part-of-speech tagger. For each word in a poem, we will attach both annotations in order to better model the function of each word in a poem. Based on (Suharto, 2004 ), we find that syllable stress is extremely predictive of a word’s position as well as its function in a particular sonnet. We also find that syllable pronunciation will be useful for the model to predict n-gram rhyming schemes (Tanasescu, Paget, & Inkpen, 2016).
	
 	After compiling our annotated dataset we will add the rhyme scheme feature to the poem genres which employ a strict rhyme scheme – these include coupled rhyme (AA BB CC), ballade (ABAB BCBC), monorhyme (AAA..), enclosed rhyme (ABBA), etc. For other poem genres, such as haiku, which need not rhyme but employ other schemes, we’ll simply omit them from this stage of training, as we concentrate on rhyme generation. We will then split prompt (first line – A) and response (differing number of lines based on poem genre – A, BA, BBA, BCA, etc.) based on the poem genre feature, in order to build the fine-tuning dataset. This will serve as the finalized dataset to be used in training.
After these steps, we will fine-tune from among 5 publicly available open-sourced models – OpenAI’s GPT and Codex, BERT, XLNet, and CTRL – in order to model rhyme and verse generation and poem generation based on an input line of text. We will employ hyperparameter tuning and evaluation throughout the process of training.
	
 	We additionally propose two more pathways for additional work: one specifically using melody to model pop lyrics generation, improving on the work of (Watanabe et al, 2018); and one specifically using beat to model rap lyric generation, improving on the work of (Xue, 2021). These two additional datasets will be an opportunity to further fine-tune our models to generate genre-specific poetry and/or lyrics, and will be more open-ended in terms of rhyming structure.
We will also use this opportunity to further finetune models like DeepRapper (Xue, 2021) with the CMU Pronouncing Dictionary and NLTK part-of-speech tags annotations in order to improve modeling of underlying part-of-speech, word-stress, and syllable patterns in rap lyrics.

### 4. Roadmap
1. Data Gathering 
- Use poetry foundation .org dataset.
2. Data Wrangling and Processing
- Add CMU pronunciation + syllable stress for each word.
- Add NLTK POS tags to each word.
- Optional: create an additional dataset with a [beat] marker to model rap lyrics.
- Optional: create an additional dataset with MIDI, tying pop lyrics to the melody.
3. Feature Engineering and Extraction
- Poem genre - optional feature.
- Rhyming structure (i.e. AABB) based on poem genre - optional feature.
4. Training
- Fine tune publicly available models using our custom dataset:

4.1 Simple LSTM

4.2 Simple RNN

4.3 GPT

4.4 BERT

4.5 OpenAI’s Codex

4.6 XLNet

4.7 CTRL

5. Hyperparameters Tuning and Evaluation
Adjust hyperparameters for deep learning models
Include different features of the language itself, such as context, previous word, sentence structures, and etc.
Evaluation Plan and Error Analysis:
- We will use a 80-20 cross validation to split our data. For results generated by each model (including testing results generated by different hyperparameters within each RNN model), we will have three stages of evaluations.conduct human evaluation and check whether the generated lyrics rhyme or not. The evaluation metric would be a simple accuracy test, since we are focusing on generating lyrics that rhyme. 
- During the first round, we use the CMU Pronouncing dictionary to automatically evaluate rhymes. Choose models with the best performance during this stage.
- During the second round, we let people evaluate a portion of our test results.
- We will then use the accuracy scores and conduct error analysis, and see what kind of mistakes or lyrics each general model might be generating (for example, maybe BERT might generate words that spell differently that the word at the end of the previous sentence but their pronunciations are the same). Since our hyperparameters tuning part includes having different combinations of features, we will use our error analysis to see the best combination based on the features we include and the hyperparameters of the RNN models. We will then keep fine tuning the model itself and upgrade the program until mistakes generated by the model are more general and not specific (like keep generating wrong words when the previous sentence ends in ‘abcdef’). 
- During the last stage, we use bleu rouge and attempt to distinguish human and machine generated lyrics.
Discussion for Academic Paper 
Wu, Dekai, Karteek Addanki, and Markus Saers. "Modeling Hip Hop Challenge-Response Lyrics as Machine Translation."

1) This is one of the earliest papers focusing on rap lyrics generation, and it approached rap lyrics generation as a translation problem. The author refers to the generation of hip hop lyric responses as a "translation problem" because they are drawing an analogy between the process of generating a response in a hip hop battle (which involves creating lines that rhyme and correspond in meaning or theme to the challenge presented by an opponent) and the process of translating text from one language to another. In both cases, there is a source input (a hip hop challenge or a text in the source language) that needs to be converted into a different but related output (a rhyming response or a text in the target language) that maintains some aspects of the original—such as meaning, rhythm, or rhyme in the case of hip hop, and meaning and grammatical correctness in the case of language translation.

“Lyrics information processing: Analysis, generation, and applications” by K. Watanabe and M. Goto.

1) This paper provides a brief introduction to the distinct nature of lyric generation compared to prose text generation. As lyrics are used in songs to convey emotions through audio and words, the author proposed to set lyrics information processing as a research field by itself.

2) To be more specific, lyrics are distinct in their nature from 3 perspectives: (1) Their sentence structure as a whole, and more importantly, oftentimes lyrics have rhyme schemes that should be followed from previous sentences. It might also follow different segmentation as a whole sentence might be segmented into three sentences of lyrics in a song. (2) The semantics of lyrics should also be analyzed as each sentence contains words that convey the same sentiments, or the entire song might be used to tell a story. As a result, generating lyrics could also mean generating a short article of texts which has its unique rhyme schemes and is capable of conveying messages and emotions through texts. 3) The most distinct nature of lyrics come from the fact that the text is intertwined with the audio (“DeepRapper” by Xue and others discussed this nature as well, and they addressed by labeling [BEAT] alongside texts).

3) The author briefly discusses a few methods that could be instrumental for lyrics processing and generation, which include ghostwriting (following a certain writing structure or rhyme scheme) with a DNN-based language model.
 
“DeepRapper: Neural rap generation with rhyme and rhythm modeling”  by Xue, L.; Song, K.; Wu, D.; Tan, X.; Zhang, N. L.; Qin, T.; Zhang, W.-Q.; and Liu, T.-Y

1) In this paper, the authors introduced a novel way of recognizing the rhymes in the rap by putting a special [BEAT] mark after words that align with beats. This is because rap songs focus more on rhythmic beats to allow the generated lyrics to be sung. To better model rhyme, they also generate words in a right-to-left order, so that the last few words of the song to rhyme with can be easily identified. A N-gram rhyme modeling is then used in the DeepRapper system to handle distinctive rhyme patterns and generate words that align with those rhymes.

Devlin, Jacob, et al. "BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding." Proceedings of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies (NAACL-HLT), 2019.

1) This paper introduces the BERT model, which stands for bidirectional encoder representation from transformers. Based on the nature of transformers, BERT is bidirectional which is instrumental in processing lyrics. Compared to the unidirectional model (like DeepRapper), now we use the previous and next word for each word, and it will be used to help with lyrics information processing. This model and article is used significantly in  the DeepRapper project, and DeepRapper is based on BERT but with more features being engineered.

Nikolov, Nikola I., et al. "Rapformer: Conditional Rap Lyrics Generation with Denoising Autoencoders."

1) Unlike a lot of other generators, the RapFormer could generate rap lyrics with source text and context (like input a news article) with a transformer based approach. The rationale behind the transformer based approach is that the author wants to capture the story and the rhyming schemes in the songs. The fact that it is based on a content is the reason for naming it Conditional Rap Lyrics Generator
Collaboration:
For our collaboration plan, we have a detailed assignment for everyone since we plan to implement several methods (BERT, LSTM, GPT, etc). We will work on different models, since we don’t need to rely on other people’s results for this. For the data engineering part, we all focus on different features, and one focuses on the pronunciation, another member focuses on adding audio features, and etc.. After having pretrained models, we will bring our results together, conduct error analysis, and discuss our model of choice and future usage and upgrade. (For Specific Plan, Look at Section Members and Collaboration Plan)
References:
Addanki, Karteek et al. “Modeling Hip Hop Challenge-Response Lyrics as Machine Translation.” Machine Translation Summit (2013).

Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. 2019. BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. In Proceedings of the 2019 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies, Volume 1 (Long and Short Papers), pages 4171–4186, Minneapolis, Minnesota. Association for Computational Linguistics.

Nikola I. Nikolov, Eric Malmi, Curtis Northcutt, and Loreto Parisi. 2020. Rapformer: Conditional Rap Lyrics Generation with Denoising Autoencoders. In Proceedings of the 13th International Conference on Natural Language Generation, pages 360–373, Dublin, Ireland. Association for Computational Linguistics.

Suharto, S. (2004). Music and language: A stress analysis of English song lyrics. Harmonia: Journal of Arts Research and Education, 5(3).

Tanasescu, C., Paget, B., & Inkpen, D. (Year). Automatic classification of poetry by meter and rhyme. University of Ottawa.

Watanabe, K., Matsubayashi, Y., Fukayama, S., Goto, M., Inui, K., & Nakano, T. (Year). A melody-conditioned lyrics language model. Graduate School of Information Sciences, Tohoku University; National Institute of Advanced Industrial Science and Technology (AIST); RIKEN Center for Advanced Intelligence Project.

Watanabe, K.,  and Goto M., “Lyrics information processing: Analysis, generation, and applications,” in Proceedings of the 1st Workshop on NLP for Music and Audio (NLP4MusA), 2020, pp. 6–12.

Xue, L.; Song, K.; Wu, D.; Tan, X.; Zhang, N. L.; Qin, T.; Zhang, W.-Q.; and Liu, T.-Y. 2021. DeepRapper: Neural rap generation with rhyme and rhythm modeling. arXiv preprint
arXiv:2107.01875.




### Resources
MLDB Lyrics API: https://www.mldb.org/

Lyrics Information Processing: Analysis, Generation, and Applications: https://aclanthology.org/2020.nlp4musa-1.2/
- Section 2 describes how lyrics are different from regular text.

NLP Song Lyrics Analysis on Kaggle: https://www.kaggle.com/code/marzenah/song-lyrics-analysis-nlp
- The main takeaway from the above analysis is that with the passing years since the 1950s, the recorded songs tend to contain more words (both, total and unique) yet the ratio between the number of unique words to a total number of words in songs is decreasing (i.e., increasing repetition of words in songs).

RNN: Poetry Generators 
https://www.kaggle.com/code/paultimothymooney/poetry-generator-rnn-markov
- Poetry often has rhymed words and this user uses a RNN Markov model to generate rhymed lyrics.

DeepRapper: Neural Rap Generation with Rhyme and Rhythm Modeling https://aclanthology.org/2021.acl-long.6/
- The paper introduces DeepRapper, the first system to generate rap incorporating both rhymes and rhythms, achieved through a Transformer-based model using a unique dataset and specialized language modeling techniques, proven to generate high-quality and creative raps through evaluations.

CMU pronounciating dictionary: http://www.speech.cs.cmu.edu/cgi-bin/cmudict

Complete Poetry Foundation .org dataset: https://www.kaggle.com/datasets/johnhallman/complete-poetryfoundationorg-dataset
