# FESTU.Navigator


The project, developed for a FESTU university (city Khabarovsk) competition, implements AR navigation through the FESTU university buildings along a pre-created corridor graph. 
Work with AR is done entirely using the SceneView library

**Video demonstration available below**

The application builds routes between university classrooms. When launching the application, it is required to scan the number of the nearest classroom in order to set the starting point of the route. 
Classrooms number recognition performed using Google MLKit. 

**By default, the recognizable classroom number must be vertical and not contain letters in the name**

<p align="middle">
  <img src="https://user-images.githubusercontent.com/35885530/179328286-371043fc-a101-46d3-a3da-b1d2885b9ee4.png" width="300" />
</p>

<br>

It is required to confirm the recognized classroom number and its position in 3D space, since an error is possible in determining the planes around the user

<p align="middle">
  <img src="https://user-images.githubusercontent.com/35885530/179328379-60c46df3-bdfd-42f4-a534-c5f79e4cd218.png" width="300" />
</p>

<br>

Initialization is required to determine the user's current position relative to the saved classrooms graph. After initialization, the user can select both the start and end points of the route

<p align="middle">
  <img src="https://user-images.githubusercontent.com/35885530/179328505-4ae5348e-0e3d-4cd9-ab29-5543bf37bfc3.png" width="200" />
</p>

<br>

The built route thanks to the SceneView AR library is displayed in 3D space in front of the user. Finding the shortest route is done using the A* algorithm, path smoothing is done using Bezier curves. 

<p align="middle">
  <img src="https://user-images.githubusercontent.com/35885530/179328566-72e66c07-088d-498d-a7eb-955cbcbf8fb4.png" width="300" />
</p>

<br>

The classrooms graph is created manually by the application administrator through a special interface

<p align="middle">
  <img src="https://user-images.githubusercontent.com/35885530/179328653-132d6894-bd8c-4a39-a8a5-d38374a82c77.png" width="300" />
</p>

<br>

## Video demonstration
Video demonstration: https://drive.google.com/file/d/1D1yIw9z9UXiHfDxtPgEj0x_9gJDrNFvz/view?usp=sharing  

## Settings
You can change the path rendering distance (increase the number of nodes) and other settings, in festunavigator/presentation/preview/PreviewFragment.kt:
```kotlin
360 companion object {
361        //path rendering distance (number of nodes)
362        const val VIEWABLE_PATH_NODES = 21
363        //tree rendering distance, used only in admin mode
364        const val VIEWABLE_ADMIN_NODES = 5f
365        //how often the check for path and tree redraw will be
366        const val POSITION_DETECT_DELAY = 100L
367        //image crop for recognition
368        val DESIRED_CROP = Pair(8, 72)
369    }
```
You can change the classroom number template (first char must be a digit by standard) in festunavigator/data/ml/classification/TextAnalyzer.kt:
```kotlin
143    private fun filterFunction(text: Text.TextBlock): Boolean {
144        return text.text[0].isDigit()
145    }
```

## Admin/User mode
To enable the audience graph editing mode, in festunavigator/data/App.kt find:
```kotlin
70 const val mode = USER_MODE
```
Change it with:
```kotlin
70 const val mode = ADMIN_MODE
```
By default, the application is installed with a pre-installed FESTU university classrooms graph. To run the application without a pre-installed graph, in festunavigator/data/App.kt find:
```kotlin
35 database = Room.databaseBuilder(this, GraphDatabase::class.java, DATABASE_NAME)
36     .createFromAsset(DATABASE_DIR)
37     .allowMainThreadQueries()
38     .build()
```
Remove line - .createFromAsset(DATABASE_DIR):
```kotlin
35 database = Room.databaseBuilder(this, GraphDatabase::class.java, DATABASE_NAME)
36     .allowMainThreadQueries()
37     .build()
```

Now you can run the app and experience it anywhere
If you have any questions, you can email me: gerbort111@gmail.com



