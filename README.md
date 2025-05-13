import 'dart:io';

// فئة تمثل لعبة Tic-Tac-Toe
class TicTacToe {
  // لوحة اللعبة كقائمة (تمثل شبكة 3x3)
  List<String> board = List.filled(9, ' ');
  String currentPlayer = 'X'; // اللاعب الحالي (يبدأ بـ X)

  // عرض لوحة اللعبة في الوحدة التحكم
  void displayBoard() {
    print('\n ${board[0]} | ${board[1]} | ${board[2]} ');
    print('---+---+---');
    print(' ${board[3]} | ${board[4]} | ${board[5]} ');
    print('---+---+---');
    print(' ${board[6]} | ${board[7]} | ${board[8]} \n');
  }

  // تنفيذ حركة اللاعب
  bool makeMove(int position) {
    // التحقق من صحة الإدخال (1-9 وخلية فارغة)
    if (position < 1 || position > 9 || board[position - 1] != ' ') {
      print('حركة غير صالحة! حاول مرة أخرى.');
      return false;
    }
    // وضع علامة اللاعب في الخلية
    board[position - 1] = currentPlayer;
    return true;
  }

  // التحقق من وجود فائز
  bool checkWin() {
    // شروط الفوز (صفوف، أعمدة، أقطار)
    List<List<int>> winConditions = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], // صفوف
      [0, 3, 6], [1, 4, 7], [2, 5, 8], // أعمدة
      [0, 4, 8], [2, 4, 6] // أقطار
    ];

    for (var condition in winConditions) {
      if (board[condition[0]] != ' ' &&
          board[condition[0]] == board[condition[1]] &&
          board[condition[1]] == board[condition[2]]) {
        return true;
      }
    }
    return false;
  }

  // التحقق من التعادل
  bool checkDraw() {
    return !board.contains(' ');
  }

  // تبديل اللاعب
  void switchPlayer() {
    currentPlayer = currentPlayer == 'X' ? 'O' : 'X';
  }

  // إعادة تعيين اللعبة
  void resetGame() {
    board = List.filled(9, ' ');
    currentPlayer = 'X';
  }
}

// الدالة الرئيسية لتشغيل اللعبة
void main() {
  TicTacToe game = TicTacToe();
  bool playAgain = true;

  while (playAgain) {
    game.resetGame();
    bool gameOver = false;

    while (!gameOver) {
      // عرض اللوحة
      game.displayBoard();

      // طلب إدخال من اللاعب
      print('دور اللاعب ${game.currentPlayer}: أدخل رقم الخلية (1-9): ');
      String? input = stdin.readLineSync();

      // التحقق من صحة الإدخال
      int? position = int.tryParse(input ?? '');
      if (position == null) {
        print('يرجى إدخال رقم صحيح!');
        continue;
      }

      // تنفيذ الحركة
      if (game.makeMove(position)) {
        // التحقق من الفوز
        if (game.checkWin()) {
          game.displayBoard();
          print('اللاعب ${game.currentPlayer} فاز!');
          gameOver = true;
        }
        // التحقق من التعادل
        else if (game.checkDraw()) {
          game.displayBoard();
          print('تعادل!');
          gameOver = true;
        }
        // تبديل اللاعب
        else {
          game.switchPlayer();
        }
      }
    }

    // السؤال عن إعادة اللعب
    print('هل تريد اللعب مرة أخرى؟ (نعم/لا): ');
    String? response = stdin.readLineSync()?.toLowerCase();
    playAgain = response == 'نعم' || response == 'yes';
  }
  print('شكرًا للعب!');
