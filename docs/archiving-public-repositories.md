GitHub has an archived repositories feature to indicate and make unmaintained repositories read-only. You can pair this feature with an automated report that gives you a list of the inactive repositories that are in your organization. Check out that action on the [GitHub Marketplace](https://github.com/marketplace/actions/stale-repos).

In order to archive a github organization repository:

1. Edit the README to add a note:

   ```md
   **NOTE**: _This repository is no longer supported or updated by <COMPANY_NAME>. If you wish to continue to develop this code yourself, we recommend you fork it._</code>
   ```

2. Then open a pull request with that change. Here's the text we've been using for the pull request:

   ```md
   This repository is no longer being maintained. This pull request makes it explicit in the `README.md`. Let me know if you have any reservations, otherwise I'm going to archive this repository.

   /cc any relevant teams or recent <COMPANY_NAME> committers
   ```

3. Let it sit until you receive confirmation from a maintainer or an adequate length of time has passed.

4. Close all open issues and pull requests and archive the repository from the repository settings.

Note: It is important to typically choose to **Archive** unmaintained repositories instead of deleting them. When you delete a public repository, the oldest, [active public fork is chosen to be the new upstream repository](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/what-happens-to-forks-when-a-repository-is-deleted-or-changes-visibility). All other repositories are forked off of this new upstream and subsequent pull requests go to this new upstream repository. This may have security and reputational risks to your organization. If you must delete a repository, work with your OSPO or similar team to understand the implications of this process.
