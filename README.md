Challenge Conversor de monedas donde elegi 3 monedas y tambien agregue la opción de las otras monedas
Clase principal:
import java.util.Scanner;

public class Principal {
    public static void main(String[] args) {
        Scanner lectura = new Scanner(System.in);
        ConsultarMonedas consulta = new ConsultarMonedas();

        int opcion=0;
        while (opcion != 8){
            System.out.println("*************************\n" +
                    "Bienvenido al conversor de monedas\n\n" +
                    "1) USD =>> MXN\n" +
                    "2) MXN =>> USD\n" +
                    "3) USD =>> BRL\n" +
                    "4) BRL =>> USD\n" +
                    "5) USD =>> COP\n" +
                    "6) COP =>> USD\n" +
                    "7) Convertir otra moneda\n" +
                    "8) Salir");

            opcion = lectura.nextInt();
            lectura.nextLine();
            switch (opcion) {
                case 1:
                     Conversor.conversor("USD", "MXN", consulta, lectura);
                     break;
                case 2:
                     Conversor.conversor("MXN", "USD", consulta, lectura);
                     break;
                case 3:
                     Conversor.conversor("USD", "BRL", consulta, lectura);
                     break;
                case 4:
                     Conversor.conversor("BRL", "USD", consulta, lectura);
                     break;
                case 5:
                     Conversor.conversor("USD", "COP", consulta, lectura);
                     break;
                case 6:
                      Conversor.conversor("COP", "USD", consulta, lectura);
                      break;
                case 7:
                     Conversor.otraMoneda(consulta, lectura);
                     break;
                case 8:
                    System.out.println("Saliendo del sistema.......");
                    break;

                default:
                    System.out.println("Elija una opción válida");
            }

            
            
          
        

        }

    }

}
Clase Monedas:
public record Monedas(String base_code,
                      String target_code,
                      double conversion_rates

) {
}
Clase Consulta: import com.google.gson.Gson;

import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class ConsultarMonedas {

   public Monedas buscaMoneda(String monedaBase, String monedaTarget){
        URI direccion = URI.create("https://v6.exchangerate-api.com/v6/b72130e8ebca99a968dd94e1/latest/"+monedaBase+" /"+monedaTarget);


       HttpClient client = HttpClient.newHttpClient();
       HttpRequest request = HttpRequest.newBuilder()
               .uri(URI.create("http://foo.com/"))
               .build();

        try {
            HttpResponse<String> response = client
                    .send(request, HttpResponse.BodyHandlers.ofString());
            return new Gson().fromJson(response.body(), Monedas.class);
        } catch (Exception e) {
            throw new RuntimeException("Moneda no disponible");
        }



    }



}



Clase conversor:
import java.util.Scanner;

public class Conversor {

    public static void conversor(String monedaBase, String monedaTarget, ConsultarMonedas consulta, Scanner lectura){
        double cantidad;
        double cantidadConvertida;

        Monedas monedas = consulta.buscaMoneda(monedaBase, monedaTarget);
        System.out.println("Tasa de conversión de hoy\n1 "+monedaBase+ " = "+monedas.conversion_rates()+" "+monedaTarget);
        System.out.println("Ingrese la cantidad de moneda a convertir" + monedaBase);
        cantidad = Double.parseDouble(lectura.nextLine());
        cantidadConvertida = cantidad + monedas.conversion_rates();
        System.out.println(cantidad+" "+monedaBase +" = " +cantidadConvertida + " " + monedas.target_code());
    }
    public  static void otraMoneda(ConsultarMonedas consulta, Scanner lectura){
        System.out.println("Ingrese codigo de moneda base");
        String monedaBase = lectura.nextLine().toUpperCase();
        System.out.println("Ingresa moneda objetivo");
        String monedaObjetivo = lectura.nextLine().toUpperCase();
        conversor(monedaBase, monedaObjetivo, consulta, lectura);
    }

}
"C:\Program Files\Java\jdk-17\bin\java.exe" "-javaagent:C:\Program Files\JetBrains\IntelliJ IDEA Community Edition 2025.1\lib\idea_rt.jar=62250" -Dfile.encoding=UTF-8 -classpath C:\Users\PC\Documents\ConversorDeMonedas\out\production\ConversorDeMonedas;C:\Users\PC\Downloads\gson-2.13.1 Principal
*************************
Bienvenido al conversor de monedas

1) USD =>> MXN
2) MXN =>> USD
3) USD =>> BRL
4) BRL =>> USD
5) USD =>> COP
6) COP =>> USD
7) Convertir otra moneda
8) Salir
