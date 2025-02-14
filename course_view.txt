import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:flutter/material.dart';

class CourseView extends StatefulWidget {
  const CourseView({super.key});

  @override
  State<CourseView> createState() => _CourseViewState();
}

class _CourseViewState extends State<CourseView> {



  final FirebaseFirestore _firestore = FirebaseFirestore.instance;

  Future<void> _deleteCourse(documentId) async
  {
    try{
      await _firestore.collection("Courses").doc(documentId).delete();

      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Deeleted Success")));

    }catch(e)
    {
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Deeleted failed ${e}")));
    }
  }

  Future<void> _updateCourse(String id, String name,String grade) async{
    try{
      await _firestore.collection("Courses").doc(id).update({
        "Course" : name,
        "Grade" :  grade
      });

    }catch(e){

    }
  }




  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(
          title: Text("Course Views"),
          centerTitle: true,
          backgroundColor: Color(0xFF640D5F),
        ),
      body: StreamBuilder(
          stream: _firestore.collection("Courses").snapshots(),
          builder: (context,snapshot){
            if(snapshot.hasError){
              return Center( child: Text("Error: ${snapshot.error}"),);
            }

            if(snapshot.connectionState == ConnectionState.waiting){
              return Center(child: CircularProgressIndicator(),);
            }

            if(!snapshot.hasData || snapshot.data!.docs.isEmpty)
              {
                return Center(child: Text("No Course available"),);
              }

            return ListView.builder(
              padding: EdgeInsets.all(10.0),
                itemCount: snapshot.data!.docs.length,
                itemBuilder: (context, index){
                  var courseDetails =snapshot.data!.docs[index];

                  return Card(
                    elevation: 8,
                    child: ListTile(
                      leading: Icon(Icons.book_outlined),
                      title: Text(
                          courseDetails['Course'],
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                        fontSize: 18
                      ),
                      ),
                      subtitle: Text(
                        courseDetails['Grade'],
                        style: TextStyle(
                          fontSize: 12
                        ),
                      ),
                      trailing: Row(
                        mainAxisSize: MainAxisSize.min,
                        children: [
                          IconButton(
                              onPressed: () async{
                                final TextEditingController courseController = TextEditingController(text: courseDetails["Course"]);
                                final TextEditingController gradeController = TextEditingController(text: courseDetails["Grade"]);
                                final confirmed =  await showDialog(
                                    context: context,
                                    builder: (context) => AlertDialog(
                                      title: Text("Update Course"),
                                      content: Column(
                                        mainAxisSize: MainAxisSize.min,
                                        children: [
                                          TextField(
                                            controller: courseController,
                                            decoration: InputDecoration(
                                              border: OutlineInputBorder(),
                                              labelText: "Course",
                                            ),
                                          ),
                                          SizedBox(height: 10,),
                                          TextField(
                                            controller: gradeController,
                                            decoration: InputDecoration(
                                              border: OutlineInputBorder(),
                                              labelText: "Grade",
                                            ),
                                          ),

                                        ],
                                      ),
                                      actions: [
                                        TextButton(
                                            onPressed: () => Navigator.of(context).pop(false),
                                            child: Text("Canceled")),
                                        TextButton(
                                            onPressed: () => Navigator.of(context).pop(true),
                                            child: Text("update")),
                                      ],

                                    ));

                                if(confirmed==true)
                                  {
                                    _updateCourse(courseDetails.id,courseController.text,gradeController.text);
                                  }



                              },
                              icon: Icon(Icons.edit)
                          ),
                          SizedBox(width: 12,),
                          IconButton(
                              onPressed: () async{
                                final confirmed = await showDialog(
                                    context: context,
                                    builder: (context) => AlertDialog(
                                      title: Text("Confirm Delete"),
                                      content: Text("Are you want to delete"),
                                      actions: [
                                        TextButton(
                                            onPressed: () => Navigator.of(context).pop(false),
                                            child: Text("Canceled")),
                                        TextButton(
                                            onPressed: () => Navigator.of(context).pop(true),
                                            child: Text("Delete")),
                                      ],
                                    ));

                                if(confirmed){
                                  _deleteCourse(courseDetails.id);
                                }
                              },
                              icon: Icon(Icons.delete,color: Colors.red,)
                          )
                        ],
                      ),

                    ),
                  );
                }
            );
          }


      ),
    );
  }
}
