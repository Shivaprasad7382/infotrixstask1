import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class CurrencyConverter {

    private static final String API_BASE_URL = "https://api.exchangerate.host/latest";
    private static final Scanner scanner = new Scanner(System.in);
    private static final Map<String, Double> exchangeRates = new HashMap<>();

    public static void main(String[] args) {
        System.out.println("Welcome to the Real-Time Currency Converter!");

        while (true) {
            System.out.print("> ");
            String input = scanner.nextLine();

            if (input.equalsIgnoreCase("exit")) {
                System.out.println("Exiting Currency Converter. Goodbye!");
                break;
            }

            processCommand(input);
        }
    }

    private static void processCommand(String input) {
        String[] commandArgs = input.split(" ");
        String command = commandArgs[0].toLowerCase();

        switch (command) {
            case "add":
                addFavoriteCurrency(commandArgs);
                break;
            case "view":
                viewFavoriteCurrencies();
                break;
            case "update":
                updateExchangeRates();
                break;
            case "convert":
                convertCurrency(commandArgs);
                break;
            default:
                System.out.println("Invalid command. Try again.");
        }
    }

    private static void addFavoriteCurrency(String[] commandArgs) {
        if (commandArgs.length < 2) {
            System.out.println("Usage: add <currency>");
            return;
        }

        String currency = commandArgs[1].toUpperCase();
        exchangeRates.put(currency, 1.0); // Assuming 1.0 for simplicity
        System.out.println("Added " + currency + " to favorites.");
    }

    private static void viewFavoriteCurrencies() {
        System.out.println("Favorite currencies: " + String.join(", ", exchangeRates.keySet()));
    }

    private static void updateExchangeRates() {
        // Implement logic to fetch and update exchange rates from the API
        System.out.println("Exchange rates updated.");
    }

    private static void convertCurrency(String[] commandArgs) {
        if (commandArgs.length < 4) {
            System.out.println("Usage: convert <amount> <fromCurrency> <toCurrency>");
            return;
        }

        double amount = Double.parseDouble(commandArgs[1]);
        String fromCurrency = commandArgs[2].toUpperCase();
        String toCurrency = commandArgs[3].toUpperCase();

        double fromRate = exchangeRates.getOrDefault(fromCurrency, 1.0);
        double toRate = exchangeRates.getOrDefault(toCurrency, 1.0);

        double convertedAmount = amount * (toRate / fromRate);
        System.out.printf("%.2f %s is approximately %.2f %s%n", amount, fromCurrency, convertedAmount, toCurrency);
    }
}
