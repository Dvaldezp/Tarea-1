sing System;
using System.Collections.Generic;
using System.Linq;

namespace PrimosCalculadora.Models
{
    public class PrimosModel
    {
        public int Numero { get; set; } // Propiedad para almacenar el número ingresado por el usuario
        public List<int> Primos { get; set; } // Propiedad para almacenar la lista de números primos
        public int Resultado { get; set; } // Propiedad para almacenar la suma de los números primos
        public string Codigo { get; set; } // Propiedad para almacenar el código único para recuperar el resultado en el futuro

        public PrimosModel()
        {
            Numero = 0;
            Primos = new List<int>();
            Resultado = 0;
            Codigo = "";
        }

        // Función para determinar si un número es primo
        public bool EsPrimo(int n)
        {
            if (n < 2)
            {
                return false;
            }
            for (int i = 2; i <= Math.Sqrt(n); i++)
            {
                if (n % i == 0)
                {
                    return false;
                }
            }
            return true;
        }

        // Función para obtener la lista de números primos menores o iguales al número ingresado
        public List<int> ObtenerPrimos()
        {
            Primos = new List<int>();
            for (int i = 2; i <= Numero; i++)
            {
                if (EsPrimo(i))
                {
                    Primos.Add(i);
                }
            }
            return Primos;
        }

        // Función para calcular la suma de los números primos
        public int SumarPrimos()
        {
            Resultado = Primos.Sum();
            return Resultado;
        }

        // Función para guardar el resultado en un archivo de texto con un código único y devolver el código
        public string GuardarResultado()
        {
            Codigo = Guid.NewGuid().ToString(); // Generar un código único
            string nombreArchivo = Codigo + ".txt"; // Nombre del archivo que contiene el resultado
            System.IO.File.WriteAllText(nombreArchivo, Resultado.ToString()); // Guardar el resultado en el archivo
            return Codigo;
        }

        // Función para cargar el resultado previamente guardado utilizando el código único
        public bool CargarResultado(string codigo)
        {
            Codigo = codigo; // Almacenar el código
            string nombreArchivo = Codigo + ".txt"; // Nombre del archivo que contiene el resultado
            if (System.IO.File.Exists(nombreArchivo)) // Verificar si el archivo existe
            {
                Resultado = int.Parse(System.IO.File.ReadAllText(nombreArchivo)); // Cargar el resultado desde el archivo
                return true;
           
