## Description
JIRA-XXXXX: Summary statement
- Describe a change

## Testing Notes
- Describe a test

## Checklist

In order to streamline the review of the PR we ask you to ensure the following steps have been taken:

### For all changes:
- [ ] Is there a JIRA ticket associated with this PR? Is it referenced in the commit message?
- [ ] Did you include a brief description of the change?
- [ ] Did you include a brief description of the testing you have completed?
- [ ] Has your PR been rebased against the latest commit within the target branch?
- [ ] Is your initial contribution a single, squashed commit?

### For code changes:
- [ ] Did you run the commands, `dbt run` and `dbt test` and both ran successfully?
- [ ] Did the data load successfully? (i.e. stage, loading, etc.)
- [ ] Did you ensure the code changes worked as expected?
- [ ] Does the code follow our coding conventions?
- [ ] When creating a new model did you add the corresponding yml file with all appropriate testing and documentation?
- [ ] If adding new dependencies, did you update the appropriate requirements file?

### For documentation related changes:
- [ ] Have you ensured that the format looks appropriate for the output in which it is rendered?