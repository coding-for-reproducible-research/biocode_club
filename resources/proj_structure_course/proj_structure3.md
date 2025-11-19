# What to store under version control?

If you are using the version control system Git, then be aware that some of the files / folders in the templates above should be ignored i.e. specified in a `.gitignore` file at the top level of the project. 

In general, do not store the following under version control:
* Anything that can be regenerated using the code in your project folder (e.g. `data/derived`, `outputs/`).
*	Raw datasets, since these are typically large files which Git is not well suited to managing. (Let alone the fact that these files may contain sensitive, personally identifiable data.)
*	Files containing (trained) statistical models, for similar reasons.

If using renv:
*	The `.Rprofile` and `renv.lock` files should be included in version control and distributed with the code.
*	Versioning for the contents of the `renv/` folder should automatically be taken care of, because the folder contains an automatically generated `.gitignore` file ensuring that everything is ignored except for an activation script `renv/activate.R`.

…Otherwise, everything else should be kept under version control.
