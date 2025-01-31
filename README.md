<h1> Natural-Language-Inference-NLI <a href= "https://colab.research.google.com/github/shahkarKhan24/Natural-Language-Inference-NLI/blob/main/main.ipynb">   <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></h1>
<h2>Abstract</h2>
In this task, I aim to train a transformer-based model to address a standard Natural Language Inference (NLI) task using the provided dataset.Additionally, I evaluate the model against an adversarial test set. To enhance the model’s performance, I implement several architectural modifications and analyze the outcomes. Fur009 thermore, I create an adversarial training set by augmenting a subset of the original dataset,training another model on this augmented set, and subsequently assessing its performance.

<h3>Dataset Description</h3>
The dataset that was provided to us contains a premise, hypothesis, label and wsd, srl an016 notations, as you can see in the figure 1 in appendice section. The dataset features three types of labels—entailment, neutral, and contradic019 tion—making it a multi-class classification prob020 lem. Our model needs to predict of the hypothesis is contradicting the premise or agreeing with the statement.
<div>
<img src="https://github.com/shahkarKhan24/Natural-Language-Inference-NLI/blob/main/Images/dataset_description.png" width="400" alt="Dataset"/>
</div>


<h3>Description:</h3>
First, I selected two transformer-based model to start my training on provided dataset namely “Roberta” and “distilbert_uncased”. Both model was giving almost same result on standard test set and adversarial set but “roberta ” model was taking way more time during training, each epoch was taking almost 32 minutes while  “distilbert” model was taking about 24 minutes. So, we selected “distilbert” model afterward for further experimentation.
For training a model, first we select the batch size of 16 because that’s the best my machine could handle. We are also suppose to feed the input data which is mainly hypothesis and premise into the format as “[CLS] P [SEP] H”. but since Bert models already handle these token internally, we don’t have to do it. During preprocessing the data for tokenization, we also set truncation = true and max length = 128. For evaluation, we used several matrices, like accuracy, precision, f1_score, recall and auc_roc. 
<h3>Architectural changes:</h3>
 To introduce meaningful architectural changes into our model for NLI task, I consider several approaches, such as integrating a pretrained sentence embedding to enhance our input representation which was imported from sentence_tranformer library. Also I add an attention Mechanisms into our model by adding an attention layer to focus on important parts of the input. So basically the model processes the input tokens (input_ids) and their attention masks (attention_mask).It extracts the [CLS] token representation from DistilBERT, which is typically used for classification tasks. Sentence embeddings are generated using the SentenceTransformer model. The projected output from are passed through a multi-head attention layer. The attention mechanism helps the model focus on different parts of the sentence embeddings, potentially capturing more useful information. The outputs from the attention layer and the projected DistilBERT output are concatenated. The combined output (768 dimensions) is passed through the final classifier layer to produce logits for classification.

<div>
<img src="https://github.com/shahkarKhan24/Natural-Language-Inference-NLI/blob/main/Images/output.png" width="400" alt="architecturL"/>
</div>

<h3>Generating Adversarial Dataset:</h3>
In this task also have to deal with Adversarial-NLI, so modifying the data in a way that the model is more robust for NLI task, so we are asked to generate an adversarial data using original dataset. In order to do that, I used several techniques all combined together to generate my own adversarial dataset comprising of 15000 samples. The focus was on augmenting the hypothesis of each sample, as this is less complex than modifying the premise.
The data augmentation techniques utilized include antonym substitution, synonym substitution, and hypernym substitution, facilitated by the Word Sense Disambiguation (WSD) annotations provided in the original dataset. For antonym substitution, the label was also switched (e.g., contradiction to entailment and vice versa). Additionally, I used cross-lingual augmentation technique, involving back-translation, which proved to be the most sophisticated method in preserving the original sentence's semantics, but at the cost of increased processing time. The other technique that I used were very generic because using semantic alone may or may not be beneficial, so I used some non-semantic augmentation but in a very small percentage. One such technique was word-swap, where subjects or objects within a sentence were exchanged, resulting in slight disorganization without losing semantic meaning. And finally, I used Noise insertion technique which simply find a random words or letter in each sentence and add it again in a sentence. This technique can help the model to be more robust to any typos that may be present in a data.
We also notice that there were many cases where some technique might not work for a sentence, in that case I simply used again the synonym substitution technique which was way more reliable if any technique fails. Each technique was assigned a specific percentage to determine its application across the subset of 15,000 samples, as detailed in the accompanying table. The techniques assigned a lower percentage were chosen as such due to their higher likelihood of causing a loss of semantic meaning in the sentence. The augmenting took around 3 hours to process for 15000 samples, which were than combine and shuffled across the original training dataset  
<div>
<img src="https://github.com/shahkarKhan24/Natural-Language-Inference-NLI/blob/main/Images/Aug_percentage.png" width="400" alt="Dataset"/>
</div>
<h3>Results:</h3>
To comment on the results, I recorded accuracy, precision , f1_score, recall and auc_roc for all experiments. 

<div>
<img src="https://github.com/shahkarKhan24/Natural-Language-Inference-NLI/blob/main/Images/metrics.png" width="400" alt="Dataset"/>
</div>
