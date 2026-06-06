# Biogpt_guided-imputation
BioGPT as a Biological Prior Extractor
BioGPT (microsoft/biogpt) is used not as a generative model at inference time, but as a frozen biological knowledge oracle in a one-time offline step. Its role is to build a gene–protein interaction prior matrix encoding which mRNA genes are most likely to regulate which RPPA protein targets in gastric adenocarcinoma (STAD), knowledge that is then injected into the cross-modal Transformer imputer.
How It Works
For every matched mRNA gene × RPPA protein pair, BioGPT is queried with structured natural-language prompts such as:
"In stomach adenocarcinoma, TP53 mRNA expression regulates TP53 protein abundance.
True or false? Answer:"
Rather than generating free text, we extract the log-softmax probabilities of positive tokens (True, Yes) and negative tokens (False, No) from the model's final output logits, converting them into a [0,1]. interaction score. Three prompts are used per pair, covering regulation, co-expression, and pathway co-membership, and their scores are averaged.
