# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 100 (from the original 1000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Aleksandra Fomina

```
Part 1.
Stages that involve sampling:
1. Infecting a random number of people:
sampling frame: all individuals attending the event (200 - weddings; 800 - brunches)
sample size: defined by ATTACK_rate which is 10% of the total population
sampling procedure: random sampling without replacement (np.random.choice)
distribution: uniform distribution (each individual has an equal chance of being infected)

2. Primary contact tracing:
sampling frame: the set of infected individuals (ppl['infected'] == True)
sample size: defined by TRACE_SUCCESS (0.20); if an infected individual is randomly assigned a number less than 0.2, then the individual is traced
sampling procedure: random sampling using np.random.rand(sum(ppl['infected'])) function and comparison with TRACE_SUCCESS
distribution: binomial distribution (each infected individual can either be traced or not with the success probability of 20%)

3. Secondary contact tracing: 
sampling frame: infected and traced individuals
sample size: the number of events that have at least SECONDARY_TRACE_THRESHOLD (2) traced individuals, who are then considered for secondary tracing. 
sampling procedure: events with more than or equal to 2 traced individuals are marked for secondary tracing (event_trace_counts[event_trace_counts >= SECONDARY_TRACE_THRESHOLD]) and all infected individuals at this event are then traced.
distribution: the selection process is deterministic and relies on event counts and the threshold value rather than any random distributions

Similar to Andre Whitby's post, the simulation model highlights again that contact tracing introduces bias in sampling and distorts the event significance in infection spread. Primary tracing selects only a random subset of infected individuals, while secondary tracing contributes more to this bias by focusing on events with a higher number of traced individuals.

Part 2.

The shapes of distributions for the true proportions of infections look similar centering around 20%, with some variation around. However,the frequency of proportions are different since the simulation was run over 50000 trials not 1000 like in this script (but it is hard to tell by how much they differ since the y-axis is missing in the post's plots). Besides, since the simulation uses random sampling, each run may yield slightly different distributions.
"Traced back to weddings" distributions look very different in their shapes (inluding means and variation around) and frequencies too. In our simulation, the traced distribution resembles closer the true proportions of infections distribution. This is could be due to the difference in methodology and our model is more simple. 


Part 3.

The distribution plots exhibit similar overall trends. However, the exact numbers and proportions in the graphs differ and there is more variability between runs compared to running it 1000 times. This could be due to the smaller sample size, and the output show more fluctuations in the exact proportions of cases. Some variation in the output graphs between runs can also be due to random sampling used in the simulation. 

Part 4.

I set the random seed number to make the script reproducible. By setting the random seed, every time the script runs, it will generate the same sequence of random numbers, meaning that the output graphs will be reproducible across multiple runs. The individuals who are infected, traced, and the proportions of infections and traces will be the same each time you execute the script. 

```


## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 09/04/2025`
* The branch name for your repo should be: `assignment-1`
* What to submit for this assignment:
    * This markdown file (a1_sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-1`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via the help channel in Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
