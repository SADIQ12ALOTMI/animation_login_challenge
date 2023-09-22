# animation_login_challenge

## Image![Screenshot 2023-09-22 025527](https://github.com/SADIQ12ALOTMI/animation_login_challenge/assets/78486332/0794215f-09f9-4530-abbe-0d622ff58c0e)

## video design
https://github.com/SADIQ12ALOTMI/animation_login_challenge/assets/78486332/ecae539a-b406-4b3b-a7af-634f652c81c4



### dart code 
``` dart



import 'dart:math';

import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: LoginPage(),
    );
  }
}

class LoginPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("FLutter Login Animation"),
        centerTitle: true,
      ),
      backgroundColor: Colors.black,
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Center(
            child: SizedBox(
              width: 500,
              height: 500,
              child: Stack(
                alignment: Alignment.center,
                children: [
                  AnimatedSquare(),
                  LoginForm(),
                ],
              ),
            ),
          ),
        ],
      ),
    );
  }
}

class AnimatedSquare extends StatefulWidget {
  @override
  _AnimatedSquareState createState() => _AnimatedSquareState();
}

class _AnimatedSquareState extends State<AnimatedSquare> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  bool isHovered = false;
  @override
  void initState() {
    super.initState();
    _controller = AnimationController(
      vsync: this,
      duration: Duration(seconds: 4),
    );

    _controller.addListener(() {
      setState(() {});
    });

    _controller.repeat();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }
  @override
  Widget build(BuildContext context) {
    return MouseRegion(
      onEnter: (_) {
        setState(() {
          isHovered = true;
        });
      },
      onExit: (_) {
        setState(() {
          isHovered = false;
        });
      },
      child: Container(
        width: 500,
        height: 500,
        child: Stack(
          alignment: Alignment.center,
          children: [
            // Wrap each AnimatedBorder with a RotationTransition
            RotationTransition(
              turns: Tween(begin: 1.0, end: 0.0).animate(_controller),
              child: AnimatedBorder(color: isHovered? Colors.green:Colors.white, radius: 150.0,controller: _controller,rotate: 360, onHover: isHovered,),
            ),
            RotationTransition(
              turns: Tween(begin: 0.0, end: 1.0).animate(_controller),
              child: AnimatedBorder(color: isHovered? Colors.pink:Colors.white, radius: 170.0,controller: _controller,rotate: 160, onHover: isHovered,),
            ),
            RotationTransition(
              turns: Tween(begin: 0.0, end: 1.0).animate(_controller),
              child: AnimatedBorder(color: isHovered?Colors.yellow:Colors.white, radius: 150.0,controller: _controller,rotate: 180, onHover: isHovered,),
            ),
          ],
        ),
      ),
    );
  }
}

class AnimatedBorder extends StatelessWidget {

  final Color color;
  final double radius;
  final double  rotate;
  final bool onHover;
  final AnimationController controller;
  AnimatedBorder({required this.color, required this.radius, required this.controller, required this.rotate, required this.onHover});


  @override
  Widget build(BuildContext context) {
    return Transform.rotate(
      angle: rotate,
      child: CustomPaint(
        painter: BorderPainter(
          radius,
          onHover,
          color: color,
          quarterTurns: 3,

        ),
        child: Container(
          width: 500,
          height: 500,
          color: Colors.transparent, // Set the background color to transparent
        ),
      ),
    );
  }
}


class BorderPainter extends CustomPainter {
  final Color color;
  final int quarterTurns;
final double radius;
final bool onHover;
  BorderPainter(this.radius, this.onHover, {required this.color, required this.quarterTurns});

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..color = color
      ..style = PaintingStyle.stroke
      ..strokeWidth = onHover?3.0:1.5;

    final shadowPaint = Paint()
      ..color = color.withOpacity(0.4) // Set shadow color with alpha 0
      ..maskFilter = MaskFilter.blur(BlurStyle.normal, 20.0)
      ..style = PaintingStyle.stroke
      ..strokeWidth =onHover? 20.0:0.0; // Increase strokeWidth to control spread

    final center = Offset(size.width / 2, size.height / 2);
    final halfSide = size.width / 2 - 20.0; // Subtract 20 to stay inside the border

    // Define the radius for rounded corners
    final borderRadius = BorderRadius.circular(radius);

    // Create a rounded rectangle path
    final borderPath = Path()
      ..addRRect(RRect.fromRectAndCorners(
        Rect.fromCenter(
          center: center,
          width: halfSide * 2,
          height: halfSide * 2,
        ),
        topLeft: borderRadius.topLeft,
        topRight: borderRadius.topRight,
        bottomLeft: borderRadius.bottomLeft,
        bottomRight: borderRadius.bottomRight,
      ));

    // Rotate the canvas
    canvas.save();
    canvas.translate(center.dx, center.dy);
    canvas.rotate(2 * pi * quarterTurns / 4);
    canvas.translate(-center.dx, -center.dy);

    // Draw the shadow
    canvas.drawPath(borderPath, shadowPaint);

    // Draw the border
    canvas.drawPath(borderPath, paint);

    // Restore the canvas
    canvas.restore();
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) {
    return false;
  }
}




class LoginForm extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      width: 300,
      height: double.infinity,
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        crossAxisAlignment: CrossAxisAlignment.center,
        children: [
          Text(
            "Login",
            style: TextStyle(
              fontSize: 32,
              color: Colors.white,
            ),
          ),
          SizedBox(height: 20),
          InputField(placeholder: "Username"),
          InputField(placeholder: "Password", isPassword: true),
          SubmitButton(text: "Sign in"),
          SizedBox(height: 20),
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceBetween,
            children: [
              Text(
                "Forget Password",
                style: TextStyle(
                  color: Colors.white,
                ),
              ),
              Text(
                "Signup",
                style: TextStyle(
                  color: Colors.white,
                ),
              ),
            ],
          ),
        ],
      ),
    );
  }
}

class InputField extends StatelessWidget {
  final String placeholder;
  final bool isPassword;

  InputField({required this.placeholder, this.isPassword = false});

  @override
  Widget build(BuildContext context) {
    return Container(
      width: 300,
      margin: EdgeInsets.symmetric(vertical: 10),
      child: TextField(
        style: TextStyle(color: Colors.white),
        obscureText: isPassword,
        decoration: InputDecoration(
          hintText: placeholder,
          hintStyle: TextStyle(color: Color(0x80FFFFFF)),
          enabledBorder: OutlineInputBorder(
            borderSide: BorderSide(color: Colors.white, width: 2.0),
            borderRadius: BorderRadius.circular(40.0),
          ),
          focusedBorder: OutlineInputBorder(
            borderSide: BorderSide(color: Colors.white, width: 2.0),
            borderRadius: BorderRadius.circular(40.0),
          ),
        ),
      ),
    );
  }
}

class SubmitButton extends StatelessWidget {
  final String text;

  SubmitButton({required this.text});

  @override
  Widget build(BuildContext context) {
    return Container(
      height: 50,
      margin: EdgeInsets.symmetric(vertical: 14),
      alignment: Alignment.center,
      decoration: BoxDecoration(
        borderRadius: BorderRadius.circular(40.0),
        gradient: LinearGradient(
          colors: [Color(0xFFff357a), Color(0xFFfff172)],
          begin: Alignment.topLeft,
          end: Alignment.bottomRight,
          stops: [0.0, 1.0],
          tileMode: TileMode.clamp,
          transform: GradientRotation(45 * 3.1415927 / 180), // 45 degrees in radians
        ),
      ),
      child: Text(
        text,
        style: TextStyle(
          fontSize: 18,
          color: Colors.white,
        ),
      ),
    );
  }
}


```


