# CURRENCY-CONVERTER
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package currencyconverte;
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;
import java.util.Map;




 public class CurrencyConverte extends JFrame{

    private JComboBox<String> baseCurrencyDropdown, targetCurrencyDropdown;
    private JTextField amountField;
    private JLabel resultLabel;
    private JButton convertButton;

    
    private final Map<String, Double> exchangeRates = new HashMap<>();
    private final String[] currencies = {"USD", "EUR", "INR", "GBP", "JPY", "AUD", "CAD"};

    public CurrencyConverte() {
        setTitle("Currency Converter");
        setSize(800, 600);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new GridLayout(5, 2, 10, 10));

        
        initExchangeRates();

        
        baseCurrencyDropdown = new JComboBox<>(currencies);
        targetCurrencyDropdown = new JComboBox<>(currencies);

       
        amountField = new JTextField();

        
        resultLabel = new JLabel("Converted Amount: ");
        resultLabel.setForeground(Color.BLUE);
        resultLabel.setFont(new Font("Arial", Font.BOLD, 14));

        
        convertButton = new JButton("Convert");
        convertButton.setBackground(Color.CYAN);
        convertButton.setForeground(Color.BLACK);

        
        add(new JLabel("Base Currency:"));
        add(baseCurrencyDropdown);

        add(new JLabel("Target Currency:"));
        add(targetCurrencyDropdown);

        add(new JLabel("Amount:"));
        add(amountField);

        add(convertButton);
        add(resultLabel);

        
        convertButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                convertCurrency();
            }
        });
    }

   
    private void initExchangeRates() {
        exchangeRates.put("USD", 1.0);
        exchangeRates.put("EUR", 0.85);
        exchangeRates.put("INR", 74.32);
        exchangeRates.put("GBP", 0.73);
        exchangeRates.put("JPY", 110.61);
        exchangeRates.put("AUD", 1.35);
        exchangeRates.put("CAD", 1.25);
    }

    
    private void convertCurrency() {
        try {
            
            String baseCurrency = (String) baseCurrencyDropdown.getSelectedItem();
            String targetCurrency = (String) targetCurrencyDropdown.getSelectedItem();
            double amount = Double.parseDouble(amountField.getText());

           
            if (amount <= 0) {
                throw new NumberFormatException();
            }

            
            double baseRate = exchangeRates.get(baseCurrency);
            double targetRate = exchangeRates.get(targetCurrency);

            
            double convertedAmount = (amount / baseRate) * targetRate;

  
            String currencySymbol = getCurrencySymbol(targetCurrency);

            
            resultLabel.setText(String.format("Converted Amount: %.2f %s", convertedAmount, currencySymbol));

        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Please enter a valid positive number for the amount.");
        } catch (Exception ex) {
            JOptionPane.showMessageDialog(this, "Error occurred during conversion.");
        }
    }

    
    private String getCurrencySymbol(String currency) {
        switch (currency) {
            case "USD":
                return "$";
            case "EUR":
                return "€";
            case "INR":
                return "₹";
            case "GBP":
                return "£";
            case "JPY":
                return "¥";
            case "AUD":
                return "A$";
            case "CAD":
                return "C$";
            default:
                return currency;
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new CurrencyConverte().setVisible(true);
        });
    }
}
