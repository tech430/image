package com.example.exambeforetheday;

import android.annotation.SuppressLint;
import android.content.Intent;
import android.database.Cursor;
import android.os.Bundle;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;
import android.widget.TextView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.ArrayList;

public class MainActivity extends AppCompatActivity {
    //create object
    EditText name,age,course;
    TextView viewPart;
    Button register,viewPage, fetchBtn;

    
    DBHelper db = new DBHelper(this);

    @SuppressLint("MissingInflatedId")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //Wiring
        name = findViewById(R.id.nameId);
        age = findViewById(R.id.ageId);
        course = findViewById(R.id.courseId);
        register = findViewById(R.id.regBtnId);
        viewPage = findViewById(R.id.viewBtnId);
        fetchBtn = findViewById(R.id.fetchId);
        viewPart = findViewById(R.id.textViewId);


        register.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                addRecord();
            }
        });

        viewPage.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
               startActivity(new Intent(MainActivity.this,MainActivity2.class));
            }
        });

        fetchBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String getname = name.getText().toString().trim();
                Student student = db.getData(getname);

                if(student != null){
                    String message = "Name: "+student.getName()+"\nAge: "+student.getAge()+"\nCourse: "+student.getCourse();

                    viewPart.setText(message);
                    viewPart.setVisibility(View.VISIBLE);
                }else{
                    Toast.makeText(MainActivity.this, "Student not available", Toast.LENGTH_SHORT).show();
                }
            }
        });

    }
    
    private void addRecord(){
        String sname = name.getText().toString().trim();
        String sage = age.getText().toString().trim();
        String scourse = course.getText().toString().trim();
        
        if (!sname.isEmpty() && !sage.isEmpty() && !scourse.isEmpty())
        {
            int newAge = Integer.parseInt(sage);
            boolean result = db.insertData(sname,newAge,scourse);
            if (result){
                Toast.makeText(this, "Inserted Successfully", Toast.LENGTH_SHORT).show();
            }else{
                Toast.makeText(this, "Inserted Failed", Toast.LENGTH_SHORT).show();
            }
            
        }else{
            Toast.makeText(this, "Fill the Empty field", Toast.LENGTH_SHORT).show();
        }
    }
}