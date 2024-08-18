# finetuning-LAION-LeoLM-for-rewriting-scientifically

## Fine-tuning the LeoLM language model to improve the academic and objective writing style of german text passages.

## Explanation:
This project is part of my software development project in the 4th semester at HTW Berlin. My task was to fine-tune Meta's Large Language Model, LLaMA 2, to rewrite german text passages in a more academic or objective manner.

Since LLaMA 2 has not been sufficiently trained in German and performs poorly in the language, I decided to use LeoLM by LAION (https://huggingface.co/LeoLM/leo-hessianai-7b-chat) as the base model, as this model has already been fine-tuned for the German language.

My responsibilities in this project included the creation of the dataset, the fine-tuning process, and the testing of the models.

## Dataset:
To fine-tune the LLM, a dataset specific to the given task is required. Since I couldn't find a dataset that contains both colloquial and scientific versions of German texts, I created one myself. Here's how I did it:

1. I downloaded four non-fiction books in PDF format (topics: Radiation Physics, Chemistry of Biology, Introduction to Computer Science, and Cryptography).
**Dataset_DocumentStyleChecker Notebook:**
2. I extracted all pages from the PDFs using **PyPDF** and filtered them to form meaningful sentence groups, which represent the scientific formulations.
3. To generate the colloquial versions, I used the OpenAI API **(gpt-3.5-turbo)** to rewrite all the sentence groups in a more informal style.
4. Finally, I combined both lists into one and converted them into the appropriate LLaMA 2 prompt format before uploading the dataset to Hugging Face.

**Dataset link:** https://huggingface.co/datasets/JannesKl/RewriteScientificallyDataset_German_LLaMA2_Format

## Finetuning:
The main source for the finetuning process was this tutorial from **DataCamp** for finetuning LLaMA 2: https://www.datacamp.com/tutorial/fine-tuning-llama-2

First, the base model is downloaded and quantized, which reduces computational load by requiring fewer bits to be processed. Additionally, not all parameters are adjusted during the training process; instead, we use Low Rank Adaptation (LoRA) to make the training process more efficient without sacrificing much performance.

These videos were very helpful to me in understanding the training process:
- https://www.youtube.com/watch?v=XpoKB3usmKc&t=1332s (QLoRA)
- https://www.youtube.com/watch?v=zxQyTK8quyY (Tranformers)

## Inference:
1. Ensure that you have Python 3 installed, access to an environment like Google Colab or Jupyter Notebooks, a stable internet connection, and sufficient GPU resources.

2. Download the **"Inference_DocumentStyleChecker.ipynb"** file from the repository and upload it to your Python environment.

3. Execute all cells in sequence. Provide your input text as an argument to the **rewrite_scientifically(string)** method.

4. The new formulation will be automatically printed after running the cell and can be stored in a string variable.

## Limitations:

1. The dataset has far too few examples. Ideally, at least 10,000 examples would be necessary for a robust training process.
2. I should have familiarized myself with metrics for determining the performance of the models before starting the training. In the end, I had ChatGPT generate five colloquial sentences for each topic (a total of 20) and had them reformulated by the three models and LeoLM. The V1 model performed the best, although it should be noted that the base model often had errors such as repetitions or other anomalies.
3. Since I wasn't yet familiar with the loss function used in the training, the hyperparameters were determined randomly. I could only evaluate the model's performance through testing during inference and then estimate the parameters for the next training based on those results.
4. Inference was very time-consuming on my server: each output took approximately 10 minutes during inference.
5. The models often repeat their sentences after a certain point or, for example, simply repeat the input without reformulating it.



