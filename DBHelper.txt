package com.example.exambeforetheday;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import androidx.annotation.Nullable;

public class DBHelper extends SQLiteOpenHelper {
    final static String DB_NAME = "student.dp";
    final static int DB_VERSION =1;
    final static String TABLE_NAME = "students";
    final static String COL_ID = "id";
    final static String COL_NAME = "name";
    final static String COL_AGE = "age";
    final static String COL_COURSE = "course";

    public DBHelper(@Nullable Context context) {
        super(context, DB_NAME,null,DB_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {
        db.execSQL("CREATE TABLE students (id INTEGER PRIMARY KEY AUTOINCREMENT, name TEXT, age INTEGER,course TEXT)");

    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS "+TABLE_NAME);
        onCreate(db);
    }

    public boolean insertData(String name,int age,String course)
    {
        SQLiteDatabase db = this.getWritableDatabase();
        ContentValues values = new ContentValues();
        values.put(COL_NAME,name);
        values.put(COL_AGE,age);
        values.put(COL_COURSE,course);

        long result = db.insert(TABLE_NAME,null,values);

        return result !=-1;
    }

    public Cursor readAllData(){
        SQLiteDatabase db = this.getReadableDatabase();
        Cursor cursor = db.query(TABLE_NAME,null,null,null,null,null,null,null);

        return cursor;
    }

    public Student getData(String name){
        SQLiteDatabase db=this.getReadableDatabase();
        Cursor cursor = db.query(TABLE_NAME,null,COL_NAME + "=?",new String[]{name},null,null,null);

          if(cursor != null && cursor.moveToFirst())
        {
            Student student = new Student(
                    cursor.getInt(0),
                    cursor.getString(1),
                    cursor.getInt(2),
                    cursor.getString(3)
            );
            cursor.close();
            return  student;
        }else {
              return null;
          }


    }
}
