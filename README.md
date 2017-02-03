# Graphics Test Framework Usage Guide & FAQ


#### Creating a new Test Suite
---

* Fork this repository in to your own Github account
* Open the project in Unity and open scene `Persistent.unity`
* Select the object `GlobalController` and set the following values on `Global_Controller.cs`
    * **Suite Name** - Sets the name for this test suite. Used for organisation of result data.  
    * **Comparison Resolution** - Sets the resolution used for taking images for graphics tests (this is temporary, will be moved to UI)
* Start making tests (See *Making new tests*)

---

#### Making new tests
Before creating new tests it is recommended that you run the project with the included Example tests to understand how the system works.

---
##### Graphics Tests
A graphics test is an image comparison test. It captures the output of a cameras render target and writes it to a file. This can be used to create reference images to compare against in subsequent runs.
* Open the template file `Graphics_Test.unity` from the `TestTemplates` folder
* All scenes added to build settings with the prefix `Graphics_` will be automatically added to the graphics test menu. Make sure they follow the conventions outlined below.
* Notice the composition of this scene file
* The scene contains one instance of `Graphics_Controller` (There is a prefab for this in the `Prefabs` folder). This is the object that handles all of the graphics tests.
* As well as this object there is some `TestGroup` objects with the tag `Test Group`. These are used to group the tests in the results. **The name of the group comes from the name of this object**.
* Each of these Test Groups has any number of children with the script `Graphics_Test.cs` attached. Notice the `TestCamera` variable on that script. **This variable must be set by hand** (so its easier to duplicate a "Template" test and fill it with your test objects) as the test may contain any number of cameras at lower depths (to sample multiple cameras at once). **The name for each test comes from the name of this object**
* Following these hierarchy/tag/naming conventions, create groups and tests containing all your test data. **Graphics tests are run across multiple scenes giving you 3 abstraction levels:**
    * Scene (known as *Set*)
    * Test Group
    * Test
* If you click the Cog button on the script `Graphics_Controller` at the bottom you will see the option `Get Tests`. This button will clear the current test list for this scene and get a new test list. It does this by trawling the hierarchy for objects with the tag `Test Group` and then its children with the script `Graphics_Test.cs` attached. **This requires all these GameObjects to be enabled.** After clicking the button the Graphics Controller will now have a full test list. **Now Disable all the test groups in the scene before saving**.
* This scene is now ready for graphics testing!

---
##### Specialist Tests
A specialist test is a more flexible manual test structure. It contains no automation and is merely a platform for viewing results manually.
* Open the template file `Specialist_Example_Test.unity` from the `TestTemplates` folder
* All scenes added to build settings with the prefix `Specialist_` will be automatically added to the specialist test menu. Make sure they follow the conventions outlined below.
* **Specialist scenes should also contain a second prefix to use for abstraction** (like Test Groups in Graphics Tests). eg: `Specialist_ExampleGroup_ExampleTest`. This will be used when grouping tests in the manu.
* Notice the relative lack of a scene composition.
* In a specialist test one object should contain the script `Test_Controller.cs`.  On this script you can specify which cameras in the scene to be able to view and generate a button array for the UI when viewing that test.
* You can create buttons to execute custom code on different scripts using the `Custom` button type and Custom input fields.

---
#### Running Tests
All usage of the Graphics Test Framework start by opening `MainMenu.unity`
From there you can run all test types

---
##### Graphics Tests
* If Graphics Tests were found the `Manual > Graphics Tests` and `Automation > Graphics Tests` menu paths should be enabled. If they are not check your scenes against the information in *Creating new tests*.

###### Manual Graphics Tests
* Viewing graphics tests manually allows you to specify specific test groups and tests from the menu and jump directly to them. You can also view test results (including reference images, comparison images and comparison values) in the UI.
* Comparison values are a percentage of the difference of all pixels on screen between the latest result and loaded comparison.
* If no reference data could be found you must create new reference data and will not be able to view comparisons.
* From the menu at the bottom of the screen you can also view live results from the test camera as well as performance metrics
* **Reference and result data cannot be saved/uploaded from a manual run.** This must be done from an automation run.
* Use arrow keys (desktop) or swipe left/right (mobile) to view different tests.
* At any point you can view the results of the test run from the bottom menu.

###### Automation Graphics Tests
* Running graphics tests from the automation menu allows you to select which test sets to run (but doesnt allow further abstraction) and gives other options for the test run.
* **To save reference or result data you must have every test selected and** `Run From All Categories` **checked**
* Once the test run is completed you will be taken straight to the results screen
* From there (if the above criteria is met) you can save results data
* If no reference data was found you can only create a new "Baseline" file to compare against on future runs
* Created files are saved to `Application.persistentDataPath` under a folder structure separating by suite, platform and graphics API

---
##### Specialist Tests
* Specialist tests are viewed through the same UI as graphics tests when run manually
* Tabs are generated by the cameras supplied to `Test_Controller.cs`
* Buttons are generated from the array supplied to `Test_Controller.cs`
