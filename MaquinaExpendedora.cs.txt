using System;

namespace MaquinaExpendedora
{
    class Program
    {
        static void Main(string[] args)
        {
            // 1. DEFINICIÓN DE VARIABLES E INVENTARIO
            // Usamos un arreglo para simular los estantes de la máquina
            string[] nombresSodas = { "Coca-Cola", "Pepsi", "Fanta", "Sprite", "Agua" };
            double[] preciosSodas = { 18.50, 16.00, 15.50, 15.50, 12.00 };
            int[] stockSodas = { 5, 5, 3, 0, 10 }; // Sprite (índice 3) está agotada para probar la lógica

            double saldoCliente = 0.0;
            bool salir = false;

            Console.WriteLine("========================================");
            Console.WriteLine("   BIENVENIDO A LA MÁQUINA EXPENDEDORA   ");
            Console.WriteLine("========================================");

            // 2. BUCLE PRINCIPAL (El programa sigue hasta que el usuario salga)
            while (!salir)
            {
                Console.WriteLine($"\nSaldo actual: ${saldoCliente:F2}");
                MostrarProductos(nombresSodas, preciosSodas, stockSodas);

                Console.WriteLine("\n[1-5] Seleccionar soda | [6] Ingresar monedas | [0] Salir");
                Console.Write("Opción: ");
                string opcion = Console.ReadLine();

                switch (opcion)
                {
                    case "1":
                    case "2":
                    case "3":
                    case "4":
                    case "5":
                        // Convertimos la opción a índice (ej. opción 1 es índice 0)
                        int indice = int.Parse(opcion) - 1;
                        ComprarSoda(indice, nombresSodas, preciosSodas, stockSodas, ref saldoCliente);
                        break;
                    case "6":
                        Console.Write("Ingresa la cantidad de dinero a insertar: $");
                        if (double.TryParse(Console.ReadLine(), out double ingreso))
                        {
                            saldoCliente += ingreso;
                            Console.WriteLine($"Has ingresado ${ingreso:F2}");
                        }
                        else
                        {
                            Console.WriteLine("ERROR: Monto inválido.");
                        }
                        break;
                    case "0":
                        salir = true;
                        break;
                    default:
                        Console.WriteLine("Opción no válida. Intenta de nuevo.");
                        break;
                }
            }
            Console.WriteLine("Gracias por su compra. ¡Vuelva pronto!");
        }

        // --- FUNCIONES (MÉTODOS) PARA ORGANIZAR EL CÓDIGO ---

        static void MostrarProductos(string[] nombres, double[] precios, int[] stock)
        {
            Console.WriteLine("\n--- CATÁLOGO ---");
            for (int i = 0; i < nombres.Length; i++)
            {
                string estado = stock[i] > 0 ? $"${precios[i]:F2}" : "AGOTADO";
                Console.WriteLine($"[{i + 1}] {nombres[i].PadRight(10)} - {estado}");
            }
        }

        static void ComprarSoda(int indice, string[] nombres, double[] precios, int[] stock, ref double saldo)
        {
            // Validar que el índice exista (por si escriben un número fuera de rango)
            if (indice < 0 || indice >= nombres.Length) return;

            // Validar stock
            if (stock[indice] == 0)
            {
                Console.WriteLine($"\nLo siento, {nombres[indice]} está agotado.");
                return;
            }

            // Validar saldo
            if (saldo < precios[indice])
            {
                Console.WriteLine($"\nSaldo insuficiente. El precio es ${precios[indice]:F2} y tienes ${saldo:F2}.");
                return;
            }

            // Realizar la compra
            saldo -= precios[indice];
            stock[indice]--;
            Console.WriteLine($"\n¡CLICK! Has comprado una {nombres[indice]}.");
            Console.WriteLine("Cayendo al compartimento inferior...");
        }
    }
}