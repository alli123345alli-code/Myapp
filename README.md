
name: sine_phase_app
description: Phase control sine wave app
publish_to: 'none'

version: 1.0.0+1

environment:
  sdk: ">=3.0.0 <4.0.0"

dependencies:
  flutter:
    sdk: flutter

flutter:
  uses-material-design: true
  import 'package:flutter/material.dart';
import 'dart:math';

void main() {
  runApp(const SineApp());
}

class SineApp extends StatelessWidget {
  const SineApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const MaterialApp(
      debugShowCheckedModeBanner: false,
      home: SineHome(),
    );
  }
}

class SineHome extends StatefulWidget {
  const SineHome({super.key});

  @override
  State<SineHome> createState() => _SineHomeState();
}

class _SineHomeState extends State<SineHome> {
  double phi = 0;
  final double A = 1;
  final double w = 2 * pi * 2;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Phase Control App")),
      body: Column(
        children: [
          Expanded(
            child: CustomPaint(
              painter: SinePainter(phi, A, w),
              child: Container(),
            ),
          ),
          Padding(
            padding: const EdgeInsets.all(8.0),
            child: Text("φ = ${phi.toStringAsFixed(2)} rad",
                style: const TextStyle(fontSize: 18)),
          ),
          Slider(
            min: 0,
            max: 2 * pi,
            value: phi,
            onChanged: (value) {
              setState(() {
                phi = value;
              });
            },
          ),
        ],
      ),
    );
  }
}

class SinePainter extends CustomPainter {
  final double phi, A, w;

  SinePainter(this.phi, this.A, this.w);

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..strokeWidth = 2
      ..style = PaintingStyle.stroke;

    final path = Path();

    for (double x = 0; x < size.width; x++) {
      double t = x / size.width * 2 * pi;
      double y = A * sin(w * t + phi);

      double yPos = size.height / 2 - y * 60;

      if (x == 0) {
        path.moveTo(x, yPos);
      } else {
        path.lineTo(x, yPos);
      }
    }

    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}
flutter pub get
flutter run
flutter build apk --release
build/app/outputs/flutter-apk/app-release.apk
