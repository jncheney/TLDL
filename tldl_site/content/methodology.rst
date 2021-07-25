Methodology
###########


:date: 20210715 12:30
:category: Methodology
:slug: modeling approach
:author: TL;DL Team

The supporting pipeline for this project is both simple and straight forward.  Upon identifying a podcast audio file, the text is transcribed and then sent to the summarization model.  The returned abstractive summary is an easily digestable text that is approximately a paragraph in length.

.. image:: /images/pipeline.png
   :height: 125
   :width: 700
   :alt: alternative text

.............
Transcription
.............

Google Cloud's Speech to Text API handles the transcription portion of the pipeline.  This out-of-the-box solution has many advantages, though it does impose some limitations on the final output.

Advantages
**********

Google Cloud's Speech to Text API provides access to extensively developed speech recognition models.  Further, these robust models performs well with multiple speakers.  Since many podcasts have multiple speakers, this feature is essential to this application.  Finally, with an eye to the future, this solution is also highly scalable, as the computational requirements of processing audio data are offloaded to the cloud, which simplifies management of computational resources.

Disadvantages
*************

Cost represents the biggest disadvantage with this transcription approach.  For the enhanced model capable of processing audio from multiple speakers, and data logging opt-in enabled to reduce costs, audio transcription costs 0.6 cents per 15 second increment.  This translates to 2.4 cents per minute, and $1.44 for a 60-minute podcast episode.  Individually, this is not a concern; however, it would become an issue at scale.  It also places a key element of the pipeline's functionality on third-party resources.  With that, it necessitates remaining vigilant about changes to the API and it provider.

...................
Summarization Model
...................

Initial summarization efforts focused on extractive synopses.  These failed to meet the intent of TL;DL.  This approach tended to latch onto advertisements within podcasts, or other frequently repeated phrasing at the expense of capturing the overall content of the podcast itself.  Though more elusive in nature, an abstrative model proved to be the appropriate solution for the TL;DL application.  Numerous summarization models were explored, with varying levels of results.  The model that generated the best and most consistent results was PEGASUS, Pre-training with Extracted Gap-sentences for Abstrative Summarization.

PEGASUS
*******

PEGASUS is a state-of-the-art model for abstractive text summarization developed by Google researched and published at the 2020 International Conference on Machine Learning.  See links provided in additional readings for further information on the PEGASUS model from its designers.

After experimenting on the 12 datasets used to fine-tune PEGASUS, clear winners emerged for short and long text summaries.  For the short summary, fine-tining on the `xSum <https://www.tensorflow.org/datasets/catalog/xsum>`_ dataset generated the most concise one sentence summarizations.  This dataset is sourced from 227,000 diverse BBC news articles from 2010 to 2017.  Each of the articles is accompanied by a professionally written single-sentence summary for training.  For the long summary, the best results emerged when fine-tuned on `Multi-news <https://github.com/Alex-Fabbri/Multi-News>`_, a 56,000 multip-news corpus of news articles. This data set was accompanied by human-written summaries for the artiles from the newser.com site.
Results from the other data sets produced either incomprensible or repetitive summaries, common problems when attempting to summarize text. Below is a sample of the results obtained when using the PEGASUS model.

.. image:: /images/transcript_sample.png
   :height: 300
   :width: 750
   :alt: alternative text

TL;DL Application
*******************

The biggest challenge with using the PEGASUS model is the fact that all the results are zero-shot summarizations.  PEGASUS was trained to summarize news articles, but our task is to summarize podcast transcripts.  The distinct differences in these types  of transcripts necessitated that a corpus of manually summarized podcast transcripts serve as the training base, rather than the original new article ones.


Additonal Readings
******************

`PEGASUS_Paper <https://arxiv.org/abs/1912.08777>`_

`PEGASUS_blog <https://ai.googleblog.com/2020/06/pegasus-state-of-art-model-for.html>`_
