import javax.swing.*;
import javax.swing.table.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;

public class LibraryManagementSystem extends JFrame implements ActionListener {
    private JLabel[] labels = new JLabel[7];
    private JTextField[] textFields = new JTextField[7];
    private JButton addButton, viewButton, editButton, deleteButton, clearButton, exitButton;
    private JPanel inputPanel;
    private ArrayList<String[]> booksList = new ArrayList<>();

    public LibraryManagementSystem() {
        setTitle("Library Management System");
        setSize(1200, 600);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        String[] labelNames = {"Book ID", "Title", "Author", "Publisher", "Year", "ISBN", "Copies"};
        Font labelFont = new Font("Arial", Font.BOLD, 20);

        inputPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(10, 10, 10, 10);
        
        for (int i = 0; i < labels.length; i++) {
            labels[i] = new JLabel(labelNames[i]);
            labels[i].setFont(labelFont);
            gbc.gridx = 0;
            gbc.gridy = i;
            inputPanel.add(labels[i], gbc);

            textFields[i] = new JTextField(20);
            textFields[i].setFont(labelFont);
            gbc.gridx = 1;
            inputPanel.add(textFields[i], gbc);
        }

        JPanel buttonPanel = new JPanel(new FlowLayout());
        addButton = createButton("Add", labelFont, buttonPanel);
        viewButton = createButton("View", labelFont, buttonPanel);
        editButton = createButton("Edit", labelFont, buttonPanel);
        deleteButton = createButton("Delete", labelFont, buttonPanel);
        clearButton = createButton("Clear", labelFont, buttonPanel);
        exitButton = createButton("Exit", labelFont, buttonPanel);

        gbc.gridy = labels.length;
        gbc.gridwidth = 2;
        gbc.anchor = GridBagConstraints.CENTER;
        inputPanel.add(buttonPanel, gbc);

        add(inputPanel);
        setVisible(true);
    }

    private JButton createButton(String text, Font font, JPanel panel) {
        JButton button = new JButton(text);
        button.setFont(font);
        button.addActionListener(this);
        panel.add(button);
        return button;
    }

    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == addButton) {
            String[] newBook = new String[7];
            for (int i = 0; i < newBook.length; i++) {
                newBook[i] = textFields[i].getText();
            }
            booksList.add(newBook);
            JOptionPane.showMessageDialog(this, "Book added successfully!");
            clearTextFields();
        } else if (e.getSource() == viewButton) {
            displayBooks();
        } else if (e.getSource() == editButton) {
            editBook();
        } else if (e.getSource() == deleteButton) {
            deleteBook();
        } else if (e.getSource() == clearButton) {
            clearTextFields();
        } else if (e.getSource() == exitButton) {
            System.exit(0);
        }
    }

    private void displayBooks() {
        String[] columns = {"Book ID", "Title", "Author", "Publisher", "Year", "ISBN", "Copies"};
        String[][] data = booksList.toArray(new String[0][7]);

        JTable table = new JTable(data, columns);
        table.setFont(new Font("Arial", Font.PLAIN, 20));
        table.setRowHeight(30);
        table.setIntercellSpacing(new Dimension(10, 10));

        JTableHeader header = table.getTableHeader();
        header.setFont(new Font("Arial", Font.BOLD, 20));

        for (int i = 0; i < columns.length; i++) {
            TableColumn column = table.getColumnModel().getColumn(i);
            column.setPreferredWidth(150);
        }

        JScrollPane scrollPane = new JScrollPane(table);
        JFrame frame = new JFrame("Books List");
        frame.add(scrollPane);
        frame.setSize(1000, 700);
        frame.setLocationRelativeTo(this);
        frame.setVisible(true);
    }

    private void editBook() {
        String bookID = JOptionPane.showInputDialog(this, "Enter the Book ID to edit:");
        if (bookID != null) {
            for (String[] book : booksList) {
                if (book[0].equals(bookID)) {
                    for (int i = 1; i < book.length; i++) {
                        book[i] = textFields[i].getText();
                    }
                    JOptionPane.showMessageDialog(this, "Book details updated!");
                    clearTextFields();
                    return;
                }
            }
            JOptionPane.showMessageDialog(this, "Book ID not found.");
        }
    }

    private void deleteBook() {
        String bookID = JOptionPane.showInputDialog(this, "Enter the Book ID to delete:");
        if (bookID != null) {
            for (int i = 0; i < booksList.size(); i++) {
                if (booksList.get(i)[0].equals(bookID)) {
                    booksList.remove(i);
                    JOptionPane.showMessageDialog(this, "Book deleted!");
                    clearTextFields();
                    return;
                }
            }
            JOptionPane.showMessageDialog(this, "Book ID not found.");
        }
    }

    private void clearTextFields() {
        for (JTextField textField : textFields) {
            textField.setText("");
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(LibraryManagementSystem::new);
    }
}
