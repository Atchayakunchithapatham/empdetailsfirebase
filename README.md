
# Ex.No:1 To create a employee details fields and to display the employee details using Firebase Database in Android Studio.


## AIM:

To create and display the employee details using Firebase Database in Android Studio.

## EQUIPMENTS REQUIRED:

Android Studio(Min.required Artic Fox)

## ALGORITHM:

Step 1: Open Android Stdio and then click on File -> New -> New project.

Step 2: Then type the Application name as HelloWorld and click Next. 

Step 3: Then select the Minimum SDK as shown below and click Next.

Step 4: Then select the Empty Activity and click Next. Finally click Finish.

Step 5: Design layout in activity_main.xml.

Step 6: Display the employee details in MainActivity file.

Step 7: Save and run the application.

## PROGRAM:
```
/*
Program to print the DatabaseTable using the firebasedatabase”.
Developed by: Atchaya.k
Registration Number : 212223220011
*/
```

### EMP.JAVA:
```
package com.example.basicapplication;

public class Emp {
    public String id;
    public String name;
    public String age;
    public String salary;

    // Required for Firebase to read data
    public Emp() {
    }

    public Emp(String id, String name, String age, String salary) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
}

```

### MAINACTIVITY.JAVA:

```
package com.example.basicapplication;

import android.os.Bundle;
import android.widget.*;
import androidx.appcompat.app.AppCompatActivity;
import com.google.firebase.database.*;
import java.util.*;

public class MainActivity extends AppCompatActivity {
    // Variables must be declared here at the class level so clearFields() can see them
    EditText etName, etAge, etSalary;
    Button btnAdd, btnUpdate, btnDelete;
    ListView listView;

    DatabaseReference dbRef;
    ArrayList<String> items = new ArrayList<>();
    ArrayList<String> keys = new ArrayList<>();
    ArrayAdapter<String> adapter;
    String selectedKey = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        etName = findViewById(R.id.etName);
        etAge = findViewById(R.id.etAge);
        etSalary = findViewById(R.id.etSalary);
        btnAdd = findViewById(R.id.btnAdd);
        btnUpdate = findViewById(R.id.btnUpdate);
        btnDelete = findViewById(R.id.btnDelete);
        listView = findViewById(R.id.listView);

        dbRef = FirebaseDatabase.getInstance().getReference("Employees");

        adapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, items);
        listView.setAdapter(adapter);

        dbRef.addValueEventListener(new ValueEventListener() {
            @Override
            public void onDataChange(DataSnapshot snapshot) {
                items.clear();
                keys.clear();
                for (DataSnapshot d : snapshot.getChildren()) {
                    Emp e = d.getValue(Emp.class);
                    if (e != null) {
                        items.add(e.name + " | " + e.age + " | " + e.salary);
                        keys.add(d.getKey());
                    }
                }
                adapter.notifyDataSetChanged();
            }

            @Override
            public void onCancelled(DatabaseError error) {
                Toast.makeText(MainActivity.this, "Database Error", Toast.LENGTH_SHORT).show();
            }
        });

        btnAdd.setOnClickListener(v -> {
            String name = etName.getText().toString();
            String age = etAge.getText().toString();
            String salary = etSalary.getText().toString();

            String id = dbRef.push().getKey();
            Emp e = new Emp(id, name, age, salary);
            if (id != null) {
                dbRef.child(id).setValue(e);
                clearFields();
            }
        });

        listView.setOnItemClickListener((p, v, pos, id) -> {
            selectedKey = keys.get(pos);
            dbRef.child(selectedKey).get().addOnSuccessListener(d -> {
                Emp e = d.getValue(Emp.class);
                if (e != null) {
                    etName.setText(e.name);
                    etAge.setText(e.age);
                    etSalary.setText(e.salary);
                }
            });
        });

        btnUpdate.setOnClickListener(v -> {
            if (selectedKey != null) {
                Emp e = new Emp(selectedKey, etName.getText().toString(), etAge.getText().toString(), etSalary.getText().toString());
                dbRef.child(selectedKey).setValue(e);
                clearFields();
            } else {
                Toast.makeText(this, "Select an item first", Toast.LENGTH_SHORT).show();
            }
        });

        btnDelete.setOnClickListener(v -> {
            if (selectedKey != null) {
                dbRef.child(selectedKey).removeValue();
                clearFields();
            } else {
                Toast.makeText(this, "Select an item first", Toast.LENGTH_SHORT).show();
            }
        });
    }

    private void clearFields() {
        etName.setText("");
        etAge.setText("");
        etSalary.setText("");
        selectedKey = null;
    }
}
```

### ACTIVITY.XML :

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/etName"
        android:layout_width="match_parent"
        android:layout_height="55dp"
        android:hint="Name" />

    <EditText
        android:id="@+id/etAge"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Age"
        android:inputType="number" />

    <EditText
        android:id="@+id/etSalary"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Salary"
        android:inputType="number" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:layout_marginTop="10dp"
        android:layout_marginBottom="10dp">

        <Button
            android:id="@+id/btnAdd"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Add" />

        <Button
            android:id="@+id/btnUpdate"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Update" />

        <Button
            android:id="@+id/btnDelete"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Delete" />
    </LinearLayout>

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```


## OUTPUT:



<img width="1910" height="983" alt="image" src="https://github.com/user-attachments/assets/c2f96835-ad97-48d6-82bf-06c4fbd40f5b" />




<img width="1910" height="962" alt="image" src="https://github.com/user-attachments/assets/c0deb227-587f-4eaa-af99-9b71dd79c6bf" />




## RESULT
Thus a Simple Android Application create a firebase database and to display the employee details using Firbase Real Time Database in Android Studio is developed and executed successfully.
