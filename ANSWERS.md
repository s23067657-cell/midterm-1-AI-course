# Midterm 1: answers

GitHub username: s23067657-cell

Session code: kettle-62

Fingerprint: 83527391

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration

I used ChatGPT to help review my answers, check numerical calculations, and improve the clarity of my explanations. I also used the course repository and its instructions to understand the required format and the purpose of each task.  I reviewed the suggested corrections and remain responsible for the final answers. Numerical results, costs, confidence intervals, and claims about data protection must be verified against my own dataset and the formulas provided in the course materials. 

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)


| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A | 0.5667 | 0.7000 | 0.8000 | 0.6485
|B  | 0.3333 | 0.5833 | 0.6333 | 0.4523


### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)


| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 | 0.7000 | 0.575 | 0.801 |
| A, k=5 | 0.8000 | 0.682 | 0.882 |
| B, k=3 | 0.5833 | 0.457 | 0.699 |
| B, k=5 | 0.6333 | 0.507 | 0.744 |


### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 | 22458 |
| A, k=5 | 29807 |
| B, k=3 | 14713 |
| B, k=5 | 16899 |



### 1d. Decision (at most 150 words)

Chosen candidate: B, k=5

I recommend (B,5). Both A candidates exceed the $19,700 budget ($22,458 and $29,807), so only B is affordable: (B,3) costs $14,713 and (B,5) costs $16,899. The reported Wilson intervals overlap, so these results alone do not establish a clear statistical advantage for one candidate. I choose (B,5) because it fits the budget and has higher overall recall than (B,3), at an additional annual cost of $2,186. Its reported recall is 0.55 for English (11/20), 0.85 for Russian (17/20), and 0.50 for Kazakh (10/20). Kazakh has the lowest observed recall, so improving retrieval for Kazakh questions should be a priority. These language-level estimates are based on only 20 questions per language and therefore have substantial uncertainty.

### 1e. Next measurement (at most 60 words)

Collect at least 100 test questions per language and compare recall@5 for (B,5) and (A,5) using the same questions. Report results separately for English, Russian, and Kazakh, together with confidence intervals. If (A,5) demonstrates a meaningful improvement, reassess whether the additional cost is justified and whether the budget can be increased.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.8833 | KK, degraded | 0.7000 | 0.521 | 0.833 | 44413 |
| L | 0.8167 | KK, degraded | 0.7000 | 0.521 | 0.833 | 18000 |

Personal-data pages per year sent abroad under H: 262800

### 2b. Memo (at most 300 words)
Recommendation: choose pipeline L (our own server in Kazakhstan).

Who is served worst. Both pipelines are weakest on degraded Kazakh scans: 21 of 30 correct (70%, 95% Wilson interval 0.521–0.833). Overall accuracy is 88.33% for H and 81.67% for L. The weakest-cell interval does not establish whether the overall accuracy difference is statistically meaningful. Each weakest-cell estimate is based on only 30 invoices, so uncertainty remains substantial.

Personal data. Under H, 262,800 pages containing personal information are sent abroad each year. Before deployment, the organisation should verify applicable Kazakhstan personal-data requirements and obtain written information from the vendor about data location, retention, model-training use, subprocessors, and deletion. A vendor statement alone is not a complete compliance assessment. Under L, scans remain on a server in Kazakhstan, giving the organisation more direct control over storage and access. A documented retention period and deletion procedure are still necessary.

Verifiable safeguards: (1) Review firewall rules and outbound network logs. (2) Run automatic deletion and retain audit logs. (3) Restrict server access to authorised staff. (4) Review 100 random pages monthly and report accuracy by language and scan condition. (5) Send degraded Kazakh scans for human review.

Choice and residual risk. L costs $18,000 annually, compared with $44,413 for H, and keeps the pages on an organisation-controlled server in Kazakhstan. H has higher observed overall accuracy, so L should be selected with monitoring and human review. Degraded Kazakh scans remain a significant risk, and the organisation must maintain the security of its own server.


## Part 3: Explain your own numbers

### Q1

Number: 1.58

Why: The byte ratio (2.14) is the encoding cost of Cyrillic, but Russian has the same byte ratio and far fewer tokens. The extra 1.58x comes from the tokenizer splitting Kazakh poorly, so the premium is mostly tokenizer, not encoding.

### Q2

Number: 559

Why: No. The chunk is about 657 e5 tokens, so e5 truncates after 512; the sentence starting at 559 is never embedded. A question matching only it cannot retrieve this chunk, and no error appears: truncation is silent.

### Q3

Number: 64.6

Why: Top-p cuts the tail: it keeps only the smallest set of tokens covering 80 % probability, then renormalises, so unlikely tokens become impossible. Lower temperature reshapes all probabilities, sharpening them but leaving every token possible.

### Q4

Number: 28

Why: The leaderboard prefers X (72 vs 66), because abstaining scores zero and a wrong answer costs nothing extra. That teaches models to always guess instead of saying "I don't know", which encourages hallucination.

### Q5

Number: 11.70

Why: Over-trained: about 11.7x more tokens per parameter than Chinchilla's compute-optimal ratio (234 vs 20). Chinchilla argued that models like GPT-3 were too large for their data, undertrained, and parameters and tokens should scale equally.
