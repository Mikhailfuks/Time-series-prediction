#include <iostream>
#include <vector>
#include <cmath>
#include <random>

using namespace std;

// Функция для генерации случайных данных временного ряда
vector<double> generateTimeSeries(int length, double trend, double seasonality, double noise) {
    vector<double> timeSeries(length);
    random_device rd;
    mt19937 gen(rd());
    normal_distribution<> dist(0, noise);

    for (int i = 0; i < length; i++) {
        timeSeries[i] = trend * i + seasonality * sin(2 * M_PI * i / 12) + dist(gen);
    }

    return timeSeries;
}

// Функция для прогнозирования временного ряда с помощью метода скользящего среднего
vector<double> predictTimeSeries(const vector<double>& timeSeries, int windowSize) {
    vector<double> predictions;
    int n = timeSeries.size();

    for (int i = windowSize; i < n; i++) {
        double sum = 0;
        for (int j = i - windowSize; j < i; j++) {
            sum += timeSeries[j];
        }
        predictions.push_back(sum / windowSize);
    }

    return predictions;
}

int main() {
    // Параметры временного ряда
    int length = 24; // Длина временного ряда
    double trend = 0.5; // Тренд
    double seasonality = 10; // Сезонность
    double noise = 2; // Шум

    // Генерация временного ряда
    vector<double> timeSeries = generateTimeSeries(length, trend, seasonality, noise);

    // Вывод исходных данных
    cout << "Исходный временной ряд:" << endl;
    for (int i = 0; i < length; i++) {
        cout << i << ": " << timeSeries[i] << endl;
    }

    // Прогнозирование с помощью метода скользящего среднего
    int windowSize = 3; // Размер окна скользящего среднего
    vector<double> predictions = predictTimeSeries(timeSeries, windowSize);

    // Вывод прогнозов
    cout << "\nПрогнозы (метод скользящего среднего):" << endl;
    for (int i = windowSize; i < length; i++) {
        cout << i << ": " << predictions[i - windowSize] << endl;
    }

    return 0;
}
