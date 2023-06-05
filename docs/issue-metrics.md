# Using issue metrics to measure maintainer responsiveness

![Octocat viewing charts and graphs](/images/Fintechtocat.png)

## Measure what? and why?

In an Open Source Programs Office (OSPO), many times gathering a community of maintainers and giving them support is part of the job.
One key part of that maintainers job is to be responsive to those that interact with the repository.
OSPOs can support maintainers across the business by measuring issue/pr responsiveness and bringing attention or resources to the repos that have response rates lower than desired.
How fast issues/prs should get a response is up to you to set based on the level of maintainership you want to provide.

## Sounds good, how do I measure responsiveness?

The GitHub OSPO has developed the GitHub Action called [issue-metrics](https://github.com/github/issue-metrics) in order to automate this measurement.
The action searches for pull requests/issues in a repository and measures the time to first response for each issue.
It then calculates the average time to first response and writes the issues with their time to first response to a Markdown file.
The issues to search for can be filtered by using a search query.

To see the full details on how to setup the action, [read more on the repository's README](https://github.com/github/issue-metrics#use-as-a-github-action).

## Sample report

![Sample report of the issue-metrics output](/images/sample-report.png)
