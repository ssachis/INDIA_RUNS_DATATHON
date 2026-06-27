# INDIA_RUNS_DATATHON
Initial dataset size=100k \

Candidate pool filtering filters it to top 4k
then final_filtering filters it to top 150
Uptill now the filtering is deterministic
Finally we use LLM ie
openai, to rank the top 100 candidates parsing it along with JD_structured

JD is also passed through LLM to get JD structured ie imp info from it
