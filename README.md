# QIBSIP
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.*;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;

public class OnlineReservationSystem {

    private JFrame frame;
    private JPanel mainPanel, reservationPanel, loginPanel, ticketsPanel, cancellationPanel;
    private JTextField usernameField, nameField, contactField, fromField, toField;
    private JPasswordField passwordField;
    private JComboBox<String> genderComboBox, classComboBox;
    private JTextField numberField, nameFieldDetails, dateField, pnrField;
    private JButton loginButton, logoutButton, bookTicketButton, cancelTicketButton, cancelSubmitButton;
    private Map<String, Reservation> reservations;
    private AtomicInteger ticketCounter;
    private JTextArea ticketDetailsArea;
    private JButton bookAnotherTicketButton, viewBookedTicketsButton;

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            OnlineReservationSystem app = new OnlineReservationSystem();
            app.createAndShowGUI();
        });
    }

    public OnlineReservationSystem() {
        reservations = new HashMap<>();
        ticketCounter = new AtomicInteger(1000); // Start PNR numbers from 1000
    }

        private void createAndShowGUI() {
        frame = new JFrame("Online Reservation System");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(600, 600);
        frame.setLocationRelativeTo(null);  // Center the frame on the screen

        mainPanel = new JPanel(new CardLayout());
        frame.add(mainPanel);

        loginPanel = createLoginPanel();
        reservationPanel = createReservationPanel();
        cancellationPanel = createCancellationPanel();

        mainPanel.add(loginPanel, "Login");
        mainPanel.add(reservationPanel, "Reservation");
        mainPanel.add(cancellationPanel, "Cancellation");

        frame.setVisible(true);
    }


    private JPanel createLoginPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);  // Space between components

        JLabel headerLabel = new JLabel("Online Reservation System");
        headerLabel.setFont(new Font("Arial", Font.BOLD, 24));
        headerLabel.setForeground(Color.BLACK);

        JLabel usernameLabel = new JLabel("Username:");
        JLabel passwordLabel = new JLabel("Password:");

        usernameField = new JTextField(20);
        passwordField = new JPasswordField(20);  // Correctly using JPasswordField

        loginButton = new JButton("Login");
        loginButton.addActionListener(e -> onLogin());

        constraints.gridx = 0;
        constraints.gridy = 0;
        constraints.gridwidth = 2;
        panel.add(headerLabel, constraints);

        constraints.gridx = 0;
        constraints.gridy = 1;
        constraints.gridwidth = 1;
        panel.add(usernameLabel, constraints);

        constraints.gridx = 1;
        panel.add(usernameField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 2;
        panel.add(passwordLabel, constraints);

        constraints.gridx = 1;
        panel.add(passwordField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 3;
        panel.add(loginButton, constraints);

        return panel;
    }

    
    private JPanel createReservationPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);  // Space between components
        panel.setBackground(Color.LIGHT_GRAY);  // Light grey background for the reservation panel

        JLabel nameLabel = new JLabel("Name:");
        JLabel genderLabel = new JLabel("Gender:");
        JLabel contactLabel = new JLabel("Contact:");
        JLabel fromLabel = new JLabel("From:");
        JLabel toLabel = new JLabel("To:");
        JLabel numberLabel = new JLabel("Number:");
        JLabel nameLabelDetails = new JLabel("Name:");
        JLabel dateLabel = new JLabel("Date:");
        JLabel classLabel = new JLabel("Class:");

        nameField = new JTextField(20);
        contactField = new JTextField(20);
        fromField = new JTextField(20);
        toField = new JTextField(20);
        numberField = new JTextField("12345", 20);  // Pre-filled with a sample number
        nameFieldDetails = new JTextField("Express Name", 20);  // Pre-filled with a sample name
        dateField = new JTextField(20);
        
        String[] genders = {"Male", "Female", "Other"};
        genderComboBox = new JComboBox<>(genders);
        
        String[] classes = {"First Class", "Second Class", "Sleeper"};
        classComboBox = new JComboBox<>(classes);

        bookTicketButton = new JButton("Book Ticket");
        bookTicketButton.addActionListener(e -> onBookTicket());

        bookAnotherTicketButton = new JButton("Book Another Ticket");
        bookAnotherTicketButton.addActionListener(e -> onBookAnotherTicket());

        viewBookedTicketsButton = new JButton("View Booked Tickets");
        viewBookedTicketsButton.addActionListener(e -> onViewBookedTickets());

        cancelTicketButton = new JButton("Cancel Ticket");
        cancelTicketButton.addActionListener(e -> showCancellationPanel());


        constraints.gridx = 0;
        constraints.gridy = 0;
        panel.add(nameLabel, constraints);

        constraints.gridx = 1;
        panel.add(nameField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 1;
        panel.add(genderLabel, constraints);

        constraints.gridx = 1;
        panel.add(genderComboBox, constraints);

        constraints.gridx = 0;
        constraints.gridy = 2;
        panel.add(contactLabel, constraints);

        constraints.gridx = 1;
        panel.add(contactField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 3;
        panel.add(fromLabel, constraints);

        constraints.gridx = 1;
        panel.add(fromField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 4;
        panel.add(toLabel, constraints);

        constraints.gridx = 1;
        panel.add(toField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 5;
        panel.add(numberLabel, constraints);

        constraints.gridx = 1;
        panel.add(numberField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 6;
        panel.add(nameLabelDetails, constraints);

        constraints.gridx = 1;
        panel.add(nameFieldDetails, constraints);

        constraints.gridx = 0;
        constraints.gridy = 7;
        panel.add(dateLabel, constraints);

        constraints.gridx = 1;
        panel.add(dateField, constraints);

        constraints.gridx = 0;
        constraints.gridy = 8;
        panel.add(classLabel, constraints);

        constraints.gridx = 1;
        panel.add(classComboBox, constraints);

        constraints.gridx = 0;
        constraints.gridy = 9;
        panel.add(bookTicketButton, constraints);

        constraints.gridx = 1;
        panel.add(bookAnotherTicketButton, constraints);

        constraints.gridx = 0;
        constraints.gridy = 10;
        panel.add(viewBookedTicketsButton, constraints);

        constraints.gridx = 1;
        panel.add(cancelTicketButton, constraints);

        return panel;
    }


    private JPanel createCancellationPanel() {
        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        JLabel pnrLabel = new JLabel("Enter PNR to Cancel:");

        pnrField = new JTextField(8);  // Smaller PNR input field for cancellation
        cancelSubmitButton = new JButton("Submit");
        cancelSubmitButton.addActionListener(e -> onCancelTicket());

        panel.add(pnrLabel);
        panel.add(pnrField);
        panel.add(cancelSubmitButton);

        return panel;
    }


    private void onLogin() {
        if (usernameField.getText().equals("user") && new String(passwordField.getPassword()).equals("password")) {
            showReservationPanel();
        } else {
            JOptionPane.showMessageDialog(frame, "Invalid username or password!");
        }
    }

    private void onBookTicket() {
        String name = nameField.getText();
        String gender = (String) genderComboBox.getSelectedItem();
        String contact = contactField.getText();
        String from = fromField.getText();
        String to = toField.getText();
        String number = numberField.getText();
        String nameDetails = nameFieldDetails.getText();
        String date = dateField.getText();
        String travelClass = (String) classComboBox.getSelectedItem();

        String pnr = "PNR" + ticketCounter.getAndIncrement(); // Generate random PNR number

        Reservation reservation = new Reservation(pnr, name, gender, contact, from, to, number, nameDetails, date, travelClass);
        reservations.put(pnr, reservation);

        JOptionPane.showMessageDialog(frame, "Ticket booked successfully with PNR: " + pnr);
        displayTicket(reservation);
    }
    

    private void onBookAnotherTicket() {
        
        nameField.setText("");
        contactField.setText("");
        fromField.setText("");
        toField.setText("");
        numberField.setText("12345");
        nameFieldDetails.setText("Express Name");
        dateField.setText("");
    }

    private void onViewBookedTickets() {
        StringBuilder bookedTicketsInfo = new StringBuilder();
        for (Reservation reservation : reservations.values()) {
            bookedTicketsInfo.append(reservation.toString()).append("\n\n");
        }
        JOptionPane.showMessageDialog(frame, bookedTicketsInfo.toString(), "Booked Tickets", JOptionPane.INFORMATION_MESSAGE);
    }

    private void onCancelTicket() {
        String pnr = pnrField.getText();
        Reservation reservation = reservations.get(pnr);

        if (reservation != null) {
            int confirm = JOptionPane.showConfirmDialog(frame, "Do you want to cancel the ticket with PNR: " + pnr, "Confirm Cancellation", JOptionPane.YES_NO_OPTION);
            if (confirm == JOptionPane.YES_OPTION) {
                reservations.remove(pnr);
                JOptionPane.showMessageDialog(frame, "Ticket with PNR " + pnr + " has been cancelled.");
            }
        } else {
            JOptionPane.showMessageDialog(frame, "No ticket found with PNR: " + pnr);
        }
    }

    private void displayTicket(Reservation reservation) {
        ticketDetailsArea = new JTextArea(15, 40);
        ticketDetailsArea.setEditable(false);
        ticketDetailsArea.setBackground(new Color(173, 216, 230)); // Light blue background

        ticketDetailsArea.setText(reservation.toString());
        JOptionPane.showMessageDialog(frame, new JScrollPane(ticketDetailsArea), "Ticket Details", JOptionPane.INFORMATION_MESSAGE);
    }

    private void showReservationPanel() {
        CardLayout cardLayout = (CardLayout) mainPanel.getLayout();
        cardLayout.show(mainPanel, "Reservation");
    }

    private void showCancellationPanel() {
        CardLayout cardLayout = (CardLayout) mainPanel.getLayout();
        cardLayout.show(mainPanel, "Cancellation");
    }

    class Reservation {
        private String pnr, name, gender, contact, from, to, number, nameDetails, date, travelClass;

        public Reservation(String pnr, String name, String gender, String contact, String from, String to, String number, String nameDetails, String date, String travelClass) {
            this.pnr = pnr;
            this.name = name;
            this.gender = gender;
            this.contact = contact;
            this.from = from;
            this.to = to;
            this.number = number;
            this.nameDetails = nameDetails;
            this.date = date;
            this.travelClass = travelClass;
        }

        @Override
        public String toString() {
            return "PNR: " + pnr + "\nName: " + name + "\nGender: " + gender + "\nContact: " + contact + "\nFrom: " + from + "\nTo: " + to
                    + "\nTrain Name: " + nameDetails + "\nDate of Journey: " + date + "\nClass: " + travelClass;
        }
    }
}
