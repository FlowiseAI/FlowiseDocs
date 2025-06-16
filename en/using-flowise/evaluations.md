# Evaluations

{% hint style="info" %}
Evaluations are only available for Cloud and Enterprise plan
{% endhint %}

Evaluations help you monitor and understand the performance of your Chatflow/Agentflow application. On the high level, an evaluation is a process that takes a set of inputs and corresponding outputs from your Chatflow/Agentflow, and generates scores. These scores can be derived by comparing outputs to reference results, such as through string matching, numeric comparison, or even leveraging an LLM as a judge. These evaluations are conducted using Datasets and Evaluators.

## Datasets

Datasets are the inputs that will be used to run your Chatflow/Agentflow, along with the corresponding outputs for comparison. User can add the input and anticipated output manually, or upload a CSV file with 2 columns: Input and Output.

<figure><img src="../.gitbook/assets/image (3) (3).png" alt=""><figcaption></figcaption></figure>

| Input                             | Output                       |
| --------------------------------- | ---------------------------- |
| What is the capital of UK         | Capital of UK is London      |
| How many days are there in a year | There are 365 days in a year |

## Evaluators

Evaluators are like unit tests. During an evaluation, the inputs from Datasets are ran on the selected flows and the outputs are evaluated using selected evaluators. There are 3 types of evaluators:

* **Text Based**: string based checking:
  * Contains Any
  * Contains All
  * Does Not Contains Any
  * Does Not Contains All
  * Starts With
  * Does Not Starts With

<figure><img src="../.gitbook/assets/image (6) (2).png" alt=""><figcaption></figcaption></figure>

* **Numeric Based:** numbers type checking:
  * Total Tokens
  * Prompt Tokens
  * Completion Tokens
  * API Latency
  * LLM Latency
  * Chatflow Latency
  * Agentflow Latency (coming)
  * Output Characters Length

<figure><img src="../.gitbook/assets/image (7) (2).png" alt=""><figcaption></figcaption></figure>

* **LLM Based**: using another LLM to grade the output
  * Hallucination
  * Correctness

<figure><img src="../.gitbook/assets/image (9) (2).png" alt=""><figcaption></figcaption></figure>

## Evaluations

Now that we have Datasets and Evaluators prepared, we can start running an evaluation.

1.) Select dataset and chatflow to evaluate. You can select multiple datasets and chatflows. Using the example below, every inputs from Dataset1 will be ran against 2 chatflows. Since Dataset1 has 2 inputs, a total of 4 outputs will be produced and evaluated.

<figure><img src="../.gitbook/assets/image (10) (2).png" alt=""><figcaption></figcaption></figure>

2.) Select the evaluators. Only string based and numeric based evaluators are available to be selected at this stage.

<figure><img src="../.gitbook/assets/image (11) (2).png" alt=""><figcaption></figcaption></figure>

3.) (Optional) Select LLM Based evaluator. Start Evaluation:

<figure><img src="../.gitbook/assets/image (12) (2).png" alt=""><figcaption></figcaption></figure>

4.) Wait for evaluation to be completed:

<figure><img src="../.gitbook/assets/image (13) (2).png" alt=""><figcaption></figcaption></figure>

5.) After evaluation is completed, click the graph icon at the right side to view the details:

<figure><img src="../.gitbook/assets/image (14) (2).png" alt=""><figcaption></figcaption></figure>

The 3 charts above show the summary of the evaluation:

* Pass/fail rate
* Average prompt and completion tokens used
* Average latency of the request

Table below the charts shows the details of each execution.

<figure><img src="../.gitbook/assets/image (15) (2).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/image (16) (2).png" alt="" width="355"><figcaption></figcaption></figure>

### Re-run evaluation

When the flows used on evaluation have been updated/modified, a warning message will be shown:

<figure><img src="../.gitbook/assets/image (17) (2).png" alt=""><figcaption></figcaption></figure>

You can re-run the same evaluation using the Re-Run Evaluation button at the top right corner. You will be able to see the different versions:

<figure><img src="../.gitbook/assets/image (18) (2).png" alt=""><figcaption></figcaption></figure>

You can also view and compare the results from different versions:

<figure><img src="../.gitbook/assets/image (19) (2).png" alt=""><figcaption></figcaption></figure>

## Video Tutorial

{% embed url="https://youtu.be/kgUttHMkGFg?si=3rLplEp_0TI0p6UV&t=486" %}
