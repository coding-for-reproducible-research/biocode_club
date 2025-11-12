## Maintaining healthy pipelines

Maintaining good pipelines requires continued attention: just like how a plant needs to be regularly watered, nurtured and pruned to keep it healthy! Try to build up these habits to keep your pipeline organized, tidy and working correctly.
Update your README as you go

It’s important to keep the README up to date to accurately reflect how to run the workflow. Remember to update all information required to run the workflow such as:
•	What order to run the pipeline scripts
•	The data sets required to start the workflow
•	The software a user needs to run the pipeline (R packages, command line tools, etc), including any version constraints.
A good practice: at the end of each day that you work on the code, take a few minutes to skim through the README and make any changes required to keep it up-to-date.
Test your code frequently
As you make changes to your code, it’s important to frequently check that not only your new changes work, but that nothing else in the pipeline has broken through those changes!
•	Aim to write scripts that:
o	Run from top to bottom without manual intervention.
o	Log useful messages or errors.
o	Use assertions to automatically check that computations have been performed correctly. 
•	Regularly run the whole workflow end-to-end to ensure it all works together.
•	Where possible, using automated checks and tests in your code is preferable to performing manual checks, since these run every time your code is run (or, in the case of unit tests , can be run separately all in one go).
Keep the project directory clean and tidy
Organised code is easier to understand, maintain and navigate. It also reduces the chance of making errors from running the wrong thing.
•	Use clear and consistent naming for scripts, data files etc. Use descriptive filenames and brief comments to explain the purpose of each file, so others (or future you) can understand the code.
•	Keep old or exploratory work separate from the main workflow code by putting it in a folder like _experimental/.
Keep scripts focussed and strive for clarity
As your pipeline develops, you may find that some scripts start to get long or begin to play multiple roles within the pipeline. Long scripts are harder to navigate and understand, while scripts that blur the logical steps in the pipeline typically make it harder to navigate the pipeline.
•	Don’t be afraid to separate out a long script into a series of shorter, logically cohesive ones.
•	When writing code, use descriptive names rather than obscure / short names. Add comments to explain why something is done a certain way, or to explain what a complicated bit of code is doing.
•	In cases where a script uses variables defined by running another script in the same R session, add comments to indicate which variables are being used and the script they came from. This makes it much easier to understand the code and debug problems.
Reduce redundancy
You may find that as you develop your pipeline, you have parts of your code that essentially repeat the same steps (perhaps just with minor differences). This typically occurs when we copy, paste and tweak code from one place to another. Repeated code is harder to maintain, runs the risk of introducing errors and obscures the logic of what is going on.
•	It is much better practice to encapsulate repeated code into functions. A function is a reusable block of code that does one specific task. You can write it once and use it many times without repeating code. For example, instead of writing the same code to filter data by age in multiple scripts, you can write a function line filter_by_age() and use it wherever needed.
•	As you’re coding, notice repetition and ask: could this be a function?
•	Any functions you create that are used across multiple scripts should be defined once within an R file in the R/ folder for re-use.
•	Similarly, repetition can sometimes occur with variables we define e.g. constants that are used in multiple scripts. These can also be defined within an R file in the R/ folder and brought into scripts, like functions.
