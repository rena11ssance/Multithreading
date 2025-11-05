# Многопоточность. Класс Thread

Задача. Имеется пустой участок земли (двумерный массив) и план сада, который необходимо реализовать. Эту задачу выполняют два садовника, которые не хотят встречаться друг с другом. Первый садовник начинает работу с верхнего левого угла сада и перемещается слева направо, сделав ряд, он спускается вниз. Второй садовник начинает работу с нижнего правого угла сада и перемещается снизу вверх, сделав ряд, он перемещается влево. Если садовник видит, что участок сада уже выполнен другим садовником, он идет дальше. Садовники должны работать параллельно. Создать многопоточное приложение, моделирующее работу садовников.

Решение.

```
using System;
using System.Threading;

namespace Multithreading
{
    internal class Program
    {
        static void Main(string[] args)
        {
            int cols = Convert.ToInt32(Console.ReadLine());
            int rows = Convert.ToInt32(Console.ReadLine());

            int[,] field = new int[rows, cols];

            for (int i = 0; i < rows; i++)
            {
                for (int j = 0; j < cols; j++)
                {
                    field[i, j] = 3;
                }
            }

            Console.WriteLine("\nНачальное состояние: ");
            PrintField(field, rows, cols);

            ParameterizedThreadStart threadStart = new ParameterizedThreadStart(Gardner2);
            Thread thread = new Thread(threadStart);

            thread.Start(field);

            Gardner1(field, rows, cols);

            thread.Join();

            Console.WriteLine("\nФинальное состояние: ");
            PrintField(field, rows, cols);
        }

        static void Gardner1(int[,] field, int rows, int cols)
        {
            for (int i = 0; i < rows; i++)
            {
                for (int j = 0; j < cols; j++)
                {
                    if (field[i, j] == 3)
                    {
                        field[i, j] = 1;
                        Thread.Sleep(10);
                    }

                    else if (field[i, j] == 2)
                    {
                        continue;
                    }
                }
            }
        }

        static void Gardner2(object fieldObj)
        {
            int[,] field = (int[,])fieldObj;

            int rows = field.GetLength(0);
            int cols = field.GetLength(1);

            for (int i = rows - 1; i >= 0; i--)
            {
                for (int j = cols - 1; j >= 0; j--)
                {
                    if (field[i, j] == 3)
                    {
                        field[i, j] = 2;
                        Thread.Sleep(10);
                    }

                    else if (field[i, j] == 1)
                    {
                        continue;
                    }
                }
            }
        }

        static void PrintField(int[,] field, int rows, int cols)
        {
            Console.WriteLine("\nРезультат:");
            for (int i = 0; i < rows; i++)
            {
                for (int j = 0; j < cols; j++)
                {
                    Console.Write(field[i, j] + " ");
                }
                Console.WriteLine();
            }
        }
    }
}
```
