# Kaggle-LLM-Prompt-Recovery
2024 Kaggle Competition - LLM Prompt Recovery

LLMs are commonly used to rewrite or make stylistic changes to text. The goal of this competition is to recover the LLM prompt that was used to transform a given text.

NLP workflows increasingly involve rewriting text, but there's still a lot to learn about how to prompt LLMs effectively. This machine learning competition is designed to be a novel way to dig deeper into this problem.

The challenge: recover the LLM prompt used to rewrite a given text. You’ll be tested against a dataset of 1300+ original texts, each paired with a rewritten version from Gemma, Google’s new family of open models.

# Evaluation Metric
For each row in the submission and corresponding ground truth, sentence-t5-base is used to calculate corresponding embedding vectors. The score for each predicted / expected pair is calculated using the Sharpened Cosine Similarity, using an exponent of 3. The SCS is used to attenuate the generous score given by embedding vectors for incorrect answers. Do not leave any rewrite_prompt blank as null answers will throw an error.

# Algorithm Descriptions:
1.Training Sample Generation:
a)Prompts Creation: Extensively generate rewriting prompts using ChatGPT.
b)Original Texts: Source open web texts from Hugging Face, filtering longer texts.
c)Text Rewriting: Input texts from steps above into Google's open LLM, gemma-7b-it, to create rewritten text data for training samples.

2.Seq2Seq Model:
a)Preprocessing: Convert prompts from generated samples into embedding vectors for faster training.
b)Training Pipeline: Input original and rewritten texts into deberta-v3-large model, concatenate feature outputs, and train on similarity with actual prompt embeddings.
c)Retrieval Database: Create a large database of prompt embeddings for inference retrieval.
d)Inference: Use the trained model for online inference to predict prompt embeddings and retrieve the most similar prompt text from the database.

3.Phi2 Fine-Tuning Model: Employ an open-source Phi2 fine-tuning model for prediction, focusing on key text segments.
4.Zero-Shot LLM Model: Use the open-source model Mistral-7B-Instruct-v0.2, inputting examples to predict directly.
5.Ensemble Prediction: Combine predictions from the three models by string concatenation for the final result.

Data Files:
prompts_df.csv: Prompts for rewritten texts.
train_clean.parquet: Training data samples.
validation826.csv: Validation set.

@misc{llm-prompt-recovery,
    author = {Will Lifferth and Paul Mooney and Sohier Dane and Ashley Chow},
    title = {LLM Prompt Recovery},
    year = {2024},
    howpublished = {\url{https://www.kaggle.com/competitions/llm-prompt-recovery}},
    note = {Kaggle}
}
