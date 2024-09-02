import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Random;

public class BouncingBall extends JPanel implements ActionListener {
    private int ballX = 50, ballY = 50; // Coordenadas iniciales de la pelota
    private int ballDiameter = 30; // Diámetro inicial de la pelota
    private int ballDeltaX = 2, ballDeltaY = 2; // Velocidad de la pelota
    private Color ballColor = Color.BLUE; // Color inicial de la pelota
    private Timer timer;
    private Random random = new Random();
    private boolean isRunning = true;
    private JButton pauseResumeButton;

    public BouncingBall() {
        pauseResumeButton = new JButton("Pausa");
        pauseResumeButton.addActionListener(e -> toggleTimer());
        
        setLayout(new BorderLayout());
        add(pauseResumeButton, BorderLayout.SOUTH);

        timer = new Timer(10, this); // Crear un temporizador que se activa cada 10 ms
        timer.start(); // Iniciar el temporizador
    }

    private void toggleTimer() {
        if (isRunning) {
            timer.stop();
            pauseResumeButton.setText("Reanudar");
        } else {
            timer.start();
            pauseResumeButton.setText("Pausa");
        }
        isRunning = !isRunning;
    }

    private void changeBallColor() {
        ballColor = new Color(random.nextInt(256), random.nextInt(256), random.nextInt(256));
    }

    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        g.setColor(ballColor);
        g.fillOval(ballX, ballY, ballDiameter, ballDiameter); // Dibujar la pelota
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (ballX + ballDeltaX < 0 || ballX + ballDiameter + ballDeltaX > getWidth()) {
            ballDeltaX *= -1; // Invertir la dirección horizontal si choca con el borde izquierdo o derecho
            changeBallColor(); // Cambiar color al rebotar
        }
        if (ballY + ballDeltaY < 0) {
            ballDeltaY *= -1; // Invertir la dirección vertical si choca con el borde superior
            changeBallColor(); // Cambiar color al rebotar
        }
        if (ballY + ballDiameter + ballDeltaY > getHeight()) {
            ballDeltaY *= -1; // Invertir la dirección vertical si choca con el borde inferior
            changeBallColor(); // Cambiar color al rebotar
            ballDiameter += 5; // Aumentar el diámetro de la pelota en 5 unidades
        }

        ballX += ballDeltaX; // Actualizar la posición horizontal de la pelota
        ballY += ballDeltaY; // Actualizar la posición vertical de la pelota

        repaint(); // Volver a pintar el panel
    }

    public static void main(String[] args) {
        JFrame frame = new JFrame("Bouncing Ball");
        BouncingBall bouncingBall = new BouncingBall();
        frame.add(bouncingBall);
        frame.setSize(600, 400); // Tamaño de la ventana ajustado para acomodar el aumento del diámetro
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
