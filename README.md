Вот подробная инструкция по созданию Android-приложения, соответствующего вашему запросу:

### 1. Создание проекта
1. Откройте **Android Studio**.
2. Выберите **"New Project"**, затем **"Empty Activity"**, и задайте имя проекта, например `SecretaryApp`.
3. Убедитесь, что выбран язык **Java** или **Kotlin** (для примера используем Kotlin).

### 2. Макет приложения
Создайте два макета: один для портретной ориентации (`res/layout/activity_main.xml`) и второй для горизонтальной (`res/layout-land/activity_main.xml`).

**Пример для портретной ориентации (`activity_main.xml`):**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/et_name"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Name"
        android:inputType="textPersonName" />

    <EditText
        android:id="@+id/et_phone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Phone Number"
        android:inputType="phone" />

    <EditText
        android:id="@+id/et_email"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Email"
        android:inputType="textEmailAddress" />

    <EditText
        android:id="@+id/et_salary"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Salary"
        android:inputType="numberDecimal" />

    <EditText
        android:id="@+id/et_department"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Department"
        android:inputType="text" />

    <Button
        android:id="@+id/btn_save"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Save" />
</LinearLayout>
```

**Пример для горизонтальной ориентации (`activity_main.xml` в `res/layout-land`):**
```xml
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    android:padding="16dp">

    <ScrollView
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">

            <!-- Поля остаются такими же -->
            <EditText
                android:id="@+id/et_name"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:hint="Name"
                android:inputType="textPersonName" />
            
            <!-- Остальные поля -->

        </LinearLayout>
    </ScrollView>
</LinearLayout>
```

### 3. Сохранение данных при смене ориентации
В `MainActivity.kt`:
```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var etName: EditText
    private lateinit var etPhone: EditText
    private lateinit var etEmail: EditText
    private lateinit var etSalary: EditText
    private lateinit var etDepartment: EditText

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        etName = findViewById(R.id.et_name)
        etPhone = findViewById(R.id.et_phone)
        etEmail = findViewById(R.id.et_email)
        etSalary = findViewById(R.id.et_salary)
        etDepartment = findViewById(R.id.et_department)

        if (savedInstanceState != null) {
            etDepartment.setText(savedInstanceState.getString("DEPARTMENT"))
        }
    }

    override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        outState.putString("DEPARTMENT", etDepartment.text.toString())
    }

    override fun onRestoreInstanceState(savedInstanceState: Bundle) {
        super.onRestoreInstanceState(savedInstanceState)
        etDepartment.setText(savedInstanceState.getString("DEPARTMENT"))
    }
}
```

### 4. Сохранение и восстановление данных с использованием `SharedPreferences`
```kotlin
override fun onPause() {
    super.onPause()
    val sharedPreferences = getSharedPreferences("SECRETARY_PREFS", MODE_PRIVATE)
    val editor = sharedPreferences.edit()

    editor.putString("NAME", etName.text.toString())
    editor.putString("PHONE", etPhone.text.toString())
    editor.putString("EMAIL", etEmail.text.toString())
    editor.putFloat("SALARY", etSalary.text.toString().toFloatOrNull() ?: 0.0f)
    editor.putString("DEPARTMENT", etDepartment.text.toString())

    editor.apply()
}

override fun onResume() {
    super.onResume()
    val sharedPreferences = getSharedPreferences("SECRETARY_PREFS", MODE_PRIVATE)

    etName.setText(sharedPreferences.getString("NAME", ""))
    etPhone.setText(sharedPreferences.getString("PHONE", ""))
    etEmail.setText(sharedPreferences.getString("EMAIL", ""))
    etSalary.setText(sharedPreferences.getFloat("SALARY", 0.0f).toString())
    etDepartment.setText(sharedPreferences.getString("DEPARTMENT", ""))
}
```

### Итог
Этот код:
1. Включает ввод данных через 5 полей разных типов.
2. Сохраняет данные полей при смене ориентации с помощью `Bundle`.
3. Сохраняет данные в `SharedPreferences` при закрытии приложения и восстанавливает их при запуске.
