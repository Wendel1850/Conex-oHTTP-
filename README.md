# Conex-oHTTP-
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HttpClientIP {

    public static void main(String[] args) {
        // Substitua pelo endereço IP e porta desejados (ex: um servidor local)
        String ipAddress = "192.168.1.10"; 
        int port = 8080;
        String endpoint = "/api/data"; // Caminho específico, se houver
        String urlString = "http://" + ipAddress + ":" + port + endpoint;

        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(urlString))
                .GET() // Método HTTP (GET, POST, PUT, DELETE, etc.)
                .build();

        // Enviar a requisição de forma síncrona
        try {
            HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

            System.out.println("Status Code: " + response.statusCode());
            System.out.println("Body: " + response.body());

        } catch (Exception e) {
            e.printStackTrace();
        }

        // Exemplo de requisição assíncrona (opcional)
        /*
        CompletableFuture<HttpResponse<String>> futureResponse = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());
        futureResponse.thenAccept(response -> {
            System.out.println("Async Status Code: " + response.statusCode());
            System.out.println("Async Body: " + response.body());
        });
        */
    }
}

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpURLConnectionIP {

    public static void main(String[] args) {
        // Substitua pelo endereço IP e porta desejados
        String ipAddress = "192.168.1.10";
        int port = 8080;
        String urlString = "http://" + ipAddress + ":" + port;

        try {
            URL url = new URL(urlString);
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();

            // Configurar a requisição
            connection.setRequestMethod("GET"); // Método HTTP
            connection.setConnectTimeout(5000); // Timeout de conexão (ms)
            connection.setReadTimeout(5000);    // Timeout de leitura (ms)

            int statusCode = connection.getResponseCode();
            System.out.println("Status Code: " + statusCode);

            // Ler a resposta
            BufferedReader reader;
            if (statusCode == HttpURLConnection.HTTP_OK) {
                reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            } else {
                reader = new BufferedReader(new InputStreamReader(connection.getErrorStream()));
            }

            String line;
            StringBuilder responseBody = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                responseBody.append(line);
            }
            reader.close();

            System.out.println("Body: " + responseBody.toString());
            connection.disconnect();

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
