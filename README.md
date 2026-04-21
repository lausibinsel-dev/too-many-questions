# Online Appendix: Too Many Questions

## Abstract

Nugget-based LLM judges evaluate Retrieval-Augmented Generation (RAG) systems using a bank of questions that capture the key facts and criteria an answer should address. These nugget banks are typically constructed through a combination of human input and LLM generation. System outputs are graded by how well they satisfy or cover the nuggets.

For cost and scalability reasons, the nugget bank should be small. However, a major limitation of current nugget generation approaches is that many questions are overly generic and fail to discriminate between top-performing RAG systems. Grounding nuggets in system responses or source documents can increase specificity, but typically leads to an explosion in the number of questions. Since every response and/or citation is graded for every nugget question, a higher number of questions directly increases the amount of LLM prompts and/or tokens, and hence increases costs and runtime.

Inspired by preference-based evaluation, we derive differential nuggets from winner–loser passage pairs, focusing on information that captures differences in topicality, level of detail, and evidential support between responses under an automatic preference judge. We examine how these contrastive signals can be leveraged to construct nugget banks that are both compact and discriminative, enabling reliable separation among top-performing RAG systems.



## Full Empirical Analysis

Raw data from evaluation service: [empirical_data/](empirical_data/)

More plots and tables: [full_empirical_result_analysis.pdf](full_empirical_result_analysis.pdf)


## Prompts and Pseudocode

DSPy 3.1 prompts are used for all implemetations. 

See [Prefnugget Prompts](prompts.md) for the prompts of the method we propose.  


**For all variants and baselines:**

* DSPy Prompts: [all_prompts.md](all_prompts.md).
* Control logic in pseudocode: [pseudocode.md](pseudocode.md)

## Auto-Judge Impementation

An Auto-Judge starterkit implementation of approaches is in [prefnugget-starterkit](https://github.com/laura-dietz/prefnugget-starterkit)

See README for how to install, run, and evaluate
