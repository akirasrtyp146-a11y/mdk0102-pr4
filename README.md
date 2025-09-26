# mdk0102-pr4





using System;
using System.Linq;

class Program
{
    static void Main()
    {
        Console.WriteLine("Вариант 8: Обработка одномерного массива");
        Console.WriteLine("========================================");
        
        // Ввод размера массива
        Console.Write("Введите размер массива n: ");
        int n = int.Parse(Console.ReadLine());
        
        // Создание и заполнение массива
        double[] array = new double[n];
        Random rand = new Random();
        
        Console.WriteLine("\nИсходный массив:");
        for (int i = 0; i < n; i++)
        {
            array[i] = Math.Round(rand.NextDouble() * 20 - 10, 2); // числа от -10 до 10
            Console.Write(array[i] + " ");
        }
        Console.WriteLine();
        
        // 1) Номер минимального элемента массива
        int minIndex = FindMinElementIndex(array);
        Console.WriteLine($"\n1) Номер минимального элемента: {minIndex + 1} (индекс {minIndex}, значение {array[minIndex]})");
        
        // 2) Сумма элементов между первым и вторым отрицательными элементами
        double sumBetweenNegatives = SumBetweenFirstTwoNegatives(array);
        if (sumBetweenNegatives != double.MinValue)
        {
            Console.WriteLine($"2) Сумма элементов между первым и вторым отрицательными: {sumBetweenNegatives:F2}");
        }
        else
        {
            Console.WriteLine("2) В массиве меньше двух отрицательных элементов");
        }
        
        // Преобразование массива
        double[] transformedArray = TransformArray(array);
        Console.WriteLine("\nПреобразованный массив (сначала элементы с модулем <= 1, потом остальные):");
        foreach (double element in transformedArray)
        {
            Console.Write(element + " ");
        }
        Console.WriteLine();
    }
    
    // 1) Поиск номера минимального элемента
    static int FindMinElementIndex(double[] arr)
    {
        int minIndex = 0;
        for (int i = 1; i < arr.Length; i++)
        {
            if (arr[i] < arr[minIndex])
            {
                minIndex = i;
            }
        }
        return minIndex;
    }
    
    // 2) Сумма элементов между первым и вторым отрицательными элементами
    static double SumBetweenFirstTwoNegatives(double[] arr)
    {
        int firstNegativeIndex = -1;
        int secondNegativeIndex = -1;
        
        // Поиск первого отрицательного элемента
        for (int i = 0; i < arr.Length; i++)
        {
            if (arr[i] < 0)
            {
                firstNegativeIndex = i;
                break;
            }
        }
        
        // Поиск второго отрицательного элемента
        if (firstNegativeIndex != -1)
        {
            for (int i = firstNegativeIndex + 1; i < arr.Length; i++)
            {
                if (arr[i] < 0)
                {
                    secondNegativeIndex = i;
                    break;
                }
            }
        }
        
        // Если нашли два отрицательных элемента, вычисляем сумму между ними
        if (firstNegativeIndex != -1 && secondNegativeIndex != -1 && secondNegativeIndex - firstNegativeIndex > 1)
        {
            double sum = 0;
            for (int i = firstNegativeIndex + 1; i < secondNegativeIndex; i++)
            {
                sum += arr[i];
            }
            return sum;
        }
        
        return double.MinValue; // специальное значение для обозначения ошибки
    }
    
    // Преобразование массива: сначала элементы с модулем <= 1, потом остальные
    static double[] TransformArray(double[] arr)
    {
        // Создаем два списка: для элементов с модулем <= 1 и для остальных
        var smallModule = new System.Collections.Generic.List<double>();
        var largeModule = new System.Collections.Generic.List<double>();
        
        foreach (double element in arr)
        {
            if (Math.Abs(element) <= 1)
            {
                smallModule.Add(element);
            }
            else
            {
                largeModule.Add(element);
            }
        }
        
        // Объединяем списки
        double[] result = new double[arr.Length];
        int index = 0;
        
        foreach (double element in smallModule)
        {
            result[index++] = element;
        }
        
        foreach (double element in largeModule)
        {
            result[index++] = element;
        }
        
        return result;
    }
}
