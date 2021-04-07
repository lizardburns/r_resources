---
title: "Setting up windows task scheduler to run R scripts"
author: "Stephen J. Price"
date: "2021-04-07"
output: 
  html_document:
    toc: yes
    keep_md: true
---





# Create a project directory for scripts
1. Right click on the "Task scheduler library" in the left-hand panel
2. Select "New folder..." and create a directory

<img src="C:/Users/stephen.price/my_home/resources/r_resources/task_scheduler/create_dir.PNG" width="1318" />

# Click Create task...
1. **General** tab: enter a name and description for task
2. **Triggers** tab: Click "New...", select "Daily" and then enter a date and time, then set an "Expire" date (usually two weeks for consultations)
3. **Actions** tab: 
  - click "New...",
  - You want the default action, "Start a program", which will invoke R from the command prompt
  - paste the path to your Rscript.exe into the Program/script box. This will be something like:
    + `"C:\Program Files\R\R-4.0.2\bin\Rscript.exe"` - **you need the inverted commas if there are spaces in your path!!**
  - paste the command you need R to run into the "Add arguments" box:
    + `-e "source('your_path/taskmaster.R')"`
    + again, the inverted commas are needed

<img src="C:/Users/stephen.price/my_home/resources/r_resources/task_scheduler/create_action.PNG" width="454" />

4. **Conditions** tab: uncheck "Start the task only if the computer is on AC power"
5. **Settings** tab: set up as in screenshot (that's **5** minutes!)

<img src="C:/Users/stephen.price/my_home/resources/r_resources/task_scheduler/settings.PNG" width="631" />

You should be good to go!
