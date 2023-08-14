# Fidel documentation

This repository is the home for the Fidel documentation located [here](https://fidel.uk/docs/). The format of this documentation is [MDX](https://mdxjs.com/), this enhances standard markdown with functionality that enables us to create richer and more interactive documentation. MDX is indicated by the presence of `<Tags>` within these files. If you wish to contribute please be aware of this, in most cases you should not need to touch any MDX however if you do we will assist you in the pull request.

## Steps to test the docs on your local machine

1. Clone the [website](https://github.com/FidelLimited/website/) project.

2. Install libraries, dependencies & follow the instructions that indicate how to run the website on your local machine, as written in the [README.md file](https://github.com/FidelLimited/website/blob/master/README.md) of the website project.

3. From the website's project root folder:
    
    a. Run `env DOCS_BRANCH=the-docs-repo-branch-you-want-to-test yarn docs` to download the docs files from a docs repo branch.

    b. Go to src/pages/docs to check the changes in the docs' files.
    
    c. Run `yarn start` to build the project

4. Wait until you see a message giving you a localhost URL that can be used to test the website. It should be something like the following:

```
You can now view fidelapi.com in the browser.

  http://localhost:8000/
```
5. Open `http://localhost:8000/` in your browser.

6. Make any changes in the `src/templates/docs/routes.ts` file, if the navigation to your documentation pages has changed or if you added new pages.
