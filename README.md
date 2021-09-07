#include <iostream>

using namespace std;

void SortArrayInsert(int* array, int size)
{
	for (int i = 1; i < size; i++)
	{
		int temp = array[i];
		int j;
		for (j = i - 1; j >= 0; j--)
		{
			if (array[j] > temp)
				array[j + 1] = array[j];
			else break;
		}
		array[j + 1] = temp;
	}
}

void SortArrayInsert(int**& matrix, int*& array, int size1, int size2)
{
	for (int i = 1; i < size1; i++)
	{
		int temp = array[i];
		int* temparray = new int[size2];
		for (int t = 0; t < size2; t++)
		{
			temparray[t] = matrix[i][t];
		}

		int j;
		for (j = i - 1; j >= 0; j--)
		{
			if (array[j] < temp)
			{
				array[j + 1] = array[j];
				for (int l = 0; l < size2; l++)
				{
					matrix[j + 1][l] = matrix[j][l];
				}
			}
			else break;
		}
		array[j + 1] = temp;
		for (int k = 0; k < size2; k++)
		{
			matrix[j + 1][k] = temparray[k];
		}
		
		delete[] temparray;
	}
}


int main()
{
	setlocale(0, "");
	srand(time(NULL));

	int N{ 1000 }, start, end, maxcheck{ 0 };

	while (N > 999 || N < 1) 
	{
		cout << "Введите число кол-во купленных товаров (> 0 и < 1000 т.):  ";
		cin >> N;
	}
	cout << "\nВведите минимальную и максимальную по цене покупку.\n\nМинимальная:  ";
	cin >> start;
	cout << "\nМаксимальная:  ";
	cin >> end;

	int* basket = new int[N];
	for (int i = 0; i < N; i++)
	{
		basket[i] = start + rand() % (end - start + 1);
	}

	SortArrayInsert(basket, N);

	for (int i = 0; i < N / 2; i += 2)
	{
		int temp{ basket[i] };
		basket[i] = basket[N - N % 2 - 1 - i];
		basket[N - N % 2 - 1 - i] = temp;
	}

	for (int i = 0; i < N; i += 2)
	{
		maxcheck += basket[i];
	}

	cout << "\n\n    Изменённый чек:     I т.\tII т.\n\n\t\t\t";
	for (int i = 1; i <= N; i++)
	{
		cout << basket[i - 1] << "\t";
		if (i % 2 == 0) cout << "\n\t\t\t";
	}

	cout << "\n\n    Максимальный чек:   " << maxcheck << "\n\n\n\n\n\n";

	delete[] basket;


	

	int size1{ 0 }, size2{ 0 };

	while (size1 < 1)
	{
		cout << "\nВведите размер массива по вертикали:  ";
		cin >> size1;
	}
	while (size2 < 1)
	{
		cout << "\nВведите размер массива во горизонтали:  ";
		cin >> size2;
	}

	cout << "\nВведите начало и конец числового промежутка для заполнения массива.\n\nНачало:  ";
	cin >> start;
	cout << "\nКонец:  ";
	cin >> end;

	cout << "\n\nИзначальный двумерный массив:\n\n";
	int** matrix = new int* [size1];
	for (int i = 0; i < size1; i++)
	{
		cout << "\t\t\t\t\t\t";
		matrix[i] = new int[size2];
		for (int j = 0; j < size2; j++)
		{
			matrix[i][j] = start + rand() % (end - start + 1);
			cout  << matrix[i][j] << "  ";
		}
		cout << endl;
	}

	int* sums = new int[size1];

	for (int i = 0; i < size1; i++)
	{
		sums[i] = 0;
		for (int j = 0; j < size2; j++)
		{
			sums[i] += matrix[i][j];
		}
	}

	SortArrayInsert(matrix, sums, size1, size2);


	cout << "\n\nОтсортированный по убыванию сверху вниз массив:\n\n";
	for (int i = 0; i < size1; i++)
	{
		cout << "\t\t\t\t\t\t";
		for (int j = 0; j < size2; j++)
		{
			cout << matrix[i][j] << "  ";
		}
		cout << "\tСумма:  " << sums[i] << endl;
	}


	for (int i = 0; i < size1; i++)
	{
		delete[] matrix[i];
	}
	delete[] matrix;
	delete[] sums;

	return 0;
}
