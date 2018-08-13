## REPO TEST BRANCH

If you're seeing this version of the README you're in the TEST BRANCH!

DO NOT WRITE CONTENT IN THIS BRANCH.

### Notes on the repo structure/Current issues

The docs folder contains the files to build the product manuals.  Each of the Doc- folders includes symlinks to the topics directory.  

Symlinks issue - Hugo can't follow symlinks, but the ccutil repo structure includes symlinks.  
Solution - Since Hugo can't follow symlinks, the Hugo configuration file is set to ignore this directory.  And the "content" folder has been renamed to "topics".

Attributes issue - Hugo doesn't use the master.adoc files, so wasn't pulling in the document-attributes.adoc file.  
Solution - Move the reference to the document-attributes file to each individual .adoc topic file.

Numbering error message -  Ccutil was throwing the following error:
``` "Invalid format for version. Value ({DocInfoProductNumber}) does not conform to constraint (^[0-9][^\p{IsSpace}]*$) at /bin/publican line 726.""
```
Solution - Because ccutil needs some of the document attributes to populate the master-docinfo.XML file and expects to find those attributes in the master.adoc file, those attributes have also been copied into the master.adoc files.  (Yeah, I did NOT get that from the error message at all!)

Image issue - Images in the topic files will be following the symlink from topics to static/img.  The "support.adoc" file includes an image for testing purposes. This file is included in the Release Notes book.  (This is where I'm currently stuck.)

ccutil error
cp: cannot stat ‘../../docs/topics/images/’: No such file or directory
(I suspect this is part of the underlying tooling expectations from ccutil)

Hugo errors
Getting several errors around sections out of sequence (means I need to chagnge the heading levels in the files)
Also getting error messages about the attributes file, which means the path which works for ccutil isn't correct for Hugo (possibly due to the prescence of symlinks which add the missing "topics" directory to the path?):
asciidoctor: ERROR: <stdin>: line 1: include file not found: /home/jstickle/git/istio-docs/document-attributes.adoc

(The path should be /home/jstickle/git/istio-docs/topics/document-attributes.adoc)

## Current TEST Repository Structure
This structure of this reposity is an attempt to work with both the upstream and downstream toolsets.

Hugo assumes that the same structure that organizes your source content is used to organize the rendered site. 

This repository uses the following directory structure:
```
├── [archetypes] - Can be used to define content, for example you can set default tags or categories and define types such as a post, tutorial or product here.  
├── [data] - Site data such a localization configurations go here.
├── [docs] - Files to build the downstream product manuals, which reference the content directory. 
│   ├── Doc-Installing-ServiceMesh
│   │   ├── master.adoc
│   │   ├── master-docinfo.xml
│   │   ├── buildGuide.sh (script to build this guide)
│   │   ├── topics -> ../topics/ (symlink to content directory)
│   ├── Doc-ServiceMesh-Release-Notes
│   │   ├── master.adoc
│   │   ├── master-docinfo.xml
│   │   ├── buildGuide.sh (script to build this guide)
│   │   ├── topics -> ../topics/ (symlink to content directory)
├── [layouts] -  Layouts for the Go html/template library which Hugo utilizes.  Note that themes override layouts.
├── [public] -  Generated output. (NOTE: This directory doesn't exist until you generate output.)
├── [scripts] - Contains scripts to automate the processes used to create and build documentation.
    └── buildGuides.sh (Builds the top-level Guides that live in the docs/ folder)
├── [static] - Any static files here will be compiled into the final website.
|   ├──  img - This directory contains all the images.  Hugo expects this directory name.
│   │  ├── .png
├── [themes] - This directory contains the theme for the site. (NOTE: Folder is EMPTY until you select a theme.)
├── config.toml - Main Hugo configuration file, used to define the websites title, language, URLs etc.
├── [topics] - Contains all the content files.
│   │   ├── .adoc (AsciiDoc topic files)
│   ├── Subfolders     
│   │   ├── .adoc (AsciiDoc topic files)
│   ├── DRAFTS - This directory is intended for topic stubs, topics that need to be written, and in-progress drafts. The Hugo config file is set to ignore this directory and its contents.  
│   │   ├── .adoc (AsciiDoc topic files)
├── README.md (This file)
```

