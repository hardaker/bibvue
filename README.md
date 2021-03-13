# bibvue: A web-based bibliography viewer

*bibvue* is a navigable viewer for listing multiple types of
bibliography files.  This is helpful when building web pages of
academic or other work.

## Example

As an example, you can look at [my home page at ISI]:

[my home page at ISI]: https://www.isi.edu/~hardaker/bibvue/

## Usage

1. Create a json file for each bibliography you wish to display using
   **pandoc-citeproc --bib2json myfile.bib > myfile.json** to convert
   your latex .bib files.
2. Edit the top of *src/components/BibVue.vue* to add in all of the
   json files you wish to include and optionally set which fields you
   always want to be shown.
3. Add any personal header information to the
   *src/components/PersonalHeader.vue* file.
4. You can optionally use **make serve** to run a temporary webserver to
   check the results first
5. Run **make dist** which will
    1. run **npm install** to pull in the needed npm packages (it will
       only do this once)
    2. create your finished webpage in a *dist/* subdirectory
6. Copy the contents of the dist/ subdirectory to where you wanted it
   hosted on your webserver
7. Profit!

