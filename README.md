Homework 1
===================
Word Segmentation: This homework is on Chinese word segmentation. We will design and implement a word segmentation program and use a reference segmentation to evaluate the correctness on some test data.

----------

Team Members
-------------
<ul>
    <li>Jiaxi Tang</li>
    <li>Yue Wang</li>
    <li>Fang Zhang</li>
</ul>

----------

Algorithm
-------------

1. Probability Dictionary
	Like the dictionary implemented in the default.py, our algorithm also reads in the unigram counts and saves the related word-probability pair as a dictionary. By applying the chain rule, the unigram model allows us to calculate the probability of a group of words using the product of each word's probability as follows:
	<center>$ p(w_1, w_2, ..., w_n)\approx p(w_1)p(w_2)...p(w_n)=\prod_{i=1}^n p(w_i) $</center>
	We also modify the Pdist class for reading the provided bigram file, where keys are replaced by key pairs. In the bigram model, the probability of a group of words is calculated using the product of each word's conditional probability on its previous word. The formula is as follows:
	<center>$ p(w_1, w_2, ..., w_n)\approx p(w_1)p(w_2|w_1)...p(w_n|w_{n-1})=p(w_1)\prod_{i=2}^n p(w_i|w_{i-1}) $</center>
	
2. Unigram Model
	At the beginning, we implement the unigram model based on the iterative segmenter psuedocode. For each text line, we compare each word $k$ in the dictionary with the first $len(k)$ characters in the sentence and store the entry of each matching word in a list. An entry contains the word, the starting position of the word, the unigram log-probability and the pointer to its previous entry. In the case that no matched word is found, we simply consider the first character as a word and give it a very small probability, say $e^{-10}$. We also maintain a dynamic table that stores the best segmentation (entry) until the end-index of current word.
	
	While the list is not empty, we pop out the top entry in the list. If there is no corresponding entry (same end-index of words) in the dynamic table, we simply add the entry to the dynamic table; Otherwise, we have to compare their log-probability. If the top entry has higher log-probability, we use the entry to replace the old one in the dynamic table to obtain a better segmentation. If the top entry doesn't beat the old entry, we need to scan the dictionary again to store the new matching words starting after the end-index in the list. The iteration stops when the list becomes empty.

	When the iteration ends, we can obtain the best segmentation of the text line by taking the entry at the end of the dynamic table and following the pointer to the previous entry recursively until the first word. With this unigram model, we achieve a score of 88.553, which is between the default score and the baseline score.

3. Bigram Model