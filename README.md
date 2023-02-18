# Tarea-1

Creamos un modelo PrimesModel.cs en la carpeta "Models" y agregar el siguiente código:

using System;
using System.Collections.Generic;
using System.Linq;

namespace PrimesCalculator.Models
{
    public class PrimesModel
    {
        public int Number { get; set; }
        public List<int> Primes { get; set; }
        public int Result { get; set; }
        public string Code { get; set; }

        public PrimesModel()
        {
            Number = 0;
            Primes = new List<int>();
            Result = 0;
            Code = "";
        }

        public bool IsPrime(int n)
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

        public List<int> GetPrimes()
        {
            Primes = new List<int>();
            for (int i = 2; i <= Number; i++)
            {
                if (IsPrime(i))
                {
                    Primes.Add(i);
                }
            }
            return Primes;
        }

        public int SumPrimes()
        {
            Result = Primes.Sum();
            return Result;
        }

        public string SaveResult()
        {
            Code = Guid.NewGuid().ToString();
            string filename = Code + ".txt";
            System.IO.File.WriteAllText(filename, Result.ToString());
            return Code;
        }

        public bool LoadResult(string code)
        {
            Code = code;
            string filename = Code + ".txt";
            if (System.IO.File.Exists(filename))
            {
                Result = int.Parse(System.IO.File.ReadAllText(filename));
                return true;
            }
            else
            {
                return false;
            }
        }
    }
}


Crear un controlador PrimesController.cs en la carpeta "Controllers" y agregar el siguiente código:


using Microsoft.AspNetCore.Mvc;
using PrimesCalculator.Models;

namespace PrimesCalculator.Controllers
{
    public class PrimesController : Controller
    {
        [HttpGet]
        public IActionResult Index()
        {
            return View();
        }

        [HttpPost]
        public IActionResult Calculate(PrimesModel model)
        {
            if (ModelState.IsValid)
            {
                model.GetPrimes();
                model.SumPrimes();
                model.SaveResult();
                return RedirectToAction("Result", new { code = model.Code });
            }
            else
            {
                return View("Index", model);
            }
        }

        [HttpGet]
        public IActionResult Result(string code)
        {
            PrimesModel model = new PrimesModel();
            if (model.LoadResult(code))
            {
                return View(model);
            }
            else
            {
                return RedirectToAction("Index");
            }
        }
    }
}
``


