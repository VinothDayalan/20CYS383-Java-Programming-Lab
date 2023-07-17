# NFT OSINT

This Java application allows you to make API calls to the Blockchain.info API and retrieve information based on the provided search input. The application provides a simple graphical user interface (GUI) built using Swing.

## Prerequisites

- Java Development Kit (JDK) installed
- A compatible Java IDE or compiler

## How to Run

1. Clone or download the project source code.
2. Open the project in your Java IDE.
3. Build and compile the code.
4. Run the `BlockchainInfoApiCaller` class.

## Usage

1. Upon running the application, a window titled "NFT OSINT" will appear.
2. In the search field, enter the value you want to search for.
3. Click the "Search" button or press Enter to initiate the API call.
4. The result will be displayed in the text area below the search field.

## Functionality

- The application uses the Blockchain.info API to retrieve information based on the search input.
- The search input can be a Bitcoin (BTC) address starting with "1" or "3", a Bitcoin SegWit (bech32) address starting with "bc1", a Litecoin (LTC) address starting with "L", an Ethereum (ETH) address starting with "0x", a Ripple (XRP) address starting with "X" or "r", or a Bitcoin Testnet address starting with "t1" or "tb1".
- If the search input matches any of the specified cryptocurrency addresses, the corresponding cryptocurrency name will be displayed in the result area.
- If the search input is invalid or not recognized, an appropriate error message will be displayed.

## Code

```

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class BlockchainInfoApiCaller extends JFrame implements ActionListener {

    private JTextField searchField;
    private JTextArea resultArea;

    public BlockchainInfoApiCaller() {
        super("NFT OSINT");

        // Create and configure components
        JLabel searchLabel = new JLabel("Search:");
        searchField = new JTextField(20);
        JButton searchButton = new JButton("Search");
        searchButton.addActionListener(this);
        resultArea = new JTextArea(10, 30);
        resultArea.setEditable(false);

        // Create layout and add components
        JPanel contentPane = new JPanel(new BorderLayout());
        JPanel inputPanel = new JPanel(new FlowLayout());
        inputPanel.add(searchLabel);
        inputPanel.add(searchField);
        inputPanel.add(searchButton);
        contentPane.add(inputPanel, BorderLayout.NORTH);
        contentPane.add(new JScrollPane(resultArea), BorderLayout.CENTER);

        // Configure JFrame
        setContentPane(contentPane);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        pack();
        setLocationRelativeTo(null);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            BlockchainInfoApiCaller app = new BlockchainInfoApiCaller();
            app.setVisible(true);
        });
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() instanceof JButton) {
            String search = searchField.getText().trim();
            if (!search.isEmpty()) {
                performApiCall(search);
            }
        }
    }

    private void performApiCall(String search) {
        String url;
        if (search.startsWith("1") || search.startsWith("3")) {
            url = "https://blockchain.info/rawaddr/" + search;
        } else if (search.startsWith("bc1")) {
            url = "https://blockchain.info/rawaddr/" + search;
        } else if (search.startsWith("L")) {
            resultArea.setText("Litecoin (LTC)");
            return;
        } else if (search.startsWith("0x")) {
            resultArea.setText("Ethereum (ETH)");
            return;
        } else if (search.startsWith("X") || search.startsWith("r")) {
            resultArea.setText("Ripple (XRP)");
            return;
        } else if (search.startsWith("t1") || search.startsWith("tb1")) {
            resultArea.setText("Bitcoin Testnet");
            return;
        } else {
            resultArea.setText("Invalid search input");
            return;
        }

        try {
            URL apiURL = new URL(url);
            HttpURLConnection connection = (HttpURLConnection) apiURL.openConnection();
            connection.setRequestMethod("GET");

            int responseCode = connection.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) {
                BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
                StringBuilder response = new StringBuilder();
                String line;
                while ((line = reader.readLine()) != null) {
                    response.append(line);
                }
                reader.close();

                // Print the response
                resultArea.setText(response.toString());
            } else {
                resultArea.setText("Failed to fetch data. Response code: " + responseCode);
            }
        } catch (IOException ex) {
            resultArea.setText("Error occurred: " + ex.getMessage());
        }
    }
}
```
## Dependencies

The application does not have any external dependencies beyond the standard Java libraries.

## Gif

<video>
    <source src="gif.mov" type="video/mov">
</video>
