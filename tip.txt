import 'package:flutter/material.dart';

class TipCalculator extends StatefulWidget {
  const TipCalculator({super.key});

  @override
  State<TipCalculator> createState() => _TipCalculatorState();
}

class _TipCalculatorState extends State<TipCalculator> {
  int _tipPercentage=0;
  int _people=1;
  double _billAmount=0;
  double _serviceCharge=0;
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("TIP Calculator"),
        centerTitle: true,
        backgroundColor: Colors.teal,
        leading: Icon(
            Icons.calculate,
        color: Colors.white,),
        actions: [
          IconButton(
              onPressed: (){},
              icon: Icon(Icons.clear))
        ],
      ),
      body: Column(
        children: [
          Container(
            margin: EdgeInsets.all(10),
            
            height: 200,
            width: double.infinity,
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(15),
              color: Colors.teal,
            ),
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Text("Total Amount",
                style: TextStyle(
                  fontSize: 28,
                  color: Colors.white
                ),
                ),
                Text("${totalBillAmount(_billAmount, _people, _tipPercentage)}",
                  style: TextStyle(
                      fontSize: 28,
                      color: Colors.white
                  ),
                ),

              ],
            ),

          ),
          Container(
            margin: EdgeInsets.all(10),
            padding: EdgeInsets.all(12),
            height: 300,
            width: double.infinity,
            decoration: BoxDecoration(
              border: Border.all(
                width:2,
                color: Colors.deepOrange,
                style: BorderStyle.solid
              ),
              borderRadius: BorderRadius.circular(10),
              color: Colors.transparent,
            ),
            child: Column(
                children: [
                  TextField(

                    decoration: InputDecoration(
                      hintText: "Enter the amount",
                      prefixIcon: Icon(Icons.currency_bitcoin)

                    ),
                    keyboardType: TextInputType.numberWithOptions(decimal: true),
                    onChanged: (String value){
                      try{
                      _billAmount=double.parse(value);
                      }catch(Exception){
                        _billAmount=0;
                      }
                    },
                  ),
                  SizedBox(height: 10,),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text("No.of.Person",
                      style: TextStyle(
                        fontSize: 18,

                      ),
                      ),
                      Row(
                        children: [
                          InkWell(
                            onTap: (){
                              setState(() {
                                if(_people>1)
                                --_people;
                              });
                            },
                            child: Container(

                              height: 40,
                              width: 40,
                              decoration: BoxDecoration(
                                shape: BoxShape.circle,
                                color: Colors.purple
                              ),
                              child: Center(
                                child: Text(
                                    "-",
                                style: TextStyle(
                                  color: Colors.white,
                                  fontSize: 30,
                                  fontWeight: FontWeight.bold
                                ),
                                ),
                              ),
                            ),
                          ),
                          SizedBox(width: 5,),
                          Text(
                              "${_people}",
                          style: TextStyle(
                            fontSize: 22
                          ),
                          ),
                          SizedBox(width: 5,),
                          InkWell(
                            onTap: (){
                              setState(() {
                                ++_people;
                              });
                            },
                            child: Container(

                              height: 40,
                              width: 40,
                              decoration: BoxDecoration(
                                  shape: BoxShape.circle,
                                  color: Colors.purple
                              ),
                              child: Center(
                                child: Text(
                                  "+",
                                  style: TextStyle(
                                      color: Colors.white,
                                      fontSize: 30,
                                      fontWeight: FontWeight.bold
                                  ),
                                ),
                              ),
                            ),
                          ),
                        ],
                      )
                    ],
                  ),
                  SizedBox(height: 10,),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text(
                        "Service Charge",
                        style: TextStyle(
                          fontSize: 18,
                        ),
                      ),
                      Text(
                        "${calServiceCharge(_billAmount, _tipPercentage)}",
                        style: TextStyle(
                          fontSize: 18,
                        ),
                      )
                    ],
                  ),
                  SizedBox(height: 10,),
                  Column(
                    children: [
                      Text(
                          "${_tipPercentage}%",
                      style: TextStyle(
                        fontSize: 18,
                      ),
                      ),
                      Slider(
                          value: _tipPercentage.toDouble(),
                          min: 0,
                          max: 100,
                          activeColor: Colors.green,
                          inactiveColor: Colors.red,
                          onChanged: (double newVal){
                            setState(() {
                              _tipPercentage=newVal.round();
                            });
                          })
                    ],
                  )

                ],
            ),
          )
        ],
      ),
    );
  }

  totalBillAmount(double bill,int persons, int percentage)
  {
    double service = calServiceCharge(bill, percentage);
    return (bill  + service ) / persons ;
  }

  calServiceCharge(double bill, int percentage)
  {
    double amount= bill * (percentage / 100);
    return amount.roundToDouble();
  }
}

