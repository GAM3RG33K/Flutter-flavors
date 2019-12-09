# Flutter-flavors
This guide shows how to add new app flavors in existing flutter apps.

### Note:
* replace <flavor> with new flavor name without '<' and '>'
* \* - required to be done only once. if already done skip the step.
* Make sure that the flavor name is in all lower case, so that it does not cause any confusion/issue in any platforms

## Add flavors in flutter dart (On Windows/Linux/mac):
1. Open flutter project in file explorer
2. Go to `lib` folder 
3. create a copy of `main.dart` file and rename it to `main_<flavor>.dart`
4. *(Optional)* add flavor related configurations to your main Widget in this file, if any.

## Add flavors in flutter Android app (On Android studio):
1. Open flutter/andorid project in android studio
2. open `build.gradle`(app) file 
3. find `buildTypes`
    * add following line below `buildTypes`:
        ```groovy
          flavorDimensions "app"
        ```
4. add flavor type below the new line : 
    ```groovy 
    productFlavors {
      // ... other flavors 
      <flavor> {
        dimension "app"
        // add custom android configurations here, like 
        applicationIdSuffix ".<flavor>"

      }
    }
    ```
5. Open `AndroidManifest.xml` (android/app/src/main/AndroidManifest.xml)\*
    * change `android:label` to "@string/app_name" (including quotes)
6. Select `src` folder in project navigation  (app/src)
	* Right click -> New -> directory -> name = `<flavor>`
	* create a new directory `res` in `<flavor>`
	* create a new directory `values` in res
    * create a new directory `drawable` in res ->  add require images/icons for `<flavor>`
	* create a `strings.xml` file in values directory with following content:
      ```
        <?xml version="1.0" encoding="utf-8"?>
        <resources>
          <string name="app_name">Flutter App <flavor></string>
        </resources>
      ```

## Add flavors in flutter iOS app (On Mac only):
1. Open XCode project with flutter app's runner file(ios/Runner.xcworkspace file)
2. create a file `<flavor>.xcconfig`
	* the added file should contain following: 
	* `bundle_name=<flavored_bundle_name>`
3. Select "Runner" in project Navigator (on left drawer menu)\*
	 * Select Info -> find "Bundle Name" -> replace value with "$(bundle_name)"
		
4. Go to Product -> Scheme -> "New scheme" from Menu on top of XCode
	-> Select Target = Runner -> Name = `<flavor>` -> OK
	
5. Copy Debug.xcconfig file and rename it to `Debug-<flavor>.xcconfig` (ios/Flutter/)
	* replace the content with following:
		```
            #include "Debug.xcconfig"
            #include "<flavor>.xcconfig"
		```
6. Copy Release.xcconfig file and rename it to `Release-<flavor>.xcconfig` (ios/Flutter/)
	*  replace the content with following:
        ```
    		#include "Release.xcconfig"
    		#include "<flavor>.xcconfig"
		```
		
7. Add these files to XCode project (ios/*.xcworkspace file)
	* select Flutter Directory below the runner target (on left drawer menu)
	* right-click => select "Add files to runner"
	* select only newly created files (ios/Flutter/) -> Add
	
8. Select "Runner" in project Navigator (on left drawer menu)
	* Click on "Runner" before "General" Menu tab in new window
	* Select "Runner" in PROJECT, NOT the one in TARGET
	
9. Add new configuration for `<flavor>`:
	* press "+" -> Duplicate "Debug" 
	* rename new configuration to `Debug-<flavor>`
	* repeat above steps for "Release" config as well
	
10. Open Assets.xcassets folder in finder (ios/Runner/Assets.xcassets)
	* create a copy of "AppIcon.appiconset" folder and rename the copy to "AppIcon-<flavor>.appiconset"
	
11. Go to Project Navigator -> click on Runner(Target) -> Search "APPICON"
	* select `Debug-<flavor>` -> change value `AppIcon` to `AppIcon-<flavor>`
	* do the same for `Release-<flavor>`
	
12. Go to Build Settings in Runner(Target)
	* Search "bundle identifier"
	* select `Debug-<flavor>` -> add `.<flavor>` to existing identifier value
	* do the same for `Release-<flavor>`

13. Click "+" near the search bar\*
	* select "Add User defined Setting"
	* Rename new Settings from "NEW_SETTING" to "APP_DISPLAY_NAME"
	
14. Expand the "APP_DISPLAY_NAME" setting
	* select `Debug-<flavor>` -> add ` <flavor>` to existing name value
	*  do the same for `Release-<flavor>`

15. Select "Runner" in project Navigator (on left drawer menu)\*
	* Select Info -> find "Bundle Display Name" -> replace value with "$(APP_DISPLAY_NAME)"

16. Select <flavor> scheme near the 'Run App' button
	* click 'Edit scheme'
	* Open each category (except 'build') and set the config to applicable flavor configuration, for example *`<flavor>`*:
    	* Run -> Debug-<flavor>
    	* Test -> Debug-<flavor>
    	* Profile -> Debug-<flavor>
    	* Analyze -> Debug-<flavor>
    	* Archive -> Release-<flavor>
