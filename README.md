# Steps to Reproduce the code

Download the 300 dimension GloVe embeddings using [this link]("https://nlp.stanford.edu/projects/glove/").</br>
Save it in folder containing the dataset and model.ipynb file. It has not been included in this repository due to its large size. </br>
After this, run model.ipynb notebook to recreate the process.


# Summary
For the given task, the dataset was first preprocessed for the relevant columns. After this, it was tokenized and an embedding layer was created. A CNN model was used for predicting the categories for the product description. The architecture of the CNN model was created after lot of experimentation specific for this dataset. An F1 score of <b><i>0.96</i></b> was obtained on the test set. The detailed steps and approach is given below. Everything mentioned here has also been shown in the Model.ipynb  notebook present in this repository.

# Preprocessing

The dataset was first loaded as a Pandas DataFrame from the excel file. It can be seen that is has 20,000 data points and 15 columns.</br>
"product_category_tree" columns was converted to a list and called "category_tree". Then,"description" column was also converted to a list and called "product_descr". </br>
Now, in category_tree list, only the root category was extracted. For eg, from ["Clothing >> Women's Clothing >> Lingerie, Sleep & Swimwear >> Shorts >> Alisha Shorts >> Alisha Solid Women's Cycling Shorts"], only Clothing was extracted. This was done because we only need to predict the primary category.</br>
The product_descr and its primary extracted category was converted to a pair. Now we had a pair for each data point. All the pairs were then appended to a list and called "list_pair". </br>
Now, since there were so many product categories, we needed to see how many data points are present for each category. Therefore, a dictionary was created to count data points for each category. This dictionary was then sorted in descending order. This dictionary can be found in the python notebook.</br>
In the dictionary we can see that many categories have very less data points. No. of categories were also very high (266 categories). Thus, All the categories having less than 10 data points were discarded. After this process 339 data points were discarded. Now we had 19961 data points, and 28 product categories. </br>
28 product categories were now mapped to integers from 0-27.</br>


# Tokenizing the Dataset

After preprocessing, dataset was split into train and test set using a 80:20 ratio.</br>
After this, BERT pretrained tokenizer ("bert-base-cased") was used to tokenized the product description.</br>
After tokenizing, a list of all unique tokens was created. From this list, a mapping dictionary was created, having each unique token as key, and its index as its value. In total 13017 unique tokens were found to be present.</br>

# Embedding Layer
For creating an embedding layer, GloVe pretrain embeddings were used("glove.6B.300d.txt"). A dictionary called "embeddings_index" was created for GloVe embeddigs.</br>
Now, an embedding_matrix was created having rows equal to number of unique tokens, and 300 columns. (Since our embeddings have 300 dimensions.)</br>

# Encoding, Padding and Truncating

For each product description, each token was encoded to the index value of that respective token using the dictionary we earlier created. Thus strings were converted to integers. If string had less than 100 tokens, they were padded to 100 tokens, and if they had more than 100 tokens, they were truncated to 100. </br>

# Model Used

A CNN Model using the Keras Framework was used for creating the prediction model. The architecture of the model is as follows: </br>

Layer 1 : Embedding Layer (Vocab_Size+1,300) </br>
Layer 2 :    Conv1D Layer    (128,3) </br>
Layer 3 : Conv1D Layer  (256,3)</br>
Layer 4 :   Conv1D Layer    (512,3)</br>
Layer 5 :   Dense Layer (256)</br>
Layer 6 :   Softmax Layer (28)</br>
</br>
Max Pooling was used between the Conv1D layers and BatchNormalizationwas used between all layers. Dropout was also use after the Dense Layer. Loss was set to "categorical_crossentropy", and "adam" optimizer was used. </br>
</br>
Model was trained for 16 epochs having a batch size of 64.

# Results
For analyzing the results, since class imbalance is very high, weighted F1 score was used.</br>
On the test set, an F1 score of <i><b>0.96</b></i> was obtained.</br>
The classification report and graphs for losses and accuracy during training are also present in the python notebook.

# Further Ahead

For increasing the accuracy of the model, further hyperparamter tuning can be done. </br>
Also, information can also be extracted from the other columns and passed onto the model. Since a very good accuracy was obtained and due to time constraint, for now only the product description was used for prediction.</br> 
HD embeddings, introduced by Kanerva can also be used instead of pretrained GloVe embeddings, using which can hopefull increase the F1 score to 0.99</br>
HD embeddings require a lot of computing power to run which couldn't be run on my local machine for such a big dataset. Otherwise, I would have experimented with it too.

