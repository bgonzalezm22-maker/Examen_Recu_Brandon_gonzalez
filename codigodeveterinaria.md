using System;
using System.Collections.Generic;
using System.IO;
using Veterinaria.Modelos;

namespace Veterinaria.App
{
    class Program
    {
        static List<Mascota> mascotas = new List<Mascota>();
        static string archivo = "mascotas.txt";

        static void Main(string[] args)
        {
            CargarDatos();

            int opcion;

            do
            {
                Console.Clear();
                Console.WriteLine("===== CLINICA VETERINARIA =====");
                Console.WriteLine("1. Registrar mascota");
                Console.WriteLine("2. Listar mascotas");
                Console.WriteLine("3. Buscar mascota por nombre");
                Console.WriteLine("4. Mostrar mascota con mayor peso");
                Console.WriteLine("5. Salir");
                Console.Write("Seleccione una opción: ");

                if (!int.TryParse(Console.ReadLine(), out opcion))
                {
                    opcion = 0;
                }

                switch (opcion)
                {
                    case 1:
                        RegistrarMascota();
                        break;

                    case 2:
                        ListarMascotas();
                        break;

                    case 3:
                        BuscarMascota();
                        break;

                    case 4:
                        MostrarMascotaMayorPeso();
                        break;

                    case 5:
                        Console.WriteLine("Saliendo...");
                        break;

                    default:
                        Console.WriteLine("Opción no válida.");
                        Pausa();
                        break;
                }

            } while (opcion != 5);
        }

        static void RegistrarMascota()
        {
            Console.Clear();

            Console.Write("Nombre: ");
            string nombre = Console.ReadLine();

            Console.Write("Tipo: ");
            string tipo = Console.ReadLine();

            Console.Write("Edad: ");
            int edad = int.Parse(Console.ReadLine());

            Console.Write("Propietario: ");
            string propietario = Console.ReadLine();

            Console.Write("Peso (kg): ");
            double peso = double.Parse(Console.ReadLine());

            Mascota nueva = new Mascota(
                nombre,
                tipo,
                edad,
                propietario,
                peso
            );

            mascotas.Add(nueva);

            GuardarDatos();

            Console.WriteLine("\nMascota registrada correctamente.");
            Pausa();
        }

        static void ListarMascotas()
        {
            Console.Clear();

            if (mascotas.Count == 0)
            {
                Console.WriteLine("No hay mascotas registradas.");
            }
            else
            {
                Console.WriteLine("LISTADO DE MASCOTAS\n");

                for (int i = 0; i < mascotas.Count; i++)
                {
                    Console.WriteLine($"{i + 1}. {mascotas[i]}");
                }
            }

            Pausa();
        }

        static void BuscarMascota()
        {
            Console.Clear();

            Console.Write("Ingrese el nombre de la mascota: ");
            string nombre = Console.ReadLine();

            bool encontrada = false;

            foreach (Mascota mascota in mascotas)
            {
                if (mascota.Nombre.ToLower() == nombre.ToLower())
                {
                    Console.WriteLine("\nMascota encontrada:");
                    Console.WriteLine(mascota);
                    encontrada = true;
                }
            }

            if (!encontrada)
            {
                Console.WriteLine("No se encontró la mascota.");
            }

            Pausa();
        }

        static void MostrarMascotaMayorPeso()
        {
            Console.Clear();

            if (mascotas.Count == 0)
            {
                Console.WriteLine("No hay mascotas registradas.");
                Pausa();
                return;
            }

            Mascota mayor = mascotas[0];

            foreach (Mascota mascota in mascotas)
            {
                if (mascota.Peso > mayor.Peso)
                {
                    mayor = mascota;
                }
            }

            Console.WriteLine("Mascota con mayor peso:\n");
            Console.WriteLine(mayor);

            Pausa();
        }

        static void GuardarDatos()
        {
            using (StreamWriter sw = new StreamWriter(archivo))
            {
                foreach (Mascota mascota in mascotas)
                {
                    sw.WriteLine(
                        $"{mascota.Nombre};{mascota.Tipo};{mascota.Edad};{mascota.Propietario};{mascota.Peso}"
                    );
                }
            }
        }

        static void CargarDatos()
        {
            if (!File.Exists(archivo))
                return;

            string[] lineas = File.ReadAllLines(archivo);

            foreach (string linea in lineas)
            {
                string[] datos = linea.Split(';');

                if (datos.Length == 5)
                {
                    mascotas.Add(new Mascota(
                        datos[0],
                        datos[1],
                        int.Parse(datos[2]),
                        datos[3],
                        double.Parse(datos[4])
                    ));
                }
            }
        }

        static void Pausa()
        {
            Console.WriteLine("\nPresione una tecla para continuar...");
            Console.ReadKey();
        }
    }
}
