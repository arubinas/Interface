# Interface
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.FileWriter;
import java.io.IOException;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Random;

public class MenuApp extends JFrame {
    private JTextArea textArea;

    public MenuApp() {
        // Set up the frame
        setTitle("Menu Application");
        setSize(600, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Create the text area
        textArea = new JTextArea();
        add(new JScrollPane(textArea), BorderLayout.CENTER);

        // Create the menu bar
        JMenuBar menuBar = new JMenuBar();
        JMenu menu = new JMenu("Options");
        
        // Menu items
        JMenuItem dateTimeItem = new JMenuItem("Print Date and Time");
        JMenuItem writeFileItem = new JMenuItem("Write to Log File");
        JMenuItem changeColorItem = new JMenuItem("Change Background Color");
        JMenuItem exitItem = new JMenuItem("Exit");

        // Add menu items to menu
        menu.add(dateTimeItem);
        menu.add(writeFileItem);
        menu.add(changeColorItem);
        menu.add(exitItem);
        
        // Add menu to menu bar
        menuBar.add(menu);
        
        // Add menu bar to frame
        setJMenuBar(menuBar);

        // Action listeners
        dateTimeItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                printDateTime();
            }
        });

        writeFileItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                writeToFile();
            }
        });

        changeColorItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                changeBackgroundColor();
            }
        });

        exitItem.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                System.exit(0);
            }
        });
    }

    private void printDateTime() {
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        textArea.append(now.format(formatter) + "\n");
    }

    private void writeToFile() {
        try (FileWriter writer = new FileWriter("log.txt", true)) {
            writer.write(textArea.getText());
            textArea.append("Written to log.txt\n");
        } catch (IOException e) {
            textArea.append("Error writing to file\n");
        }
    }

    private void changeBackgroundColor() {
        Random random = new Random();
        float hue = random.nextFloat() * 0.33f; // Green hues range from 0.25 to 0.5
        Color randomGreen = Color.getHSBColor(hue, 0.8f, 0.8f);
        getContentPane().setBackground(randomGreen);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new MenuApp().setVisible(true);
            }
        });
    }
}
