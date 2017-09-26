# Password Strength Checker Android
Check password strength (Weak, Medium, Strong, Very Strong). Setting optional requirements by required length, with at least 1 special character, numbers and letters in uppercase or lowercase.

![giphy](https://user-images.githubusercontent.com/13048080/30842440-e8e7a56c-a2b4-11e7-87aa-3dddcd69b544.gif)

## How to use
Step 1. Add the JitPack repository to your build file. Add it in your root build.gradle at the end of repositories:

```gradle
allprojects {
  repositories {
    ...
    maven { url "https://jitpack.io" }
  }
}
```
Step 2. Add the dependency

```gradle
dependencies {
    compile 'com.github.yesterselga:password-strength-checker-android:v1.0'
}
```

## Usage

in your MainActivity.java

```java
import com.ybs.passwordstrengthchecker.PwdStrength.PasswordStrength;

public class MainActivity extends AppCompatActivity implements TextWatcher {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText password = (EditText)findViewById(R.id.login_password);
        password.addTextChangedListener(this);

    }

    @Override
    public void afterTextChanged(Editable s) {
    }
    @Override
    public void beforeTextChanged(
            CharSequence s, int start, int count, int after) {
    }

    @Override
    public void onTextChanged(CharSequence s, int start, int before, int count) {
        updatePasswordStrengthView(s.toString());
    }

    private void updatePasswordStrengthView(String password) {

        ProgressBar progressBar = (ProgressBar) findViewById(R.id.progressBar);
        TextView strengthView = (TextView) findViewById(R.id.password_strength);
        if (TextView.VISIBLE != strengthView.getVisibility())
            return;

        if (password.isEmpty()) {
            strengthView.setText("");
            progressBar.setProgress(0);
            return;
        }

        PasswordStrength str = PasswordStrength.calculateStrength(password);
        strengthView.setText(str.getText(this));
        strengthView.setTextColor(str.getColor());

        progressBar.getProgressDrawable().setColorFilter(str.getColor(), android.graphics.PorterDuff.Mode.SRC_IN);
        if (str.getText(this).equals("Weak")) {
            progressBar.setProgress(25);
        } else if (str.getText(this).equals("Medium")) {
            progressBar.setProgress(50);
        } else if (str.getText(this).equals("Strong")) {
            progressBar.setProgress(75);
        } else {
            progressBar.setProgress(100);
        }
    }
}
```
in your activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:padding="20sp">
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Password Meter"
        android:gravity="center_horizontal"
        android:paddingTop="25sp"
        android:paddingBottom="20sp"
        android:textSize="20sp" />
    <EditText
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:freezesText="true"
        android:hint="Enter Password"
        android:clickable="true"
        android:singleLine="true"
        android:inputType="textPassword"
        android:id="@+id/login_password"
        android:padding="10dp"
        android:background="@null"/>
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <ProgressBar
            android:id="@+id/progressBar"
            style="?android:attr/progressBarStyleHorizontal"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:indeterminate="false"
            android:maxHeight="20dip"
            android:minHeight="20dip"
            android:layout_weight="1"
            android:progress="0"/>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:gravity="center_horizontal"
            android:text="Weak"
            android:id="@+id/password_strength" />
    </LinearLayout>
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/login_instructions"
        android:text="Your password must be at least 8 characters. With at least 1 number and 1 special character" />
</LinearLayout>
```
in your values/strings.xml

```xml
<resources>
    <string name="password_strength_weak">Weak</string>
    <string name="password_strength_medium">Medium</string>
    <string name="password_strength_strong">Strong</string>
    <string name="password_strength_very_strong">Very Strong</string>
</resources>
```

Done ;)
