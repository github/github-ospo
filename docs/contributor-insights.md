# Contributor insights

Contributors are an essential part of a thriving open source community. Without them, it doesn't really feel like open source! In light of that, OSPOs and open source maintainers often research their contributors and monitor contributions as a way of indicating project health.

In the GitHub OSPO we have developed a tool that we use in order to get information on a project or organizations contributors. We maintain several open source repositories and have developed this action to empower maintainers to measure how many new and returning contributors and contributions have occurred over any given time period.

## Contributors action
Our goal as an OSPO is to use the contributors action in order to:

- **Identify candidates** for [onboarding new maintainers](https://github.blog/2023-06-01-elevating-open-source-contributors-to-open-source-maintainers/)
- Make it easier to **say thank you to new and returning contributors**
- **Signal when contributors are increasing or decreasing** in a given project
- **Understand when projects need additional support** from an Open Source Programs Office (OSPO) or Foundation

### What can you measure?

With the contributors GitHub action, you can gather data on:

- The total number of contributors.
- The total contributions made by these contributors.
- The percentage of new contributors.
- Individual contributor activity, including the number of commits.
- How to sponsor your contributors via GitHub Sponsors.

The action protects contributor privacy by not supplying any information that is not publicly available on the user’s profile or a repository’s commit history.

### How to use the contributors action

Interested in implementing the contributors action in your project or organization? Here's a step-by-step guide to get you started:

1. Create a Repository
Start by creating a new repository to host the contributors GitHub action or select an existing one. You can also run this action on an entire organization.

2. Choose an Example Workflow
Select the best-fit workflow file from the [examples](https://github.com/github/contributors/blob/main/README.md#example-workflow) provided and customize it to your needs with the ["Configuration" section of the contributors action README.md file](https://github.com/github/contributors/blob/main/README.md#configuration).

3. Copy and Edit the Workflow
Copy the chosen example workflow into your repository and save it in the .github/workflows/ directory with the .yml file extension. Edit the values in the workflow to match your specific needs, including the organization or repository you want to measure, start and end dates (if applicable), and other configurations.

4. Set the GitHub Token
Ensure that you have a GitHub Token with the necessary permissions to read the repository or organization you're interested in scanning. If you're running the Action on a different repository or organization, create a GitHub API token and store it as a repository secret, referencing it in your workflow.

5. Commit and Trigger the Action
Commit the workflow file to your default branch and wait for the Action to trigger based on your specified schedule or manually initiate it via a [workflow_dispatch](https://docs.github.com/en/actions/using-workflows/manually-running-a-workflow).

## `CONTRIBUTING.md` files for every project

Another tool that we use that helps us to gain more contributors is the [Automatic Contributing.md action](https://github.com/github/automatic-contrib-prs). It opens up a pull request with a template `CONTRIBUTING.md` file in every repo in an organization that doesn't already have one.

This is important because new would-be contributors are often looking for guidance on how to become a contributor to a project. Managing a lot of open source repositories, it can be difficult to make sure that each of them have this documentation and that it is customized to the needs of the project.

To learn more about using this action, check out the [README](https://github.com/github/automatic-contrib-prs#readme).
