// Описание типа Matrix
using System;
class Matrix
{
    private int[,] data; // двумерный массив значений матрицы
    private int rows; // количество строк
    private int cols; // количество столбцов
    
    // Конструктор
    public Matrix(int[,] data)
    {
        this.data = data;
        this.rows = data.GetLength(0);
        this.cols = data.GetLength(1);
    }
    
    // Сложение матриц
    public static Matrix operator +(Matrix m1, Matrix m2)
    {
        if (m1.rows != m2.rows || m1.cols != m2.cols)
        {
            throw new MatrixOperationException("Невозможно выполнить операцию сложения матриц разных размеров", m1.rows, m1.cols, m2.rows, m2.cols);
        }
        
        int[,] resultData = new int[m1.rows, m1.cols];
        
        for (int i = 0; i < m1.rows; i++)
        {
            for (int j = 0; j < m1.cols; j++)
            {
                resultData[i, j] = m1.data[i, j] + m2.data[i, j];
            }
        }
        
        return new Matrix(resultData);
    }
    
    // Вычитание матриц
    public static Matrix operator -(Matrix m1, Matrix m2)
    {
        if (m1.rows != m2.rows || m1.cols != m2.cols)
        {
            throw new MatrixOperationException("Невозможно выполнить операцию вычитания матриц разных размеров", m1.rows, m1.cols, m2.rows, m2.cols);
        }
        
        int[,] resultData = new int[m1.rows, m1.cols];
        
        for (int i = 0; i < m1.rows; i++)
        {
            for (int j = 0; j < m1.cols; j++)
            {
                resultData[i, j] = m1.data[i, j] - m2.data[i, j];
            }
        }
        
        return new Matrix(resultData);
    }
    
    // Умножение матриц
    public static Matrix operator *(Matrix m1, Matrix m2)
    {
        if (m1.cols != m2.rows)
        {
            throw new MatrixOperationException("Невозможно выполнить операцию умножения матриц: количество столбцов первой матрицы не совпадает с количеством строк второй матрицы", m1.rows, m1.cols, m2.rows, m2.cols);
        }
        
        int[,] resultData = new int[m1.rows, m2.cols];
        
        for (int i = 0; i < m1.rows; i++)
        {
            for (int j = 0; j < m2.cols; j++)
            {
                int sum = 0;
                
                for (int k = 0; k < m1.cols; k++)
                {
                    sum += m1.data[i, k] * m2.data[k, j];
                }
                
                resultData[i, j] = sum;
            }
        }
        
        return new Matrix(resultData);
    }
    
    // Метод, возвращающий нулевую матрицу заданного размера
    public static Matrix GetEmpty(int rows, int cols)
    {
        int[,] data = new int[rows, cols];
        return new Matrix(data);
    }
    
    // Вложенный тип исключения
    public class MatrixOperationException : Exception
    {
        public int Rows1 { get; }
        public int Cols1 { get; }
        public int Rows2 { get; }
        public int Cols2 { get; }
        
        public MatrixOperationException(string message, int rows1, int cols1, int rows2, int cols2)
            : base(message)
        {
            this.Rows1 = rows1;
            this.Cols1 = cols1;
            this.Rows2 = rows2;
            this.Cols2 = cols2;
        }
        
        public MatrixOperationException(string message, Exception innerException, int rows1, int cols1, int rows2, int cols2)
            : base(message, innerException)
        {
            this.Rows1 = rows1;
            this.Cols1 = cols1;
            this.Rows2 = rows2;
            this.Cols2 = cols2;
        }
    }
}

// Пример использования типа Matrix
class Program
{
    static void Main()
    {
        Matrix a = new Matrix(new int[,] { {1, 2}, {3, 4} });
        Matrix b = new Matrix(new int[,] { {5, 6}, {7, 8} });
        
        try
        {
            Matrix c = a + b;
            // Matrix c = a - b;
            // Matrix c = a * b;
            
            Console.WriteLine("Результат операции:");
            for (int i = 0; i < c.Rows; i++)
            {
                for (int j = 0; j < c.Cols; j++)
{
                    Console.Write("{0}\t", c[i, j]);
                }
                Console.WriteLine();
            }
        }
        catch (Matrix.MatrixOperationException ex)
        {
            Console.WriteLine("Ошибка операции: {0}", ex.Message);
            Console.WriteLine("Матрица 1: {0}x{1}", ex.Rows1, ex.Cols1);
            Console.WriteLine("Матрица 2: {0}x{1}", ex.Rows2, ex.Cols2);
        }
    }
}
