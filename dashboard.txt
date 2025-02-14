import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';
import 'package:student_management/course_view.dart';

class Dashboard extends StatefulWidget {
  const Dashboard({super.key});

  @override
  State<Dashboard> createState() => _DashboardState();
}

class _DashboardState extends State<Dashboard> {
  final TextEditingController _courseController = TextEditingController();
  final TextEditingController _gradeController = TextEditingController();

  bool isLoading=false;

  FirebaseFirestore _firestore = FirebaseFirestore.instance;

  void insertCourse() async{
    setState(() {
      isLoading=true;
    });

    if(_courseController.text.isNotEmpty && _gradeController.text.isNotEmpty)
      {
        try{
          await _firestore.collection("Courses").add({
            "Course" : _courseController.text,
            "Grade" : _gradeController.text
          });

          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Inserted Successfully")));

        }catch(e){
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Inserted Failed ${e}")));
        }
      }else{
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Fill the empty field")));
    }

    setState(() {
      isLoading = false;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: Icon(Icons.home,color: Colors.white,),
        title: Text("DashBoard",
        style: TextStyle(
          color: Colors.white
        ),),
        centerTitle: true,
        backgroundColor: Color(0xFF640D5F),
      ),
      body: Padding(
        padding: const EdgeInsets.all(10.0),
        child: Column(
          children: [
            TextField(
              controller: _courseController,
              decoration: InputDecoration(
                prefixIcon: Icon(Icons.book),
                border: OutlineInputBorder(),
                labelText: "Enter the Course",
              ),
            ),
            SizedBox(
              height: 10,
            ),
            TextField(
              controller: _gradeController,

              decoration: InputDecoration(
                prefixIcon: Icon(Icons.rocket),
                border: OutlineInputBorder(),
                labelText: "Enter the Grade",
              ),
            ),
            SizedBox(
              height: 18,
            ),
            ElevatedButton(

                onPressed: isLoading ? null : insertCourse,
                style: ElevatedButton.styleFrom(
                    backgroundColor: Color(0xFF640D5F),
                    minimumSize: Size(200, 70)
                ),
                child: isLoading ? CircularProgressIndicator() : Text(
                  "Enter Course",
                  style: TextStyle(
                      color: Colors.white
                  ),
                )
            ),
            SizedBox(
              height: 18,
            ),
            ElevatedButton(

                onPressed: (){
                  Navigator.push(context, MaterialPageRoute(builder: (context) => CourseView()));
                },
                style: ElevatedButton.styleFrom(
                  backgroundColor: Color(0xFF640D5F),
                  minimumSize: Size(200, 70),
                ),
                child: Text(
                  "View Course",
                  style: TextStyle(
                      color: Colors.white
                  ),
                )
            ),

          ]

        ),
      ),
    );
  }
}
