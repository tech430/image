import 'package:cloud_firestore/cloud_firestore.dart';
import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutter/material.dart';
import 'package:student_management/login.dart';

class Register extends StatefulWidget {
  const Register({super.key});

  @override
  State<Register> createState() => _RegisterState();
}

class _RegisterState extends State<Register> {
  final TextEditingController _nameController =  TextEditingController();
  final TextEditingController _emailController =  TextEditingController();
  final TextEditingController _passController =   TextEditingController();
  bool isLoading=false;

  final FirebaseAuth _auth= FirebaseAuth.instance;
  final FirebaseFirestore _firestore = FirebaseFirestore.instance;

  void insertData() async{

    setState(() {
      isLoading=true;
    });
    if(_nameController.text.isNotEmpty && _emailController.text.isNotEmpty && _passController.text.isNotEmpty)
      {
        try{



          await _auth.createUserWithEmailAndPassword(
              email: _emailController.text.trim(),
              password: _passController.text);

          await _firestore.collection("Student").add({
             "name" : _nameController.text,
            "email" : _emailController.text
          });

          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Insert Successful")));

        }catch(e){
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Insert Failed: ${e}")));
        }
      }
    else{
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Fill the empty fields")));
    }

    setState(() {
      isLoading=false;
    });
  }


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Container(
              height: 180,
                width: 180,
                child: Image(image: AssetImage("images/logo.png")
                )
            ),
            Padding(
              padding: const EdgeInsets.all(10.0),
              child: Column(
                
                children: [
                  TextField(
                    controller: _nameController,
                    decoration: InputDecoration(
                      prefixIcon: Icon(Icons.person),
                      border: OutlineInputBorder(),
                      labelText: "Enter the name",
                    ),
                  ),
                  SizedBox(
                    height: 10,
                  ),
                  TextField(
                    controller: _emailController,
                    decoration: InputDecoration(
                      prefixIcon: Icon(Icons.email),
                      border: OutlineInputBorder(),
                      labelText: "Enter the email",
                    ),
                  ),
                  SizedBox(
                    height: 10,
                  ),
                  TextField(
                    controller: _passController,
                    obscureText: true,
                    decoration: InputDecoration(
                      prefixIcon: Icon(Icons.password),
                      border: OutlineInputBorder(),
                      labelText: "Enter the password",
                    ),
                  ),
                  SizedBox(
                    height: 18,
                  ),
                  ElevatedButton(

                      onPressed: isLoading ? null : insertData,
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Color(0xFF640D5F),
                        minimumSize: Size(200, 70)
                      ),
                      child: isLoading ? CircularProgressIndicator() : Text(
                        "Register",
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
                        Navigator.push(context, MaterialPageRoute(builder: (context) => Login()));
                      },
                      style: ElevatedButton.styleFrom(
                        backgroundColor: Color(0xFF640D5F),
                          minimumSize: Size(200, 70),
                      ),
                      child: Text(
                        "Login",
                        style: TextStyle(
                            color: Colors.white
                        ),
                      )
                  ),
                ],
              ),
            )

          ],
        ),
      ),
    );
  }
}
