# Vectara Question-answering Chain

A chain for performing question-answering tasks with Vectara.\

## Definitions

**A retrieval-based question-answering chain**, which integrates with a Vectara retrieval component and allows you to configure input parameters and perform question-answering tasks.\

## Inputs

* Vectara store Retriever

## Parameters

* Summarizer Prompt Name: an optional Vectara prompt name to be used in generating the summary.
* Response Language: a string containing the ISO 639-2 code for the desired language for the response
* Max Summarized Results: number of top results to use in summarization (defaults to 7)

## Outputs

Retrieves previously loaded data from a Vectara store, to answer user queries.
