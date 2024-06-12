**El programa como tal debe abrirse descargando el archivo ZIP (MiConversorDeMonedas.ZIP) omitir el .gitingnore.

**Se abre el codigo con el nombre designado 

public class CurrencyConverter {
**Se integra la API inicialmente con mi KEY obtenida. 

 private static final String API_KEY = "c5c1df4a0747824382373851";
    private static final String BASE_URL = "https://v6.exchangerate-api.com/v6/";
    
** Se integra el HTTP response con el HTTP request. 

public static double obtenerTasaDeCambio(String baseCurrency, String targetCurrency) throws IOException, InterruptedException {
        String url = BASE_URL + API_KEY + "/latest/" + baseCurrency;
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create(url))
                .build();

        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
**Se usó la configuración base para obtener correctamente los datos de la API. 
**Se deja el siguiente condicional en caso de error.
 if (response.statusCode() != 200) {
            throw new IOException("Error en la solicitud a la API de tasas de cambio");
        }

**Se integra la biblioteca GSON:
 Gson gson = new Gson();
        JsonObject jsonObject = gson.fromJson(response.body(), JsonObject.class);
        double tasaDeCambio = jsonObject
                .getAsJsonObject("conversion_rates")
                .get(targetCurrency)
                .getAsDouble();

        return tasaDeCambio;

     **Se define el return que nos dará dependiendo la cantidad y la tasa de cambio actual (desde la api)
     public static double convertirDivisa(double cantidad, double tasaDeCambio) {
        return cantidad * tasaDeCambio;

******Se utiliza un scanner para obtener la información y ejecutarla dentro del código:
 public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("Ingresa la divisa base (por ejemplo, USD): ");
        String baseCurrency = scanner.nextLine();

        System.out.print("Ingresa la divisa objetivo (por ejemplo, EUR): ");
        String targetCurrency = scanner.nextLine();

        System.out.print("Ingresa la cantidad a convertir: ");
        double cantidad = scanner.nextDouble();

        try {
            double tasaDeCambio = obtenerTasaDeCambio(baseCurrency, targetCurrency);
            double cantidadConvertida = convertirDivisa(cantidad, tasaDeCambio);
            System.out.printf("%.2f %s son %.2f %s%n", cantidad, baseCurrency, cantidadConvertida, targetCurrency);
        } catch (IOException | InterruptedException e) {
            System.err.println("Ha ocurrido un error: " + e.getMessage());
        }
        scanner.close();
    }

# conversordedivisas
