import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

class TicTacToeGUI extends JFrame implements ActionListener {
    private JButton[][] buttons = new JButton[3][3];
    private char[][] board = new char[3][3];
    private boolean player1Turn = true;
    private char player1Mark = 'X';
    private char player2Mark = 'O';
    private String player1Name;
    private String player2Name;

    public TicTacToeGUI() {
        player1Name = JOptionPane.showInputDialog(this, "Enter Player 1 Name (X):");
        if (player1Name == null || player1Name.trim().isEmpty()) {
            player1Name = "Player 1";
        }
        player2Name = JOptionPane.showInputDialog(this, "Enter Player 2 Name (O):");
        if (player2Name == null || player2Name.trim().isEmpty()) {
            player2Name = "Player 2";
        }

        setTitle("Tic Tac Toe - " + player1Name + " vs " + player2Name);
        setSize(400, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new GridLayout(3, 3));
        initBoard();
        setVisible(true);
    }
    private void initBoard() {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                board[i][j] = ' ';
                buttons[i][j] = new JButton("");
                buttons[i][j].setFont(new Font(Font.SANS_SERIF, Font.BOLD, 60));
                buttons[i][j].addActionListener(this);
                add(buttons[i][j]);
            }
        }
    }
    @Override
    public void actionPerformed(ActionEvent e) {
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (e.getSource() == buttons[i][j] && board[i][j] == ' ') {
                    char currentMark = player1Turn ? player1Mark : player2Mark;
                    String currentPlayer = player1Turn ? player1Name : player2Name;

                    board[i][j] = currentMark;
                    buttons[i][j].setText(String.valueOf(currentMark));

                    if (checkWin(currentMark)) {
                        showResult(currentPlayer + " wins!");
                        return;
                    } else if (checkDraw()) {
                        showResult("It's a draw!");
                        return;
                    }

                    player1Turn = !player1Turn;
                    return;
                }
            }
        }
    }
