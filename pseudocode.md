# Pseudo-Code of Control Logic

For prompts see [all_prompts.md](all_prompts.md).

## Phase 1: Pairwise Preference Elicitation

```
for each topic:
    pairs = stratified_sample(responses, num_others=4)
    for (resp_A, resp_B) in pairs:
        winner  = LLM(PrefJudgment, passage_1=A, passage_2=B)
        winner2 = LLM(PrefJudgment, passage_1=B, passage_2=A)  # swapped
        record results; drop ties
    borda[resp] = wins - losses
    rank responses by borda score (best first)
```

## Phase 2a: PrefNugget -- Contrastive Nugget Extraction

```
for each topic:
    questions = []
    pairs = winner_loser_pairs sorted by borda(winner) + 0.99*borda(loser) desc
    for (winner, loser) in pairs[:100], taken 2 per topic per batch:
        if len(questions) >= 20: stop
        new_qs = LLM(IterativeExtractDifferentiatingNuggets,
                      winner_passage, loser_passage,
                      given_exam_questions=questions)[:2]   # max 2 new per pair
        questions += deduplicate(new_qs)
    nugget_bank[topic] = questions[:20]
```

## Phase 2b: GroundedNugget -- Single-Response Extraction

```
for each topic:
    questions = []
    responses sorted by borda score (best first)
    for response in responses[:100], taken 2 per topic per batch:
        if len(questions) >= 20: stop
        new_qs = LLM(GroundedIterativeNuggets,
                      response_passage=response,
                      given_exam_questions=questions)[:2]   # max 2 new per response
        questions += deduplicate(new_qs)
    nugget_bank[topic] = questions[:20]
```

## Phase 2c: QueryOnlyNugget -- Parametric Generation

```
for each topic:
    questions = LLM(IterativeGenerateNuggetQuestionsReportRequest,
                     query_title, query_background, query_problem)
    nugget_bank[topic] = deduplicate(questions)[:20]
```

## Phase 3: Response Grading

```
for each (response, nugget_question) in responses x nugget_bank[topic]:

    if grading == "response":
        grade = LLM(GradeNuggetAnswer,
                     question=nugget_question,
                     passage=response.text)                   # -> 0-5

    elif grading == "document_paragraphs":
        for paragraph in response.cited_documents.paragraphs:
            g = LLM(GradeNuggetAnswer,
                     question=nugget_question,
                     passage=paragraph)
        grade = max(g for all paragraphs)                     # best paragraph wins

    nugget_grades[response][nugget] = grade

# Aggregate per response
MAX_GRADE = max(grade for all nuggets)
```
