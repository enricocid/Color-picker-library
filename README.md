# Color picker library

# DEPRECATION NOTICE: This project is no longer mantained and has been replaced by ColorBox: https://github.com/enricocid/ColorBox-library

### Features

- Live color preview;
- Change color using A(alpha) R(red) G(green) B(blue) seekbars;
- Change colors by editing HEX|RGB's A (alpha) R (red) G (green) B (blue) edit texts;
- Built-in material palette;

[ ![cpl](https://api.bintray.com/packages/enricod/Enrico/Color-picker-library/images/download.svg) ](https://bintray.com/enricod/Enrico/Color-picker-library/_latestVersion)
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-Color%20picker%20library-orange.svg?style=flat)](https://android-arsenal.com/details/1/4949)

![ScreenShot](https://github.com/enricocid/Color-picker-library/blob/master/art/screens2.png)
------

# Sample Project

You can download the latest sample APK from this repo here: https://github.com/enricocid/Color-picker-library/blob/master/app/app-release.apk

The source code is also available: https://github.com/enricocid/Color-picker-library/tree/master/app

#F-Droid
 
<a href="https://f-droid.org/repository/browse/?fdid=com.enrico.sample">
  <img alt="Get it on F-Droid"       src="https://raw.githubusercontent.com/enricocid/Storage-USB/master/art/fdroid.png" />
</a>




# Gradle Dependency

### Repository

The Gradle dependency is available via [jCenter](https://bintray.com/enricod/Enrico/Color-picker-library/view).
jCenter is the default Maven repository used by Android Studio.

The minimum API level supported by this library is API 16 (Jelly Bean).

### Download

Add the following dependency to your projects build.gradle:

```gradle
dependencies {
    // ... other dependencies here
    compile 'com.github.enricocid:cpl:1.0.3'
}
```

# Usage

###To display a color picker DialogFragment:

``` java
colorDialog.showColorPicker(AppCompatActivity activity, int tag);
```

**tag:** it's an integer tag assigned to the dialog, for example 1.

If You wanna use multiple dialog in Your activity You can use progressive numbers (1,2,3 ...).

###The Activity showing the color picker dialog must implement ColorSelectedListener:

``` java
public class YourActivity extends AppCompatActivity implements colorDialog.ColorSelectedListener {

    // ...

    @Override

    public void onColorSelection(DialogFragment dialogFragment, int color) {

        // Do Your shiz on color selection
        // ...

        // Set the picker's dialog color
        colorDialog.setPickerColor(YourActivity.this, int tag, int color);
    }
}
```

###If You have multiple dialogs you can take advantage of tags. For example, if we have created four dialogs (1,2,3,4):

``` java
public class YourActivity extends AppCompatActivity implements colorDialog.ColorSelectedListener {

    // ...

    @Override

    public void onColorSelection(DialogFragment dialogFragment, int color) {

        int tag;

        // get tag number from fragment
        tag = Integer.valueOf(dialogFragment.getTag());

        switch (tag) {

        case 1:

            // do your shiz with selected color from dialog 1
            // ...

            //Set the picker dialog's color
            colorDialog.setPickerColor(YourActivity.this, 1, color);

            break;

        case 2:

            // do your shiz with selected color from dialog 2
            // ...

            //Set the picker dialog's color
            colorDialog.setPickerColor(YourActivity.this, 2, color);

            break;

        case 3:

            // do your shiz with selected color from dialog 3
            // ...

            //Set the picker dialog's color
            colorDialog.setPickerColor(YourActivity.this, 3, color);

            break;

        case 4:

            // do your shiz with selected color from dialog 4
            // ...

            //Set the picker dialog's color
            colorDialog.setPickerColor(YourActivity.this, 4, color);

            break;
        }
    }
}
```

# Usage instructions for Preferences

If you're developing for Android 3.0 (API level 11) and higher, you should use a PreferenceFragment to display your list of Preference objects as recommended by Google (https://developer.android.com/guide/topics/ui/settings.html#Fragment)

###To display a color picker DialogFragment on Preference click:

``` java
public static class YourPreferenceFragment extends PreferenceFragment {
    
    //retrieve AppCompatActivity
    final AppCompatActivity appCompatActivity = (AppCompatActivity) getActivity();   
    
    //your preference
    Preference yourPreference;

    // ...

    @Override

    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);

        //get yourPreference
        yourPreference = findPreference("yourPreferenceKey");

        //on click show colorDialog
        yourPreference.setOnPreferenceClickListener(new Preference.OnPreferenceClickListener() {

            @Override

            public boolean onPreferenceClick(Preference preference) {

                colorDialog.showColorPicker(appCompatActivity, int tag);

                return false;
            }
        });
    }
}
```

###PreferenceActivity must implement colorSelectedListener:

```java
public class YourPreferenceActivity extends AppCompatActivity implements colorDialog.ColorSelectedListener {

    // ...

    @Override

    public void onColorSelection(DialogFragment dialogFragment, int color) {

        // Do Your shiz on color selection
        // ...

        //Set the picker's dialog color
        colorDialog.setPickerColor(YourPreferenceActivity.this, int tag, color);
    }
}
```

###To retrieve the dialog color on app's resume by tag number(on onCreate or onResume)You can use 

```java
colorDialog.getPickerColor(AppCompatActivity activity, int tag);
```

```java
public static class YourPreferenceFragment extends PreferenceFragment {

    //your preference
    static Preference yourPreference;

    // ...

    @Override

    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);

        //retrieve the color
        int color = colorDialog.getPickerColor(getActivity(), int tag);
    }
}
```

###(Optional) Set a custom preference
  
![ScreenShot](https://github.com/enricocid/Color-picker-library/blob/master/art/pref2.png)

We are going to access the preference in a static method. In Your Preference fragment let's define the preference:

```java
public static class YourPreferenceFragment extends PreferenceFragment {

    //your preference
    static Preference yourPreference;

    // ...

    @Override

    public void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);

        //Get yourPreference
        yourPreference = findPreference("yourPreferenceKey");
    }
}
```

###onColorSelection method we are going to set custom Preference:

```java
public class PreferenceActivity extends AppCompatActivity implements colorDialog.ColorSelectedListener {

    import static com.yourPackage.YourPreferenceActivity.YourPreferenceFragment.yourPreference;

    // ...

    @Override

    public void onColorSelection(DialogFragment dialogFragment, int color) {
    
        // ...

        //Set the picker dialog's color
        colorDialog.setPickerColor(YourPreferenceActivity.this, int tag, color);

        //set custom preference summary
        colorDialog.setColorPreferenceSummary(yourPreference, color, YourPreferenceActivity.this, getResources());
    }
}
```

###You can use switch case statement if You have multiple dialogs in Your preferences (as explained above):

```java
switch (tag) {

case 1:

    //Set the picker dialog's color
    colorDialog.setPickerColor(YourPreferenceActivity.this, 1, color);

    //set preference style
    colorDialog.setColorPreferenceSummary(yourPreference, color, YourPreferenceActivity.this, getResources());

    break;

case 2:

    // ...
    break;

case 3:

    // ...
    break;

case ...
    // ...
    break;
    
case ...   
}
```

###To mantain the circle and summary colors on app's resume You can use: 

```java
colorDialog.getPickerColor(AppCompatActivity activity, int tag);
```

In YourPreferenceFragment:

```java
public static class YourPreferenceFragment extends PreferenceFragment {

    // ...
    
    //color picker preferences
    static Preference yourPreference;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        //get charge color preference
        yourPreference = findPreference("yourPreferenceKey");

        //get preferences colors
        int color = colorDialog.getPickerColor(getActivity(), int tag);

        //set preferences colors
        colorDialog.setColorPreferenceSummary(yourPreference, color, getActivity(), getResources());
    }
}
```

#Utilities

Color picker library contains two color utilities that You can use in Your app.

###shiftColor(int color, float fraction)

Returns shifted down color by the selected fraction

Example usage for color #FF673AB7:

```java
colorDialog.shiftColor(color, 0.9f);
```

![ScreenShot](https://github.com/enricocid/Color-picker-library/blob/master/art/shift.png)

###getComplementaryColor(int colorToInvert)

Returns inverted color

Example usage for color #FF673AB7:

```java
colorDialog.getComplementaryColor(color)
```

![ScreenShot](https://github.com/enricocid/Color-picker-library/blob/master/art/compl.png)


