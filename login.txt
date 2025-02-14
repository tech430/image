import 'package:firebase_auth/firebase_auth.dart';
import 'package:flutter/material.dart';
import 'package:student_management/dashboard.dart';

class Login extends StatefulWidget {
  const Login({super.key});

  @override
  State<Login> createState() => _LoginState();
}

class _LoginState extends State<Login> {
  final TextEditingController _emailController =  TextEditingController();
  final TextEditingController _passController =   TextEditingController();


  final FirebaseAuth _auth = FirebaseAuth.instance;

  Future<void> loginUser() async{
    if(_emailController.text.isNotEmpty && _passController.text.isNotEmpty)
      {
        UserCredential userCredential= await _auth.signInWithEmailAndPassword(
            email: _emailController.text,
            password: _passController.text);

        if(userCredential!=null){
            ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Login Sucessfully")));
            Navigator.push(context, MaterialPageRoute(builder: (context) => Dashboard()));
        }else{
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Invalid Credentials")));
        }
      }
    else{
      ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text("Fill the empty field")));
    }
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

                      onPressed: loginUser,
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
