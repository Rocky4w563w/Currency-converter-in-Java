# Currency-converter-in-Java

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.util.Scanner;
import org.json.JSONObject;

public class CurrencyConverter {

    // Replace with your actual API key from exchangerate-api.com
    private static final String API_KEY = "YOUR_API_KEY";

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter base currency (e.g., USD): ");
        String baseCurrency = scanner.nextLine().toUpperCase();

        System.out.print("Enter target currency (e.g., EUR): ");
        String targetCurrency = scanner.nextLine().toUpperCase();

        System.out.print("Enter amount to convert: ");
        double amount = scanner.nextDouble();

        try {
            double rate = getExchangeRate(baseCurrency, targetCurrency);
            if (rate != -1) {
                double convertedAmount = amount * rate;
                System.out.printf("Converted Amount: %.2f %s\n", convertedAmount, targetCurrency);
            } else {
                System.out.println("Unable to fetch exchange rate.");
            }
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }

        scanner.close();
    }

    public static double getExchangeRate(String base, String target) {
        try {
            String urlStr = "https://v6.exchangerate-api.com/v6/" + API_KEY + "/latest/" + base;
            URL url = new URL(urlStr);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            connection.setRequestMethod("GET");
            BufferedReader in = new BufferedReader(
                    new InputStreamReader(connection.getInputStream()));

            String inputLine;
            StringBuilder content = new StringBuilder();

            while ((inputLine = in.readLine()) != null) {
                content.append(inputLine);
            }

            in.close();
            connection.disconnect();

            JSONObject json = new JSONObject(content.toString());
            JSONObject rates = json.getJSONObject("conversion_rates");
            return rates.getDouble(target);

        } catch (Exception e) {
            return -1;
        }
    }
}
Output:- Enter base currency (e.g., USD): USD
Enter target currency (e.g., EUR): INR
Enter amount to convert: 100

Converted Amount: 8307.50 INR
