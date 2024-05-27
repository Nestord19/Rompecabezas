/*
 * Click nbfs://nbhost/SystemFileSystem/Templates/Licenses/license-default.txt to change this license
 */
package com.mycompany.puzzle1;

/**
 *
 * @author Nestor D
 */
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;


//Puzzle1 extiende JFrame, lo que significa que es una ventana principal de la aplicación.
public class Puzzle1 extends JFrame {


//variables de instancia
    //
    private int contadorMovimientos = 0;//lleva la cuenta de los movimientos realizados.
    private JLabel contadorLabel;//etiqueta para mostrar el número de movimientos.
    private JLabel cronometroLabel;//etiqueta para mostrar el tiempo transcurrido.
    private Timer timer;//temporizador para actualizar el tiempo.
    private int segundosTranscurridos = 0;//cuenta los segundos transcurridos.

    // tamaño de la matriz 
    JButton[][] sq = new JButton[3][3];
    int m, n;

    public Puzzle1() {

        initUI();
        iniciarCronometro();
    }
// Metodo en el cual se muestra el rompecabezas Resuelto
    private boolean puzzleResuelto() {
        int[][] estadoDeseado = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 0} //0 Representa espacio vacio 
        };

        // Compara cada elemento del rompecabezas actual con el estado deseado
       for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            int valorActual = Integer.parseInt(sq[i][j].getText());
            if (valorActual != estadoDeseado[i][j]) {
                    return false;
                }
            }
        }

        return true;
    }
//Punto de entrada de la aplicación. Crea una instancia del juego, establece el título, tamaño y visibilidad de la ventana, mezcla el tablero y establece la operación de cierre.
    private void initUI() {

        JPanel topPanel = new JPanel();
        topPanel.setLayout(new GridLayout(2, 1));
        contadorLabel = new JLabel("Movimientos: 0");
        cronometroLabel = new JLabel("Tiempo: 0 s");

        topPanel.add(contadorLabel);
        topPanel.add(cronometroLabel);
        add(topPanel, BorderLayout.NORTH);

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(3, 3)); // Por ejemplo, un rompecabezas de 3x3
        add(panel);

        //Seleccionar tamaño de la ventana 
       
        ButaneListener pushed = new ButaneListener();
        //Recorrer matriz
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                JButton n = new JButton();
                sq[i][j] = n;
                sq[i][j].addActionListener(pushed);
                sq[i][j].setBackground(Color.white);// Color de fondo 
                panel.add(sq[i][j]);

            }
        }

    }

    int contadormovimiento = 0;
//Punto de entrada de la aplicación. Crea una instancia del juego, establece el título, tamaño y visibilidad de la ventana, mezcla el tablero y establece la operación de cierre.
    public static void main(String args[]) {

        Puzzle1 game = new Puzzle1();
        game.setTitle("TRABAJO Complejidad");
        game.setVisible(true);
        game.setSize(400, 400);
        game.scramble();
        game.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
//Actualiza la etiqueta que muestra el número de movimientos realizados.
    private void actualizarContador() {
        contadorLabel.setText("Movimientos: " + contadormovimiento);
    }
//Inicia un cronómetro que actualiza la etiqueta de tiempo cada segundo.
    private void iniciarCronometro() {
        timer = new Timer(1000, new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                segundosTranscurridos++;
                cronometroLabel.setText("Tiempo: " + segundosTranscurridos + " s");
            }
        });
        timer.start();
    }

    //meotod scramble desordenada y asigna valores al azar 
        public void scramble() {
            boolean[] used = new boolean[9];
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    int val = (int) (9 * Math.random());
                    while (used[val]) {
                        val = (int) (9 * Math.random());
                    }
                    used[val] = true;
                    if (val != 0) {
                        sq[i][j].setText("" + val);
                    } else {
                        sq[i][j].setText("0"); // Asignar el valor de 0 a la casilla vacía
                        sq[i][j].setBackground(Color.gray);
                        m = i;
                        n = j;
                    }
                }
            }
        }
// es una clase interna que se encarga de Manejar los clics en los botones del tablero. Actualiza el contador de movimientos, verifica si el puzzle está resuelto y muestra un mensaje de victoria si es el caso.
    class ButaneListener implements ActionListener {

        public void actionPerformed(ActionEvent e) {
            Object square = e.getSource();
            outer:
            for (int i = 0; i < 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (sq[i][j] == square) {
                        moves(i, j);
                        contadormovimiento++;
                        actualizarContador();
                        System.out.println("Movimientos" + contadormovimiento);
                         if (segundosTranscurridos>150) {
                            JOptionPane.showMessageDialog(null, "Se agoto tu tiempo.");
                        }
                        if (puzzleResuelto()) {
                            JOptionPane.showMessageDialog(null, "¡Ganaste! Felicitaciones.");
                        }
                        break outer;

                    }

                }
            }
        }
    }
// Este metodo se encarga de mover el boton presionado a la casilla que tenga vacia y este adyacente 
    public void moves(int i, int j) {
        if ((i + 1 == m && j == n) || (i - 1 == m && j == n) || (i == m && j + 1 == n) || (i == m && j - 1 == n)) {
            sq[m][n].setText(sq[i][j].getText());
            sq[m][n].setBackground(Color.white);
            //sq[i][j].setText("");
            sq[i][j].setText("0"); // Asignar el valor de 0 a la casilla vacía
            sq[i][j].setBackground(Color.gray);
            m = i;
            n = j;
        }
    }

}
