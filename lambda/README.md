How to create a lambda expression that requires dependencies

1. Create your script as normal
1. pip install YOURDEPENDENCY -t /path/to/script
1. Select all of the files and folders adjacent to your script and zip them up. Make sure you dont zip the folder that contains all of the files.
1. upload the ziped folder to lambda
1. on the configurations tab, set the Handler to NAMEOFYOURSCRIPT.NAMEOFYOURFUNCTION
