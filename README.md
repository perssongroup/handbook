# Persson Group Handbook

This is the repo for the [Persson Group's handbook site](https://perssongroup.github.io/handbook/), which contains useful information for postdocs and students. The site is hosted on Github Pages, which is powered by Jekyll.

## Contributing
To contribute to the handbook, please fork this repo and make your changes in that branch. After you are happy with your changes, submit a pull request with a short note about the changes you made. Committing directly to this branch is not recommended unless the change is small (fixing typos, etc) or you really know what you're doing. Persson group members should feel free to merge their own pull requests.

## How to add content to the site
Each page on the site is a markdown file in the "content" folder of this repo (i.e. "students.md" specifies the content that is found at perssongroup.github.io/handbook/content/students). To add content to a page, all you have to do is edit that markdown file. You can find a nice markdown tutorial here: https://guides.github.com/features/mastering-markdown/

#### Local Testing
To test your changes locally, install the contents of the requirements.txt file
```
pip install -r requirements.txt
```

Then serve the docs using the following command.
```
mkdocs serve
```

Once the server starts up, visit [127.0.0.1:8000/](http://127.0.0.1:8000/)

#### Deploying
To deploy the mkdocs to [perssongroup.github.io/handbook](https://perssongroup.github.io/handbook/), ensure you have the materialsproject/master branch on your computer. If not, run the following lines of code
```
git clone git@github.com:perssongroup/handbook.git
cd handbook
pip install -r requirements.txt
```
or
```
git clone https://github.com/perssongroup/handbook.git
cd handbook
pip install -r requirements.txt
```

To build and deploy the docs, run the command
```
mkdocs gh-deploy --clean
```

## Where is all the content that used to be here?
The stuff that was previously in the README file is now in index.md.
