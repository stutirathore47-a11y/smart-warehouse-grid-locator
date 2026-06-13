class GridItem {
    // Private attributes (Encapsulation)
    private String id;
    private String name;
    private int quantity;
    // Constructor to initialize values
    public GridItem(String id, String name, int quantity) {
        this.id = id;
        this.name = name;
        this.quantity = quantity;
    }
    // Getters
    public String getId() {
        return id;
    }
    public String getName() {
        return name;
    }
    public int getQuantity() {
        return quantity;
    }
    // Display item details
    @Override
    public String toString() {
        return "[ID: " + id + ", Name: " + name + ", Quantity: " + quantity + "]";
    }
}
// -----------------------------------------------
// Step 2: Warehouse Class with 2D Array
// -----------------------------------------------
class Warehouse {
    // 2D array of GridItem objects
    private GridItem[][] grid;
    private int rows;
    private int cols;
    // Constructor — defines grid size
    public Warehouse(int rows, int cols) {
        this.rows = rows;
        this.cols = cols;
        this.grid = new GridItem[rows][cols];
        // All cells are null (empty) by default
    }
    // -----------------------------------------------
    // Step 3: Place item at a specific grid position
    // -----------------------------------------------
    public void placeItem(int row, int col, GridItem item) {
        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            System.out.println("Error: Position [" + row + "][" + col + "] is out of bounds.");
            return;
        }
        if (grid[row][col] != null) {
            System.out.println("Warning: Slot [" + row + "][" + col + "] is already occupied by " + grid[row][col].getName());
            return;
        }
        grid[row][col] = item;
        System.out.println("Placed " + item.getName() + " at [" + row + "][" + col + "]");
    }
    // -----------------------------------------------
    // Step 4 & 5: Search by Item ID using nested loops
    // -----------------------------------------------
    public void searchItem(String id) {
        System.out.println("\nSearching for item ID: " + id);
        // Outer loop — iterates over rows
        for (int r = 0; r < rows; r++) {
            // Inner loop — iterates over columns
            for (int c = 0; c < cols; c++) {
                // Check null to avoid NullPointerException
                if (grid[r][c] != null && grid[r][c].getId().equals(id)) {
                    System.out.println("Item found at Row: " + r + ", Column: " + c);
                    System.out.println("Item Details: " + grid[r][c]);
                    return; // Stop searching once found
                }
            }
        }
        // If we reach here, item was not found
        System.out.println("Item not found in warehouse.");
    }
    // -----------------------------------------------
    // Step 6 (Optional): Display the entire grid
    // -----------------------------------------------
    public void displayGrid() {
        System.out.println("\n--- Warehouse Grid (" + rows + " x " + cols + ") ---");
        for (int r = 0; r < rows; r++) {
            for (int c = 0; c < cols; c++) {
                if (grid[r][c] != null) {
                    System.out.printf("[%s]", grid[r][c].getId());
                } else {
                    System.out.printf("[----]");
                }
                System.out.print(" ");
            }
            System.out.println(); // New line after each row
        }
        System.out.println("-------------------------------");
    }
    // -----------------------------------------------
    // Step 6 (Optional): Remove an item from the grid
    // -----------------------------------------------
    public void removeItem(int row, int col) {
        if (row < 0 || row >= rows || col < 0 || col >= cols) {
            System.out.println("Error: Position out of bounds.");
            return;
        }
        if (grid[row][col] == null) {
            System.out.println("Slot [" + row + "][" + col + "] is already empty.");
            return;
        }
        System.out.println("Removed: " + grid[row][col].getName() + " from [" + row + "][" + col + "]");
        grid[row][col] = null;
    }
}
// -----------------------------------------------
// Step 7: Main Method — Testing
// -----------------------------------------------
public class Main {
    public static void main(String[] args) {
        // Create a 5x5 warehouse
        Warehouse warehouse = new Warehouse(5, 5);
        System.out.println("=== Populating Warehouse Grid ===");
        // Populate the grid with GridItem objects
        warehouse.placeItem(0, 0, new GridItem("I101", "Laptop",     10));
        warehouse.placeItem(1, 2, new GridItem("I102", "Phone",      25));
        warehouse.placeItem(2, 4, new GridItem("I103", "Tablet",      8));
        warehouse.placeItem(3, 1, new GridItem("I104", "Keyboard",   50));
        warehouse.placeItem(4, 3, new GridItem("I105", "Monitor",     5));
        warehouse.placeItem(0, 3, new GridItem("I106", "Mouse",      40));
        warehouse.placeItem(2, 1, new GridItem("I107", "Headphones", 15));
        warehouse.placeItem(4, 0, new GridItem("I108", "Webcam",     20));
        // Display the full grid
        warehouse.displayGrid();
        // -----------------------------------------------
        // Search Operations
        // -----------------------------------------------
        System.out.println("\n=== Search Operations ===");
        // Test 1: Item exists
        warehouse.searchItem("I102");
        // Expected Output:
        // Searching for item ID: I102
        // Item found at Row: 1, Column: 2
        // Item Details: [ID: I102, Name: Phone, Quantity: 25]
        // Test 2: Item does not exist
        warehouse.searchItem("I999");
        // Expected Output:
        // Searching for item ID: I999
        // Item not found in warehouse.
        // Test 3: Another search
        warehouse.searchItem("I105");
        // Expected Output:
        // Searching for item ID: I105
        // Item found at Row: 4, Column: 3
        // -----------------------------------------------
        // Optional: Remove an item and search again
        // -----------------------------------------------
        System.out.println("\n=== Optional: Remove and Re-search ===");
        warehouse.removeItem(1, 2);         // Remove Phone
        warehouse.searchItem("I102");        // Should not be found now
        warehouse.displayGrid();             // Show updated grid
    }
}
