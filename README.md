import 'dart:math';
import 'package:flutter/material.dart';

void main() {
  runApp(PakaVerseApp());
}

class PakaVerseApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'PakaVerse - Le Défi des Émotions',
      theme: ThemeData(
        primarySwatch: Colors.blue,
        visualDensity: VisualDensity.adaptivePlatformDensity,
      ),
      home: PakaVerseGame(),
    );
  }
}

class PakaVerseGame extends StatefulWidget {
  @override
  _PakaVerseGameState createState() => _PakaVerseGameState();
}

class _PakaVerseGameState extends State<PakaVerseGame> {
  final List<String> defis = [
    "Fais une danse joyeuse",
    "Imite un animal triste",
    "Exprime ta colère en chantant",
    "Saute comme un kangourou pendant 30 secondes",
    "Dis une phrase en utilisant uniquement des mots qui commencent par 'S'",
    "Fais une roue",
    "Saute à cloche-pied",
    "Dis ton prénom avec une voix de robot",
    "Imite un super-héros en sauvant quelqu'un",
    "Joue à être un extraterrestre qui découvre la Terre",
    "Fais une imitation de ton personnage de dessin animé préféré",
    "Chante comme un oiseau tout en marchant comme un crabe",
    "Raconte une blague tout en imitant un chien",
  ];

  String _defi = "Appuyez sur le bouton pour commencer!";
  int _score = 0;
  bool _isGameOver = false;
  int _tour = 1;

  // Fonction pour tirer un défi au hasard
  void _tirerDefi() {
    final random = Random();
    setState(() {
      _defi = defis[random.nextInt(defis.length)];
      _isGameOver = false;
    });
  }

  // Fonction pour gérer la réponse de l'utilisateur
  void _répondre(bool reponse) {
    setState(() {
      if (reponse) {
        _score++;
      }
      _tour++;
      if (_tour > 5) {
        _isGameOver = true; // Fin du jeu après 5 tours
      }
    });
  }

  // Widget pour afficher le défi actuel
  Widget _buildDefiText() {
    return Text(
      _defi,
      style: TextStyle(
        fontSize: 24,
        fontWeight: FontWeight.bold,
        color: Colors.white,
        fontFamily: 'Roboto',
      ),
      textAlign: TextAlign.center,
    );
  }

  // Widget pour afficher les boutons de réponse
  Widget _buildAnswerButtons() {
    return Row(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        ElevatedButton(
          onPressed: () => _répondre(true),
          style: ElevatedButton.styleFrom(
            primary: Colors.green,
            padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15),
          ),
          child: Text("Oui", style: TextStyle(fontSize: 18)),
        ),
        SizedBox(width: 20),
        ElevatedButton(
          onPressed: () => _répondre(false),
          style: ElevatedButton.styleFrom(
            primary: Colors.red,
            padding: EdgeInsets.symmetric(horizontal: 30, vertical: 15),
          ),
          child: Text("Non", style: TextStyle(fontSize: 18)),
        ),
      ],
    );
  }

  // Widget pour afficher l'écran de fin du jeu
  Widget _buildGameOverScreen() {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Text(
            "Jeu Terminé!",
            style: TextStyle(fontSize: 36, fontWeight: FontWeight.bold, color: Colors.orange),
          ),
          SizedBox(height: 20),
          Text(
            "Votre score final est : $_score",
            style: TextStyle(fontSize: 24, color: Colors.white),
          ),
          SizedBox(height: 20),
          ElevatedButton(
            onPressed: () {
              setState(() {
                _score = 0;
                _tour = 1;
                _isGameOver = false;
                _defi = "Appuyez sur le bouton pour commencer!";
              });
            },
            style: ElevatedButton.styleFrom(primary: Colors.blue),
            child: Text("Rejouer", style: TextStyle(fontSize: 18)),
          ),
        ],
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("PakaVerse - Le Défi des Émotions"),
        backgroundColor: Colors.purple,
      ),
      backgroundColor: Colors.deepPurple,
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: _isGameOver
            ? _buildGameOverScreen()
            : Column(
                mainAxisAlignment: MainAxisAlignment.center,
                crossAxisAlignment: CrossAxisAlignment.center,
                children: <Widget>[
                  _buildDefiText(),
                  SizedBox(height: 40),
                  _buildAnswerButtons(),
                  SizedBox(height: 20),
                  Text(
                    "Tour: $_tour/5",
                    style: TextStyle(fontSize: 20, color: Colors.white),
                  ),
                  SizedBox(height: 20),
                  Text(
                    "Votre score : $_score",
                    style: TextStyle(fontSize: 20, color: Colors.yellowAccent),
                  ),
                ],
              ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: _tirerDefi,
        backgroundColor: Colors.green,
        child: Icon(Icons.refresh),
      ),
    );
  }
}