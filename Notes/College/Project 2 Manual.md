**Using the App**
To start the application you must have population.json and grades.json in the src directory. These data files are loaded into local storage on startup and determine the initial state of the app. Without these files behavior is undefined. 

After starting the program with ```npm start```, the following screen should be visible: 

![[Pasted image 20240409233352.png]]

The left panel is the editor and the right is the graph viewer. The right column is the labels of the bars, the "X" value of that bar. The left column is the height of the bars, the "Y" value of that bar. The X column can contain any type of data while the Y column must contain numerical data. Any non-numerical data inputted as a Y value will result in that bar having a value of zero. The axes headings above each column show the units that the data will be expressed in. 

Any values changed in the editor are dynamically updated in the graph viewer. For example, we could change the population in 1950 to 1.98,  or change 1970 to "leap year" and the graph would be updated accordingly. The title can also be modified in the same way, however the axes cannot.

![[Pasted image 20240409234101.png]]

New datapoints can be added by clicking the "add row" button, which creates a new row with X and Y value 0:

![[Pasted image 20240409234227.png]]

Similarly, datapoints can be deleted by clicking the large X button next to each row:

![[Pasted image 20240409234257.png]]

Once the user is done editing their graph, they can save and load it using the "File" button in the top left corner, which has three options when selected:

![[Pasted image 20240409234348.png]]

Load pulls up a menu of saved datasets. At first only "population.json" and "grades.json" will be available. A new dataset can be loaded by clicking on it. Click cancel to exit the menu.

![[Pasted image 20240409234456.png]]

Save saves your changes to the existing file. Save As pulls up an additional menu, asking you to enter a filename. It then saves your graph as a new dataset with the inputted filename. This dataset can be accessed later through the "load" menu.

![[Pasted image 20240409234857.png]]

![[Pasted image 20240409235037.png]]

**App API**
The app has a very simple API with three routes: GET, POST, and PATCH. All of these are routed to the index '/', with the only difference being the request type. A GET request is used for initially fetching the datasets, POST for creating new datasets, and PATCH for modifying existing datasets. Both POST and PATCH return the entire list of datasets after running so the frontend can update without needing to make a second GET request. 