# Midterm 1: answers

GitHub username:sharapatdintansulu-lang
Session code:KETTLE-62
Fingerprint:6078b04d

> Fill in the three lines above from `my_data/DATA.md`. Do not rename headings or table columns: a script reads the numbers.
> Write only in the empty cells and lines. Percentages as 0.78 or 78 %, money as plain numbers.

## AI-use declaration
I used claude as an assistant during the exam.I used it to explain the task and the formulas,write Python code for Google Colab and check my calculations.I ran the code myself on my own data.The decisions,the memo and the explanations in Part 3 are based on my own reading of the numbers.
(About half a page: which assistants or tools you used and for what. If you used none, say so.)

## Part 1: Retrieval and cost decision

### 1a. Metrics (all 60 questions)

| config | recall@1 | recall@3 | recall@5 | MRR@10 |
|---|---|---|---|---|
| A |0.5333 |0.6167 |0.6833 |0.5929 |
| B |0.3500 |0.6333 |0.7167 |0.4907 |

### 1b. Recall and 95 % Wilson interval of each candidate (recall at its own k)

| candidate | recall@k | lower | upper |
|---|---|---|---|
| A, k=3 |0.6167 |0.4902 |0.7291 |
| A, k=5 |0.6833 |0.5577 |0.7869 |
| B, k=3 |0.6333 |0.5068 |0.7438 |
| B, k=5 |0.7167 |0.5923 |0.8149 |

### 1c. Annual model cost in USD

| candidate | annual cost |
|---|---|
| A, k=3 |17178.85 |
| A, k=5 |22480.07 |
| B, k=3 |11133.62|
| B, k=5 |12404.68 |

### 1d. Decision (at most 150 words)

Chosen candidate:B, k=3
All four 95 % Wilson intervals overlap (B,3: 63 % [51-74]; B,5: 72 % [59-81]; A,3: 62 % [49-73]), so the data do not show that any candidate is better. A,5 costs $22,480, over the $19,800 budget. Among the affordable ones I take the cheapest: B,3 at $11,134 a year, about $6,000 below A,3. Recall of B,3 by language: EN 80 % [58-92], KK 65 % [43-82], RU 45 % [26-66]. Russian is served worst: fewer than half of the questions get the right chunk in the top 3, and the overall 63 % hides this. B,5 costs $1,271 more and gives RU 65 %, but with 20 questions per language this is not proven.

(Decision, using your intervals and the budget. Give the recall of your choice for EN, RU and KK separately.)

### 1e. Next measurement (at most 60 words)
Collect about 100 more labelled questions per language, especially Russian, and rerun B,3 and B,5. If recall@5 for Russian is clearly higher with non-overlapping intervals, switch to B,5, which is still within the budget.

## Part 2: A document pipeline you can defend

### 2a. Numbers

| pipeline | overall accuracy | weakest cell (language, condition) | accuracy there | lower | upper | annual cost |
|---|---|---|---|---|---|---|
| H | 0.906 | KK, degraded | 0.833 | 0.664 | 0.927 | 5566.25 |
| L | 0.800 | KK, degraded | 0.700 | 0.521 | 0.833 | 18000 |

Personal-data pages per year sent abroad under H:146000

### 2b. Memo (at most 300 words)
Pipeline L, with human review of Kazakh scans.

Worst served. Overall accuracy: H 90.6 % (163/180), L 80.0 % (144/180). Averages hide weak groups. L is worst on Kazakh: 21/30 = 70 % in both conditions (95 % interval 52-83 %). H is worst on degraded Russian and Kazakh scans: 25/30 = 83 % (66-93 %). These intervals overlap, so I cannot say H is better for Kazakh; with 30 scans per cell I am only sure that L is weaker overall.

Personal data. H: the scan goes to a data centre abroad; 146,000 pages a year carry a name and ИИН. Who stores it and for how long I do not know; the vendor must confirm in writing, and that is not a compliance review. L: the scan stays on our server in Kazakhstan, we store it and set retention; I still must check backups and access logs.

Safeguards: (1) automatic deletion of scans after a set retention period, checked by a quarterly log audit; (2) a named-staff access list, reviewed quarterly; (3) manual check of all Kazakh scans, error rate measured monthly; (4) legal review before any switch to H.

Choice. H costs $5,566 a year, L $18,000. I choose L because 146,000 ИИН-bearing pages a year would leave the country without review. Residual risk: up to 30 % of Kazakh invoices may be misread if review misses them, and we pay about $12,400 more per year.

## Part 3: Explain your own numbers

### Q1

Number:2.62
Why (at most 40 words):Output tokens cost 6 times more than input, and KK output grows only 1.57 times while KK input grows 3.08 times. Pricing weights the slower-growing output, so the bill ratio (2.62) is below the token ratio (2.98).

### Q2

Number:874
Why (at most 40 words):No. e5 reads only the first 512 tokens; this sentence starts at token 874, so it is never embedded and cannot influence retrieval. There is no error: truncation is silent, the chunk just looks like its beginning.

### Q3

Number:53.5
Why (at most 40 words):Dividing scores by T<1 preserves the ranking, since it is monotonic. It only sharpens the distribution toward the top token. All five tokens keep non-zero probability, so the sampleable set is unchanged.


### Q4

Number:27.2
Why (at most 40 words):Left out arithmetic sequences and two-digit multiplication: they follow rules, so a model can learn them rather than memorise arbitrary facts. The instruction doesn't create missing knowledge, so the bound stays.

### Q5

Number:36946
Why (at most 40 words):By turn 28 the first message has been billed 28 times, once per turn. Prompt caching bills the repeated prefix at a reduced rate, so the cost drops, but the tokens sent stay the same.
