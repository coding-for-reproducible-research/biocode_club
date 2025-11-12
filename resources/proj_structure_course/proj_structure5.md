## How to adapt your existing code into a standard structure

The following steps can help you turn a collection of scripts into one of the standard project structures.

Create a `README` file if one doesn’t exist
*	Open a plain text file and name it README.md (or README.txt if you prefer writing in just plain text). 
*	Fill in the project title, data sources, script descriptions, and any script dependencies as you go through the subsequent steps.
  *	Take this opportunity to record all the software that your pipeline relies upon, including the version numbers, and keep this up-to-date. This should include R packages that the pipeline uses, as well as any other software e.g. command line tools.
Identify the workflow
*	Ask yourself: What is the analysis trying to achieve? Break it down into logical steps (e.g., load data → filter → run model → make plots).
Isolate the first step
*	Find the first logical step in your current code.
*	Copy that code into a new script file named clearly e.g. 01_load_data.R.
*	Take the opportunity to comment the code to make it easier to follow without changing any of its functionality:
 *	Comment distinct sections of the code that represent different steps in the workflow, making it easier to navigate.
 *	Explain why certain steps are done, not just what is being done, to help others (and future you) follow the logic.
 *	Test that the entire script runs from start to finish without errors, producing the expected outputs.
Repeat for each step
*	Continue splitting the original script/s into separate files, one at a time, following steps 5 and 6 above. Each script should perform one of the logical steps identified earlier.
*	Ensure that you run the new scripts to check that they give the expected data sets, models, figures, etc. Also frequently run the full pipeline to check that the new scripts work together correctly and produce the expected outputs for the pipeline.
By the end of the process you should end up with a series of scripts that can be run in order, for example:
 *	01_load_data.R
 *	02_clean_data.R
 *	03_analysis.R
 *	04_visualisation.R, etc.
