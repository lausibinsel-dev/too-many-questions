# Prompts for PrefNugget

## Phase 1: Preference Judging (`PrefJudgment`)

> You are a highly experienced and accurate assessor for TREC.
>
> Select the passage that answers the query better. Just answer 1 or 2, without any explanation or extra verbiage.
> If both passages are similar, select the simplest and clearest.

| Direction | Field | Description |
| --- | --- | --- |
| In | `query_title` | Query title |
| In | `query_background` | Background context for the query |
| In | `query_problem` | Problem statement to be addressed |
| In | `passage_1` | Passage 1 |
| In | `passage_2` | Passage 2 |
| Out | `better_passage` | 1 or 2 |
| Out | `confidence` | Score 0.0--1.0 |

## Phase 2: Iterative Contrastive Nugget Extraction (`IterativeExtractDifferentiatingNuggets`)

> Compare Winner vs Loser RAG responses for a query. Focus on relevance, correctness, completeness.
>
> From given_exam_questions, identify or generate questions the Winner addresses much better than the Loser. Reuse questions where possible. New differentiating_questions must be brief, atomic questions about information the Winner handles much better.
>
> Avoid generic quality questions. Make questions self-contained (e.g., "Capital of France?" not "The capital?").

| Direction | Field | Description |
| --- | --- | --- |
| In | `query_title` | Query title |
| In | `query_background` | Background context for the query |
| In | `winner_passage` | The passage that won the comparison |
| In | `loser_passage` | The passage that lost the comparison |
| In | `given_exam_questions` | Given exam questions (from prior iterations) |
| Out | `differentiating_questions` | JSON array of new questions |
| Out | `reasoning` | Brief explanation of the analysis |
| Out | `confidence` | Score 0.0--1.0 |

## Phase 3: Nugget-Based Grading (`GradeNuggetAnswer`)

> Grade how well a passage answers a specific question.
>
> Can the question be answered based on the available context? Choose one:
> - 5: highly relevant, complete, and accurate
> - 4: mostly relevant and complete, minor gaps
> - 3: partially relevant, noticeable gaps
> - 2: limited relevance, significant gaps
> - 1: minimally relevant, substantial shortcomings
> - 0: not relevant or complete at all

| Direction | Field | Description |
| --- | --- | --- |
| In | `question` | The question to be answered |
| In | `passage` | The passage that may contain the answer |
| Out | `grade` | Grade 0--5 |
| Out | `reasoning` | Brief explanation of the grade |
| Out | `confidence` | Score 0.0--1.0 |

