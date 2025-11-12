## How to start a new workflow using a standard structure

If you're beginning a new project, it's the perfect opportunity to start with a logical, modular structure according to one of the standard templates. Here are some tips to get you going.
Set up your project folder

*	Create a new directory with a clear, descriptive name, e.g., `~/projects/methylation_analysis/`
*	Inside it, create the standard subfolders and files according to one of the templates above.
Draft your workflow plan
*	Think through the steps you'll need (e.g. import data → clean data → analyse data → plot results)
*	Write down a rough plan in the `README`, including things like:
  *	What each step does
  *	What files / data each step requires and produces
  *	Software tools/libraries to be used (if known ahead of time)

Create one script per step:
*	Use clear names with numbered prefixes (e.g. `01_load_data.R`, `02_filter_samples.R`, `03_analysis.R`, `04_plot_results.R`)
*	Each script should:
  *	Be runnable from beginning to end as a complete R script without manual intervention (i.e. don’t rely on highlighting lines within R Studio and running these separately).
  *	Be easy to read: using descriptive variable names, including comments where helpful to understand what’s going on, etc.
*	Consider how data should best be passed between the scripts i.e. how the scripts will interface with each other. Two approaches are:
  *	Have scripts save data to intermediary files (e.g. CSV / TSV), for subsequent script(s) in the pipeline to read from. This gives the option to run scripts in the pipeline without having to run previous scripts, which can save on runtime. It also allows you to export datasets for other analyses.
  *	Have the scripts run sequentially within the same R session, so that they share objects/variables in the same environment. This is simple, though requires clear documentation within scripts to say where they are getting their variables from.
