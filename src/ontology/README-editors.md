These notes are for the EDITORS of ornaseq

This project was created using the [ontology starter kit](https://github.com/cmungall/ontology-starter-kit). See the site for details.

For more details on ontology management, please see the [OBO tutorial](https://github.com/jamesaoverton/obo-tutorial) or the [Gene Ontology Editors Tutorial](go-protege-tutorial.readthedocs.io)

## Editing Notes

I should probably add the following to the Makefile. For now these steps are manual.

1. Rebuild OntoFox:
 * curl -s -F file=@"imports/terms_input.txt" -o imports/terms_import.owl http://ontofox.hegroup.org/service.php
 * curl -s -F file=@"imports/properties_input.txt" -o imports/properties_import.owl http://ontofox.hegroup.org/service.

Not sure why NCIT_C25402 is flagged as an error in terms_input.

2. Need to edit 'imports/terms_import.owl', resolving following errors (i.e. redundent label fields)
 * 2018-05-17 20:28:17,553 ERROR org.obolibrary.obo2owl.OWLAPIOwl2Obo - Duplicate clause 'name( neuron)' generated in frame: CL:0000540
   > remove this line:         <rdfs:label xml:lang="en">neuron</rdfs:label>
 * 2018-05-17 20:28:17,559 ERROR org.obolibrary.obo2owl.OWLAPIOwl2Obo - Duplicate clause 'name( cell)' generated in frame: CL:0000000
   > remove this line:         <rdfs:label xml:lang="en">cell</rdfs:label>

3. Generate OntoFox obo files
 * robot extract -i imports/terms_import.owl -T imports/terms_list.txt --method BOT -O http://purl.obolibrary.org/obo/ornaseq/dev/terms_import.owl -o imports/terms_import.obo
 * robot extract -i imports/properties_import.owl -T imports/properties_list.txt --method BOT -O http://purl.obolibrary.org/obo/ornaseq/dev/properties_import.owl -o imports/properties_import.obo

4. Prepare release
 * make prepare_release

## Editors Version

Make sure you have an ID range in the [idranges file](ornaseq-idranges.owl)

If you do not have one, get one from the head curator.

The editors version is [ornaseq-edit.owl](ornaseq-edit.owl)

** DO NOT EDIT ornaseq.obo OR ornaseq.owl in the top level directory **

[../../ornaseq.owl](../../ornaseq.owl) is the release version

To edit, open the file in Protege. First make sure you have the repository cloned, see [the GitHub project](https://github.com/safisher/ornaseq) for details.

## ID Ranges

These are stored in the file

 * [ornaseq-idranges.owl](ornaseq-idranges.owl)

** ONLY USE IDs WITHIN YOUR RANGE!! **

If you have only just set up this repository, modify the idranges file
and add yourself or other editors. Note Protege does not read the file
- it is up to you to ensure correct Protege configuration.


### Setting ID ranges in Protege

We aim to put this up on the technical docs for OBO on http://obofoundry.org/

For now, consult the [GO Tutorial on configuring Protege](http://go-protege-tutorial.readthedocs.io/en/latest/Entities.html#new-entities)


## Release Manager notes

You should only attempt to make a release AFTER the edit version is
committed and pushed, and the travis build passes.

to release:

    cd src/ontology
    make

If this looks goo
d type:

    make prepare_release

This generates derived files such as ornaseq.owl and ornaseq.obo and places
them in the top level (../..). The versionIRI will be added.

Commit and push these files.

    git commit -a

And type a brief description of the release in the editor window

Finally type

    git push origin master

IMMEDIATELY AFTERWARDS (do *not* make further modifications) go here:

 * https://github.com/safisher/ornaseq/releases
 * https://github.com/safisher/ornaseq/releases/new

The value of the "Tag version" field MUST be

    vYYYY-MM-DD

The initial lowercase "v" is REQUIRED. The YYYY-MM-DD *must* match
what is in the versionIRI of the derived ornaseq.owl (data-version in
ornaseq.obo).

Release title should be YYYY-MM-DD, optionally followed by a title (e.g. "january release")

Then click "publish release"

__IMPORTANT__: NO MORE THAN ONE RELEASE PER DAY.

The PURLs are already configured to pull from github. This means that
BOTH ontology purls and versioned ontology purls will resolve to the
correct ontologies. Try it!

 * http://purl.obolibrary.org/obo/ornaseq.owl <-- current ontology PURL
 * http://purl.obolibrary.org/obo/ornaseq/releases/YYYY-MM-DD.owl <-- change to the release you just made

For questions on this contact Chris Mungall or email obo-admin AT obofoundry.org

# Travis Continuous Integration System

Check the build status here: [![Build Status](https://travis-ci.org/safisher/ornaseq.svg?branch=master)](https://travis-ci.org/safisher/ornaseq)

Note: if you have only just created this project you will need to authorize travis for this repo. Go to [https://travis-ci.org/profile/safisher](https://travis-ci.org/profile/safisher) for details