/*
class TipCalculator extends StatefulWidget {
  const TipCalculator({super.key});

  @override
  State<TipCalculator> createState() => _TipCalculatorState();
}

class _TipCalculatorState extends State<TipCalculator> {

  int _tipPerc =0;
  int _noOfPerson=1;
  double _billAmount=0;
  double _serviceCharge=0;


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.purple.shade50,

      appBar: AppBar(
        title: Center(child: Text('Tip Calculator', style: TextStyle(fontSize: 30,fontWeight: FontWeight.bold, color: Colors.deepPurple),)),
      ),
      body: Container(

        margin: EdgeInsets.fromLTRB(0, 50, 0, 0),
        alignment: Alignment.center,
        color: Colors.white12,

        child: ListView(
          scrollDirection: Axis.vertical,
          padding: EdgeInsets.all(10),

          children: [

            Container(
              width: 100,
              height: 200,
              decoration: BoxDecoration(
                color: Colors.deepPurple.shade500,
                borderRadius: BorderRadius.circular(20),

              ),

              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,

                children: [
                  Text('Bill Amount Per Person',style: TextStyle(fontSize: 30,fontWeight: FontWeight.bold,color: Colors.white),),
                  Text('Rs. ${calcBillPerPers(
                      _billAmount,_tipPerc, _noOfPerson,
                      _serviceCharge)}',style: TextStyle(fontSize: 30,fontWeight: FontWeight.bold,color: Colors.white)),
                ],
              ),
            ),

            Container(
              padding: const EdgeInsets.all(20),
              margin: const EdgeInsets.all(10),
              decoration: BoxDecoration(
                color: Colors.transparent,
                border: Border.all(
                  width: 2.0,
                  color: Colors.purpleAccent,
                  style: BorderStyle.solid,
                ),
                borderRadius: BorderRadius.circular(20),
              ),

              child: Column(

                children: [

                  TextField(
                      keyboardType: const TextInputType.numberWithOptions(decimal: true),
                      style: const TextStyle(color: Colors.deepPurple, fontWeight: FontWeight.bold,fontSize: 25),

                      decoration: const InputDecoration(
                        prefixIcon: Icon(Icons.attach_money),
                      ),

                      onChanged: (String value){
                        try{
                          _billAmount = double.parse(value);

                        }
                        catch(Exception){
                          _billAmount =0.0;

                        }
                      }
                  ),

                  const SizedBox(height: 20,),

                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,

                    children: [
                      const Text('Number of Person', style: TextStyle(fontWeight: FontWeight.bold, color: Colors.grey, fontSize: 15),),

                      Row(

                        children: [
                          InkWell(
                            onTap: () {
                              setState(() {
                                if(_noOfPerson>1){
                                  _noOfPerson--;
                                }
                              });
                            },

                            child:  Container(
                              width: 40,
                              height: 40,
                              padding: EdgeInsets.all(10),
                              decoration: BoxDecoration(
                                  color: Colors.deepPurple,
                                  borderRadius: BorderRadius.circular(5)
                              ),

                              child: const Center(child: Text('-',style: TextStyle(fontSize: 12,color: Colors.white,fontWeight: FontWeight.bold),)),
                            ),
                          ),
                          SizedBox(
                            width: 15,
                          ),

                          Text('$_noOfPerson',style: TextStyle(fontSize: 20,fontWeight: FontWeight.bold,color: Colors.deepPurple),),

                          SizedBox(
                            width: 15,
                          ),

                          InkWell(
                            onTap: () {
                              setState(() {
                                _noOfPerson++;
                              });
                            },

                            child:  Container(
                              width: 40,
                              height: 40,
                              padding: EdgeInsets.all(10),
                              decoration: BoxDecoration(
                                  color: Colors.deepPurple,
                                  borderRadius: BorderRadius.circular(5)
                              ),
                              child: const Center(child: Text('+',style: TextStyle(fontSize: 12,color: Colors.white,fontWeight: FontWeight.bold),)),
                            ),
                          ),
                        ],
                      ),



                    ],
                  ),
                  SizedBox(
                    height: 10,
                  ),
                  Row(
                    mainAxisAlignment: MainAxisAlignment.spaceBetween,
                    children: [
                      Text('Service Charge',style: TextStyle(fontWeight: FontWeight.bold,fontSize: 17,color: Colors.grey),),
                      Text('${calcService(
                          _billAmount, _tipPerc, _noOfPerson,
                          _serviceCharge)}',style: TextStyle(fontWeight: FontWeight.bold,fontSize: 18,color: Colors.deepPurple),),
                    ],
                  ),

                  Column(
                    children: [
                      Text('$_tipPerc%',style: TextStyle(fontWeight: FontWeight.bold,fontSize: 20,color: Colors.grey)),
                      Slider(value: _tipPerc.toDouble(),

                          min:0, max:100, activeColor: Colors.purple, inactiveColor: Colors.grey,

                          label: "$_tipPerc",

                          onChanged: (double newVal){
                            setState(() {
                              _tipPerc = newVal.round();
                            });
                          }),
                    ],
                  )
                ],
              ),
            )
          ],
        ),
      ),
    );
  }
  calcService(double bill,int percentage,int person,double totalService){

    bill = _billAmount;
    percentage = _tipPerc;
    person = _noOfPerson;
    totalService= _serviceCharge;

    if(bill<0 || bill==null || bill.toString().isEmpty){

    }else{
      totalService=(bill * percentage)/100;
    }
    return totalService;
  }

  calcBillPerPers(double bill,int percentage,int person,double totalService){
    bill = _billAmount;
    percentage = _tipPerc;
    person = _noOfPerson;
    totalService= _serviceCharge;
    double billAm=0;
    double temp = calcService(bill,percentage,person,totalService);

    billAm =(bill+temp)/person;


    return billAm.toStringAsFixed(2);
  }

}

*/