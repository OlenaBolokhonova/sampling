# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Olena Bolokhonova

```
1. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post: 
Step 1: Setting Up the DataFrame
Function Used: pd.DataFrame
Sample Size: 1000 individuals
Sampling Frame: The entire population of event attendees, consisting of two event types: weddings and brunches.
Procedure:
The script creates a DataFrame representing 1000 individuals attending events.
200 individuals are assigned to weddings, and 800 to brunches.
Underlying Distribution: Deterministic allocation (200 weddings, 800 brunches).

Step 2: Infecting a Random Subset of Individuals
Function Used: np.random.choice
Sample Size: 10% of the population (100 individuals), as defined by the ATTACK_RATE.
Sampling Frame: The entire population of 1000 individuals.
Procedure:
Randomly selects 100 individuals to be infected.
Underlying Distribution: Uniform random selection without replacement.

Step 3: Primary Contact Tracing
Function Used: np.random.rand
Sample Size: A subset of the infected individuals (20% of infected individuals), as defined by the TRACE_SUCCESS.
Sampling Frame: The subset of infected individuals.
Procedure:
For each infected individual, a random number is generated to decide whether they are traced. If the random number is less than TRACE_SUCCESS (0.20), the individual is traced.
Underlying Distribution: Bernoulli distribution (each infected individual has a 20% chance of being traced).

Step 4: Secondary Contact Tracing Based on Event Attendance
Function Used: value_counts, loc
Sample Size: Events with at least two traced individuals.
Sampling Frame: The set of events attended by traced individuals.
Procedure:
Counts the number of traced individuals per event.
Events with two or more traced individuals undergo further tracing. All infected individuals at these events are traced.
Underlying Distribution: The distribution of traced individuals per event determines the events selected for secondary tracing.

Step 5: Calculating Proportions of Infections and Traces
Function Used: sum
Sample Size: All infected individuals and all traced individuals.
Sampling Frame: The entire population of individuals.
Procedure:
Calculate the proportions of infections and traces attributed to weddings versus brunches.
Underlying Distribution: Proportions calculated from the sampled data of infections and traces.

2. Does this code appear to reproduce the graphs from the original blog post?
The outputs are different from those presented in the blog post

3. Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results:
Without setting a random seed or similar configuration, discrepancies are observed in the generated graphs each time the script is run, highlighting the need for further investigation to identify and address the sources of variability.

4. Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file:
Set the random seed for each iteration using np.random.seed(seed + m).
Set a seed at the beginning of the script to ensure overall reproducibility.
Using np.random.seed(seed + m) for each iteration ensures that each iteration has a seed, providing diverse random sequences while maintaining reproducibility.
Also, imported the logging module.
Configured the logger with a specific format and level.
Add logging statements at key points in your script to track the flow of execution and important events.
I used a logger to track the execution of the script. It can help understand what the script is doing and when, and it can be invaluable for debugging and verifying the reproducibility of the results.

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
