import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;

public class HotelManagementSystem {
    // Data Storage
    private static HashMap<Integer, Room> rooms = new HashMap<>();
    private static HashMap<Integer, Customer> customers = new HashMap<>();
    private static int bookingIdCounter = 1;

    public static void main(String[] args) {
        // Initialize GUI
        JFrame frame = new JFrame("Hotel Management System");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(500, 400);

        // Main Menu
        JPanel menuPanel = new JPanel();
        menuPanel.setLayout(new GridLayout(5, 1, 10, 10));
        menuPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));

        JButton btnManageRooms = new JButton("Manage Rooms");
        JButton btnCheckIn = new JButton("Check In");
        JButton btnCheckOut = new JButton("Check Out");
        JButton btnViewCustomers = new JButton("View Customers");
        JButton btnExit = new JButton("Exit");

        menuPanel.add(btnManageRooms);
        menuPanel.add(btnCheckIn);
        menuPanel.add(btnCheckOut);
        menuPanel.add(btnViewCustomers);
        menuPanel.add(btnExit);

        frame.add(menuPanel);
        frame.setVisible(true);

        // Button Listeners
        btnManageRooms.addActionListener(e -> openManageRooms(frame));
        btnCheckIn.addActionListener(e -> openCheckIn(frame));
        btnCheckOut.addActionListener(e -> openCheckOut(frame));
        btnViewCustomers.addActionListener(e -> viewCustomers(frame));
        btnExit.addActionListener(e -> System.exit(0));
    }

    // Manage Rooms
    private static void openManageRooms(JFrame parentFrame) {
        JFrame frame = new JFrame("Manage Rooms");
        frame.setSize(400, 300);
        frame.setLayout(new BorderLayout());

        JPanel topPanel = new JPanel(new FlowLayout());
        JLabel lblRoomNumber = new JLabel("Room Number:");
        JTextField txtRoomNumber = new JTextField(10);
        JButton btnAddRoom = new JButton("Add Room");
        topPanel.add(lblRoomNumber);
        topPanel.add(txtRoomNumber);
        topPanel.add(btnAddRoom);

        DefaultListModel<String> roomListModel = new DefaultListModel<>();
        JList<String> roomList = new JList<>(roomListModel);
        rooms.forEach((k, v) -> roomListModel.addElement("Room " + k + " - " + (v.isOccupied() ? "Occupied" : "Available")));
        JScrollPane scrollPane = new JScrollPane(roomList);

        btnAddRoom.addActionListener(e -> {
            try {
                int roomNumber = Integer.parseInt(txtRoomNumber.getText());
                if (rooms.containsKey(roomNumber)) {
                    JOptionPane.showMessageDialog(frame, "Room already exists!", "Error", JOptionPane.ERROR_MESSAGE);
                } else {
                    rooms.put(roomNumber, new Room(roomNumber));
                    roomListModel.addElement("Room " + roomNumber + " - Available");
                    txtRoomNumber.setText("");
                }
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(frame, "Invalid room number!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        frame.add(topPanel, BorderLayout.NORTH);
        frame.add(scrollPane, BorderLayout.CENTER);
        frame.setVisible(true);
    }

    // Check-In
    private static void openCheckIn(JFrame parentFrame) {
        JFrame frame = new JFrame("Check In");
        frame.setSize(400, 300);
        frame.setLayout(new GridLayout(6, 2, 10, 10));
        frame.setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);

        JLabel lblName = new JLabel("Customer Name:");
        JTextField txtName = new JTextField();
        JLabel lblPhone = new JLabel("Phone:");
        JTextField txtPhone = new JTextField();
        JLabel lblRoomNumber = new JLabel("Room Number:");
        JTextField txtRoomNumber = new JTextField();
        JButton btnCheckIn = new JButton("Check In");

        frame.add(lblName);
        frame.add(txtName);
        frame.add(lblPhone);
        frame.add(txtPhone);
        frame.add(lblRoomNumber);
        frame.add(txtRoomNumber);
        frame.add(new JLabel());
        frame.add(btnCheckIn);

        btnCheckIn.addActionListener(e -> {
            String name = txtName.getText();
            String phone = txtPhone.getText();
            int roomNumber;
            try {
                roomNumber = Integer.parseInt(txtRoomNumber.getText());
                if (!rooms.containsKey(roomNumber)) {
                    JOptionPane.showMessageDialog(frame, "Room does not exist!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }
                Room room = rooms.get(roomNumber);
                if (room.isOccupied()) {
                    JOptionPane.showMessageDialog(frame, "Room is already occupied!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }
                Customer customer = new Customer(bookingIdCounter++, name, phone, roomNumber);
                customers.put(customer.getId(), customer);
                room.setOccupied(true);
                JOptionPane.showMessageDialog(frame, "Check-in successful!");
                txtName.setText("");
                txtPhone.setText("");
                txtRoomNumber.setText("");
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(frame, "Invalid room number!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        frame.setVisible(true);
    }

    // Check-Out with Payment
    private static void openCheckOut(JFrame parentFrame) {
        JFrame frame = new JFrame("Check Out");
        frame.setSize(300, 200);
        frame.setLayout(new GridLayout(4, 2, 10, 10));

        JLabel lblRoomNumber = new JLabel("Room Number:");
        JTextField txtRoomNumber = new JTextField();
        JButton btnCheckOut = new JButton("Check Out");
        JLabel lblAmount = new JLabel("Amount Due: ");
        JTextField txtAmountDue = new JTextField();
        txtAmountDue.setEditable(false);

        frame.add(lblRoomNumber);
        frame.add(txtRoomNumber);
        frame.add(lblAmount);
        frame.add(txtAmountDue);
        frame.add(new JLabel());
        frame.add(btnCheckOut);

        btnCheckOut.addActionListener(e -> {
            try {
                int roomNumber = Integer.parseInt(txtRoomNumber.getText());
                if (!rooms.containsKey(roomNumber)) {
                    JOptionPane.showMessageDialog(frame, "Room does not exist!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }
                Room room = rooms.get(roomNumber);
                if (!room.isOccupied()) {
                    JOptionPane.showMessageDialog(frame, "Room is not occupied!", "Error", JOptionPane.ERROR_MESSAGE);
                    return;
                }

                Customer customer = findCustomerByRoomNumber(roomNumber);
                if (customer != null) {
                    // Show amount due
                    txtAmountDue.setText("$" + room.getRoomPrice());

                    // Prompt for payment
                    String paymentMethod = JOptionPane.showInputDialog(frame, "Enter payment method (cash/card):");
                    double amountPaid = Double.parseDouble(JOptionPane.showInputDialog(frame, "Enter amount paid:"));
                    if (amountPaid >= room.getRoomPrice()) {
                        // Successful payment
                        customer.setPaid(true);
                        room.setOccupied(false);  // Free up the room
                        JOptionPane.showMessageDialog(frame, "Check-out successful! Payment completed.");
                    } else {
                        JOptionPane.showMessageDialog(frame, "Insufficient payment amount.", "Error", JOptionPane.ERROR_MESSAGE);
                    }
                }
                txtRoomNumber.setText("");
            } catch (NumberFormatException ex) {
                JOptionPane.showMessageDialog(frame, "Invalid input!", "Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        frame.setVisible(true);
    }

    // Helper method to find customer by room number
    private static Customer findCustomerByRoomNumber(int roomNumber) {
        for (Customer customer : customers.values()) {
            if (customer.getRoomNumber() == roomNumber) {
                return customer;
            }
        }
        return null;
    }

    // View Customers
    private static void viewCustomers(JFrame parentFrame) {
        JFrame frame = new JFrame("View Customers");
        frame.setSize(400, 300);

        DefaultListModel<String> customerListModel = new DefaultListModel<>();
        JList<String> customerList = new JList<>(customerListModel);
        customers.forEach((k, v) -> customerListModel.addElement("ID: " + k + ", Name: " + v.getName() + ", Room: " + v.getRoomNumber()));

        JScrollPane scrollPane = new JScrollPane(customerList);

        frame.add(scrollPane);
        frame.setVisible(true);
    }
}

// Room Class with pricing
class Room {
    private int roomNumber;
    private boolean occupied;
    private double roomPrice;

    public Room(int roomNumber) {
        this.roomNumber = roomNumber;
        this.occupied = false;
        this.roomPrice = 100.00; // Example fixed price
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public boolean isOccupied() {
        return occupied;
    }

    public void setOccupied(boolean occupied) {
        this.occupied = occupied;
    }

    public double getRoomPrice() {
        return roomPrice;
    }
}

// Customer Class with payment info
class Customer {
    private int id;
    private String name;
    private String phone;
    private int roomNumber;
    private boolean isPaid;

    public Customer(int id, String name, String phone, int roomNumber) {
        this.id = id;
        this.name = name;
        this.phone = phone;
        this.roomNumber = roomNumber;
        this.isPaid = false;  // By default, the payment is not made
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public String getPhone() {
        return phone;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public boolean isPaid() {
        return isPaid;
    }

    public void setPaid(boolean isPaid) {
        this.isPaid = isPaid;
    }
}
