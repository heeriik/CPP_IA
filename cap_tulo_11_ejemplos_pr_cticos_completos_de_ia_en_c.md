# Capítulo 11: Ejemplos Prácticos Completos de IA en C++

## 11.1 Regresión Lineal con Descenso de Gradiente

Implementación completa desde cero minimizando el Error Cuadrático Medio (MSE):

```cpp
#include <iostream>
#include <vector>
#include <cstddef>

double computeCost(const std::vector<double>& x,
                   const std::vector<double>& y,
                   double m, double b) {
    double cost = 0.0;
    const std::size_t n = x.size();
    for (std::size_t i = 0; i < n; ++i) {
        double prediction = m * x[i] + b;
        double error = prediction - y[i];
        cost += error * error;
    }
    return cost / (2.0 * n);
}

void gradientDescent(const std::vector<double>& x,
                     const std::vector<double>& y,
                     double& m, double& b,
                     double alpha, int iterations) {
    const std::size_t n = x.size();
    for (int iter = 0; iter < iterations; ++iter) {
        double dm = 0.0, db = 0.0;
        for (std::size_t i = 0; i < n; ++i) {
            double error = (m * x[i] + b) - y[i];
            dm += error * x[i];
            db += error;
        }
        m -= alpha * (dm / n);
        b -= alpha * (db / n);
    }
}

int main() {
    std::vector<double> x = {1, 2, 3, 4, 5};
    std::vector<double> y = {2, 4, 6, 8, 10};
    double m = 0.0, b = 0.0;
    gradientDescent(x, y, m, b, 0.01, 1000);
    std::cout << "Slope (m): " << m << " | Intercept (b): " << b << "\n";
}
```

## 11.2 Red Neuronal Feedforward (Backpropagation)

Implementación mínima con activación sigmoide $\sigma(x) = \frac{1}{1 + e^{-x}}$, derivada $\sigma'(y) = y(1 - y)$ y actualización de pesos mediante retropropagación del gradiente.

## 11.3 Q-Learning Tabular en Gridworld ($5 \times 5$)

Un agente aprende a navegar desde `(0,0)` hasta `(4,4)` maximizando la recompensa acumulada ($+10$ en meta, $-10$ en obstáculos) mediante exploración $\epsilon$-greedy y la regla de actualización:

$$Q(s,a) \leftarrow Q(s,a) + \alpha\left(r + \gamma \max_{a'} Q(s',a') - Q(s,a)\right)$$

## 11.4 Propagación Hacia Adelante Concurrente con `std::thread`

Cada neurona de una capa densa es independiente, lo que permite particionar el cálculo entre múltiples hilos de ejecución:

```cpp
#include <iostream>
#include <vector>
#include <thread>
#include <mutex>

std::mutex io_mutex;

double relu(double x) { return x > 0 ? x : 0; }

void compute(const std::vector<double>& in,
             const std::vector<std::vector<double>>& w,
             const std::vector<double>& b,
             std::vector<double>& out,
             int start, int end) {
    for (int i = start; i < end; ++i) {
        double sum = b[i];
        for (std::size_t j = 0; j < in.size(); ++j) {
            sum += in[j] * w[i][j];
        }
        out[i] = relu(sum);
        std::lock_guard<std::mutex> lock(io_mutex);
        std::cout << "Neuron " << i << " -> " << out[i] << "\n";
    }
}
```