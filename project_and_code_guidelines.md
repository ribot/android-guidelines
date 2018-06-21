# 1. Project guidelines

## 1.1 Project structure

New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure). The [ribot Boilerplate](https://github.com/ribot/android-boilerplate) project is a good reference to start from.

## 1.2 File naming

### 1.2.1 Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 1.2.2 Resources files

Resources file names are written in __lowercase_underscore__.

#### 1.2.2.1 Drawable files

Naming conventions for drawables:


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

Naming conventions for selector states:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 Layout files

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| View / ViewGroup | `SliderView`           | `view_slider.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

A slightly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should start with `item_`.

Other case is when layout is intended to be inflated as View or ViewGroup custom implementation layout. In this case `view_` prefix should be used.

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the prefix `partial_`.

#### 1.2.2.3 Menu files

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practice is to not include the word `menu` as part of the name because these files are already located in the `menu` directory.

#### 1.2.2.4 Values files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 2 Code guidelines

## 2.1 Java language rules

### 2.1.1 Don't ignore exceptions

You must never do the following:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_While you may think that your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions like above creates mines in your code for someone else to trip over some day. You must handle every Exception in your code in some principled way. The specific handling varies depending on the case._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See alternatives [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).

### 2.1.2 Don't catch generic exception

You should not do this:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```

See the reason why and some alternatives [here](https://source.android.com/source/code-style.html#dont-catch-generic-exception).
If you still decide to do this, write a comment why this is a good idea.

### 2.1.3 Don't use finalizers

_We don't use finalizers. Read more: [Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers)_

### 2.1.4 Fully qualify imports

This is bad: `import foo.*;`

This is good: `import foo.Bar;`

See more info [here](https://source.android.com/source/code-style.html#fully-qualify-imports).

### 2.1.5 Avoid Serialization and Deserialization

_Serialization and deserialization of objects is a CPU-intensive procedure and is likely to slow down your application. While performance loss won't be visible on small objects beware of serialization and deserialization of large lists of objects.
Preferable approach is to use [Parcelable](https://developer.android.com/reference/android/os/Parcelable.html)._

### 2.1.6 Avoid Synchronized

Synchronization has hight performance cost in java and improper synchronization can also cause a deadlock.
So use synchronization if you are truly dealing with multithreaded processes and doing so follow the guidelines.
You can read more detailed guidelines here: [Java Language Best Practices](http://docs.oracle.com/cd/A97688_16/generic.903/bp/java.htm#1006832)

#### 2.1.6.1 Synchronize Critical Sections Only

If only certain operations in the method must be synchronized, use a synchronized block with a lock instead of synchronizing the entire method. For example:
```
private final Object lock = new Object();
    ...
    private void doSomething()
    {
     // perform tasks that do not require synchronicity
        ...
        synchronized (lock)
            {
            ...
            }
        ...
    }
```

#### 2.1.6.2 Know Which Java Objects Already Have Synchronization Built-in
Some Java objects (such as Hashtable, Vector, and StringBuffer) already have synchronization built into many of their APIs. They may not require additional synchronization.

#### 2.1.6.3 Do Not Under-Synchronize
Some Java variables and operations are not atomic. If these variables or operations can be used by multiple threads, you must use synchronization to prevent data corruption. For example: (i) Java types long and double are comprised of eight bytes; any access to these fields must be synchronized. (ii) Operations such as ++ and -- must be synchronized because they represent a read and a write, not an atomic operation.

## 2.2 Java style rules

### 2.2.1 Fields definition and naming

Fields should be defined at the __top of the file__ and they should follow the naming rules listed below.

* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.
* Other fields start with a lower case letter.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    private static MyClass myClass;
    public int publicField;
    int packagePrivateField;
    private int privateField;
    protected int protectedField;
}
```

### 2.2.2 Treat acronyms as words

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.3 Use spaces for indentation

Use __4 space__ indents for blocks:

```java
if (x == 1) {
    x++;
}
```

Use __8 space__ indents for line wraps:

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.4 Use standard brace style

Braces go on the same line as the code before them.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

Braces around the statements are required unless the condition and the body fit on one line.

If the condition and the body fit on one line and that line is shorter than the max line length, then braces are not required, e.g.

```java
if (condition) body();
```

This is __bad__:

```java
if (condition)
    body();  // bad!
```

### 2.2.5 Annotations

#### 2.2.5.1 Annotations practices

According to the Android code style guide, the standard practices for some of the predefined annotations in Java are:

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the `@inheritDoc` Javadoc tag, and derive from a class (not an interface), you must also annotate that the method `@Override`s the parent class's method.

* `@SuppressWarnings`: The `@SuppressWarnings` annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the `@SuppressWarnings` annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotation guidelines can be found [here](http://source.android.com/source/code-style.html#use-standard-java-annotations).

#### 2.2.5.2 Annotations style

__Classes, Methods and Constructors__

When annotations are applied to a class, method, or constructor, they are listed after the documentation block and should appear as __one annotation per line__ .

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__Fields__

Annotations applying to fields should be listed __on the same line__, unless the line reaches the maximum line length.

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.6 Limit variable scope

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.7 Logging guidelines

VERBOSE and DEBUG logs __must__ be disabled on release builds. It is also recommended to disable INFORMATION, WARNING and ERROR logs but you may want to keep them enabled if you think they may be useful to identify issues on release builds. If you decide to leave them enabled, you have to make sure that they are not leaking private information such as email addresses, user ids, etc.

### 2.2.8 Class member ordering

There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Inner interfaces
2. Constants
3. Static fields
4. Static methods
5. Fields
6. Constructors
7. Overridden methods and callbacks
8. Public methods
9. Private methods
10. Inner classes

Example:
```java
public class MainFragment extends Fragment {

    public interface Callback {
        void onPhotoSent();
    }

    public static final String TAG = "MainFragment";
    private static final int DEFAULT_TIMEOUT = 20;

    public static String publicStaticString;
    private static String privateStaticString;

    public static Fragment newInstance() {
        return new MainFragment();
    }

    private String title;
    private TextView textView;

    public void setTitle(String title) {
        this.title = title;
    }

    @Override
    public void onCreate() {
        ...
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {
        ...
    }

}
```

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.9 Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.10 String constants, naming, and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARG_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |
| Activity result extras | `RESULT_`           |

Note that the arguments of a Fragment - `Fragment.getArguments()` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them.
Also use `RESULT_` prefix for Activity result intents.

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARG_USER_ID = "ARG_USER_ID";

static final String EXTRA_SURNAME = "EXTRA_SURNAME";
static final String RESULT_SURNAME = "RESULT_SURNAME";

// Intent action keys use full package name as value
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.11 Arguments in Fragments and Activities

When data is passed into an `Activity `or `Fragment` via an `Intent` or a `Bundle`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expects arguments, it should provide a `public static` method that facilitates the creation of the relevant `Intent` or `Fragment`.

In the case of __Activities__ the method is usually called `getStartIntent()`:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

For retrieving extras use getter with 'Extra' prefix:

```java
private User getExtraUser() {
    return getIntent().getParcelableExtra(EXTRA_USER);
}
```

For __Fragments__ it is named `newInstance()` and handles the creation of the Fragment with the right arguments:

```java
public static UserFragment newInstance(User user) {
    UserFragment fragment = new UserFragment;
    Bundle args = new Bundle();
    args.putParcelable(ARG_USER, user);
    fragment.setArguments(args)
    return fragment;
}
```

__Note__: If we provide the methods described above, the keys for extras and arguments should be `private` because there is not need for them to be exposed outside the class.

For retrieving fragment arguments use getter method with 'Arg' prefix:

```java
private User getArgUser() {
    // No need for getArguments() == null check as long as
    // there is no newInstance method with zero args.
    return getArguments().getParcelable(ARG_USER);
}
```


### 2.2.12 Android View subclass naming

It is very common to instantiate views in Fragment's `onCreateView()` method and declare them as class variables. But the naming of these variable could be tricky in some cases.

For example:
```java

    // BAD EXAMPLE!
    // The view nameText and a String can be confused with each other.
    private String name;
    private TextView nameText;

```
To avoid this confusion and to group different views by their type a simple rule should be applied: View variable name should be prefixed with its type shortening. For example:
```java
    // tv stands for TextView
    private TextView tvName, tvEmail;
    // iv stands for ImageView
    private ImageView ivLogo;
    // b stands for Button
    private Button bSubmit;
    // w stands for Wrapper
    private LinearLayout wUserData
```

### 2.2.13 Android View declaration

Views should be declared in single line declaration.

__Bad:__
```java
    private TextView tvName;
    private TextView tvEmail;
```
__Good:__
```java
    private TextView tvName, tvEmail;
```


### 2.2.14 Line length limit

Code lines should not exceed __100 characters__. If the line is longer than this limit there are usually two options to reduce its length:

* Extract a local variable or method (preferable).
* Apply line-wrapping to divide a single line into multiple ones.

There are two __exceptions__ where it is possible to have lines longer than 100:

* Lines that are not possible to split, e.g. long URLs in comments.
* `package` and `import` statements.

#### 2.2.14.1 Line-wrapping strategies

There isn't an exact formula that explains how to line-wrap and quite often different solutions are valid. However there are a few rules that can be applied to common cases.

__Break at operators__

When the line is broken at an operator, the break comes __before__ the operator. For example:

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
```

__Assignment Operator Exception__

An exception to the `break at operators` rule is the assignment operator `=`, where the line break should happen __after__ the operator.

```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__Method chain case__

When multiple methods are chained in the same line - for example when using Builders - every call to a method should go in its own line, breaking the line before the `.`

```java
Picasso.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
Picasso.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__Long parameters case__

When a method has many parameters or its parameters are very long, we should break the line after `(`, before `)`, and after every comma `,`.

```java
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(
	context,
	"http://ribot.co.uk/images/sexyjoe.jpg",
	mImageViewProfilePicture,
	clickListener,
	"Title of the picture"
);
```

### 2.2.15 RxJava chains styling

Rx chains of operators require line-wrapping. Every operator must go in a new line and the line should be broken before the `.`

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mRetrofitService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```

## 2.3 XML style rules

### 2.3.1 Use self closing tags

When an XML element doesn't have any contents, you __must__ use self closing tags.

This is good:

```xml
<TextView
    android:id="@+id/tv_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/tv_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 Resources naming

Resource IDs and names are written in __lowercase_underscore__.

#### 2.3.2.1 ID naming

IDs should be prefixed with a shortening of element type in lowercase underscore. For example:


| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`         | `tv_`             |
| `ImageView`        | `iv_`             |
| `Button`           | `b_`              |
| `ViewGroup`	     | `w_`

Image view example:

```xml
<ImageView
    android:id="@+id/iv_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Use `w_` for wrappers, a.k.a. ViewGroups. For example:

```xml
<LinearLayout
    android:id="@+id/w_user_data"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu items id should be prefixed with `menu_`. For example:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Styles and Themes

Style names are written in __UpperCamelCase__ and should be prefixed with project name or project name shortening. Also style names should be divided by `.` into logical pieces, for example:
```xml
<style name="MPay.Theme.Light"/>
<style name="MPay.Text.Header1"/>
<style name="MPay.Button.Flat"/>
```

#### 2.3.2.3 Colors

Color names are written in __lowercase_underscore__. Color code should be __UPPERCASE__, for example:
```xml
<color name="grey_light">#F0F0F0</color>
```

Use prefixes to designate color groups for text `text_`, toolbar `toolbar_`, status bar `status_`, tabs `tab_`. Use buttons prefix `btn_` for clickable items and selectors. Before adding new color, check if it doesn't already exist in this color group. Do NOT use colors from a different color group.

For colors with a specified alpha value, append opacity percentage as a two digit number at the end of a color name. E.g.
```xml
<color name="btn_red_67">#AAFD3C30</color>.
```

Determine percentage from hex value [here](http://online.sfsu.edu/chrism/hexval.html).

### 2.3.3 Attributes ordering

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

### 2.3.4 Use design time layout attributes

These are attributes which are used when the layout is rendered in the tool, but have no impact on the runtime.
Use these attributes to display sample data when editing the layout. This makes layout previews much more informative.

To achieve this use __tools__ namespace:

```xml
//Make views visible even if they're not visible initially.
<LinearLayout
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:visibility="gone"
        tools:visibility="visible">

        //Set sample text if non english string resource is used.
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/login_user_name"
            tools:text="Username"/>

        //Set sample text if text field is empty initially.
        <EditText
            android:id="@+id/et_username"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            tools:text="John Dao"/>

    </LinearLayout>
```

### 2.3.5 Use of dimension resources

All components align to an 8dp square baseline grid. Iconography in toolbars align to a 4dp square baseline grid. Dimensions that can be reused must be stored in dimension resources, to keep projects visual design language consistent.

#### 2.3.5.1 Use common dimension resources

Use common dimension resources for view paddings and margins to make layouts consistent and better adapt to different screen configurations.

| Margins | Paddings |
| --- | --- |
| `margin_xs` | `padding_xs` |
| `margin_s` | `padding_s` |
| `margin_m` | `padding_m` |
| `margin_l` | `padding_l` |
| `margin_xl` | `padding_xl` |

#### 2.3.5.2 Use screen margin dimension resources

Use screen margin dimensions for content container that matches screen size. This will provide more appropriate and distinct layout for large screen configurations with minimal effort when separate layout file is not required.

| Dimension | Phone | 7" | 10" |
| --- | --- | --- | --- |
| `screen_margin_horizontal` | `0dp` | `64dp` | `80dp` |
| `screen_margin_vertical` | `0dp` | `40dp` | `64dp`|

If container view is __scrollable__, `screen_margin_vertical` dimension value should be used as vertical padding instead of margin in conjunction with `clipToPadding` attribute value set to __false__, for example:

```xml
...
<RecyclerView
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_marginLeft="@dimen/screen_margin_horizontal"
    android:layout_marginRight="@dimen/screen_margin_horizontal"
    android:clipToPadding="false"
    android:paddingTop="@dimen/screen_margin_vertical"
    android:paddingBottom="@dimen/screen_margin_vertical" />
...
```

Screen margin dimensions can also be __used as padding__ to achieve same visual result when content container is top level container, for example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:tools="http://schemas.android.com/tools"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:paddingLeft="@dimen/screen_margin_horizontal"
             android:paddingRight="@dimen/screen_margin_horizontal"
             android:paddingTop="@dimen/screen_margin_vertical"
             android:paddingBottom="@dimen/screen_margin_vertical">
             ...
```

## 2.4 Tests style rules

### 2.4.1 Unit tests

Test classes should match the name of the class the tests are targeting, followed by `Test`. For example, if we create a test class that contains tests for the `DatabaseHelper`, we should name it `DatabaseHelperTest`.

Test methods are annotated with `@Test` and should generally start with the name of the method that is being tested, followed by a precondition and/or expected behaviour.

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

Precondition and/or expected behaviour may not always be required if the test is clear enough without them.

Sometimes a class may contain a large amount of methods that at the same time require several tests for each method. In this case, it's recommended to split up the test class into multiple ones. For example, if the `DataManager` contains a lot of methods we may want to divide it into `DataManagerSignInTest`, `DataManagerLoadUsersTest`, etc. Generally you will be able to see which tests belong together because they have common [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).

### 2.4.2 Espresso tests

Every Espresso test class usually targets an Activity, therefore the name should match the name of the targeted Activity followed by `Test`, e.g. `SignInActivityTest`.

When using the Espresso API it is a common practice to place chained methods in new lines.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```
