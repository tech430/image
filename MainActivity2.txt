package com.example.exambeforetheday;

import android.annotation.SuppressLint;
import android.database.Cursor;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.ListView;
import android.widget.Toast;

import androidx.activity.EdgeToEdge;
import androidx.appcompat.app.AppCompatActivity;
import androidx.core.graphics.Insets;
import androidx.core.view.ViewCompat;
import androidx.core.view.WindowInsetsCompat;

import java.util.ArrayList;

public class MainActivity2 extends AppCompatActivity {
    ListView viewList;
    DBHelper db=new DBHelper(this);

    @SuppressLint("MissingInflatedId")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);

        viewList = findViewById(R.id.listViewId);
        Cursor cursor= db.readAllData();
        ArrayList<String> list= new ArrayList<>();

        if(cursor!=null && cursor.moveToFirst())
        {
            do{
                String record= "ID: "+cursor.getInt(0) +
                        "\nName: " + cursor.getString(1) +
                        "\nAge: " + cursor.getInt(2) +
                        "\nCourse: "+cursor.getString(3);
                list.add(record);
            }while (cursor.moveToNext());

            cursor.close();

            ArrayAdapter<String> adapter = new ArrayAdapter<>(
                    MainActivity2.this, android.R.layout.simple_list_item_1,list
            );
            viewList.setAdapter(adapter);
        }else {
            Toast.makeText(MainActivity2.this, "No Available", Toast.LENGTH_SHORT).show();
        }

    }
}