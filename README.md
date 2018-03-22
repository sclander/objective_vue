# objective_vue

> Coding exercise for Objective inc

## Overview

Most of this project was set up automatically through the Vue cli. The important files for review are:

* index.html - actual index file where CSS is loaded, Vue instance will be bootstrapped to this.
* src/App.vue - main Vue component, loads JSON data and passes it into the ApplicantTable component
* src/components/ApplicantTable.vue - the important vue component that contains all of the table generation and display logic
* src/data.json - JSON data file from the tarball

## Methodology

I built this with the thought in mind that the setup should be able to display
the proper data for any arbitrary JSON file of the same format. You can test this
funtionality either by altering the data.json file or replacing it with a new 
data set of the same form. Though this now only simulates an AJAX call, this project
could be easily extended to read data from any source.

The general data parsing process is as follows:
1. Import raw json data (App.vue)
2. Reformat data to a hierachical structure of jobs > applicants > skills (ApplicantTable.vue)
3. Calculate totals from formatted data (ApplicantTable.vue)

## Further Improvements

The rows for the table are generated based on iterating over the skills of each applicant in the formatted data. This was done to keep the semantics of multiple ```<tr>``` elements from the source index.html.

This introduces a potential bug: if an applicant has no associated skills, that 
applicant will currently not show up in the table. For the purposes of this exercise, I made the assumption that all applicants would always have at least one associated skill. If in practice, this assumption might be untrue, the code would need to be altered to account for it.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).


