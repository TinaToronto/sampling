# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Answer: 
1. Event Sampling
a. Sampling Procedure: The initial set-up in the simulate_event function involves creating a DataFrame ppl representing individuals attending events (either weddings or brunches).
b. Sample Size: The sample consists of 1,000 individuals, with 200 attending weddings and 800 attending brunches.
c. Sampling Frame: This frame represents the entire hypothetical population of individuals in a single time period.
d. Distribution: A fixed distribution is assigned, where 20% of individuals are allocated to weddings and 80% to brunches.
e. Relation to Blog Post:  The event sampling setup mirrors the blog’s structure of a “toy model,” which categorizes individuals into different types of events (weddings vs. brunches) with set proportions. 

2. Infection Sampling
a. Sampling Procedure: A subset of individuals is marked as infected based on a set infection rate (ATTACK_RATE). This is done by randomly selecting indices from the ppl DataFrame using np.random.choice.
b. Sample Size: 10% of the total individuals (100 people) are sampled as infected.
c. Sampling Frame: The sampling frame includes all individuals in the event population.
d. Distribution: Infection is randomly assigned with a probability of 0.10 across all individuals, independent of event type.
e. Relation to Blog Post: The infection sampling procedure aligns with the blog’s assumption that a set proportion of attendees at each event type are infected.

3. Primary Contact Tracing Sampling
a. Sampling Procedure: For those marked as infected, primary contact tracing determines whether they are “traced” based on TRACE_SUCCESS, which is a 20% chance. This is performed by randomly assigning tracing outcomes to infected individuals.
b. Sample Size: Only 20% of the infected individuals (expected to be about 20 cases) are likely to be traced initially.
c. Sampling Frame: The sampling frame is the subset of infected individuals.
d. Distribution: Each infected person has an independent 20% probability of being traced, using a binomial sampling approach.
e. Relation to Blog Post: This step mirrors the real-world contact tracing limitations mentioned in the blog, where only a subset of cases is effectively traced.

4. Secondary Contact Tracing
a. Sampling Procedure: If an event (e.g., a wedding) has two or more traced cases, secondary contact tracing applies, marking all attendees of that event as traced if they are also infected. This procedure identifies clusters of infections within traceable events and ensures all infected individuals at these events are recorded.
b. Sample Size: This step depends on which events exceed the threshold for secondary tracing, potentially including all infected individuals at these events in the sample of traced cases.
c. Sampling Frame: The frame includes all infected individuals at events that exceed the tracing threshold.
d. Relation to Blog Post: Secondary contact tracing amplifies the perceived importance of specific event types, like weddings, that are easier to trace and meet the tracing threshold, as noted in the blog.  

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Answer: This code does not appear to reproduce the graphs from the original blog post.

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Answer: 
Reducing the number of repetitions in the simulation from 50,000 to 1,000 impacts the statistical stability and reproducibility of the graphs. With only 1,000 simulations, the model has fewer trials to average out random variations, which can cause noticeable fluctuations between runs. Here's my observation. 
1. Greater Variability in Distributions: Each time you run the simulation with 1,000 iterations, the shapes of the histograms might differ slightly. The peaks, spread, and center of the distributions for both infection proportions and traced cases are likely to shift between runs.
2. Less Reliable Mean Values: With only 1,000 repetitions, the average proportion of cases attributed to weddings in both infection and traced cases graphs may vary more noticeably around the expected values.
3. Reproducibility Across Runs: The results from each 1,000-iteration run may not fully replicate the insights from the original blog post. This lack of reproducibility is typical when the sample size is too low to stabilize around true proportions, and statistical noise overshadows patterns seen in larger trials.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

Answer: 

To ensure reproducibility in the code, we need to set a random seed before any random sampling occurs. This will fix the random number generation sequence, making the script produce the same output every time it’s run, even with lower repetitions.

With this change:

a. Every run of the script with the same random seed (42 in this case) will produce the same histogram and values in props_df, even if executed on different machines or at different times.
b. Running the script multiple times will yield identical results, addressing variability and making testing, debugging, or reporting consistent.
c. By controlling randomness, we effectively stabilize the results of the model simulation, ensuring each run reflects the same underlying distributions.

# Author: YOUR NAME

```
Please write your explanation here...

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
