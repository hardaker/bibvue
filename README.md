# bibvue: A web-based bibliography viewer

*bibvue* is a navigable viewer for listing multiple types of
bibliography files.  This is helpful when building web pages of
academic or other work.

## Example

As an example, you can look at [my home page at ISI]:

[my home page at ISI]: https://www.isi.edu/~hardaker/bibvue/

## Installation

You can clone/copy whatever *index.html* and save it where you want it
on your web-page.  Edit the top of the HTML to change the references
to the "*var bibdefs = *" structure to point to your bibliography
files.

## Creating the JSON version

Install the *pandoc-citeproc* package and convert each of your *.bib*
files using:

```
pandoc-citeproc --bib2json myfile.bib > myfile.json
```

