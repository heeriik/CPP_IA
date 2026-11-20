# Capítulo 2: Machine Learning con Modern C++

## 2.1 Fundamentos de Machine Learning

El aprendizaje automático busca aproximar una función $f(X) \rightarrow Y$, donde $X$ representa las características de entrada e $Y$ las salidas predichas.

### 1. Aprendizaje Supervisado (Producto Escalar Paralelo)

```cpp
#include <vector>
#include <numeric>
#include <execution>

double dot_product(const std::vector<double>& a, const std::vector<double>& b) {
    return std::transform_reduce(
        std::execution::par_unseq,
        a.begin(), a.end(),
        b.begin(),
        0.0
    );
}

double predict(const std::vector<double>& weights,
               const std::vector<double>& features,
               double bias) {
    return dot_product(weights, features) + bias;
}
```

### 2. Aprendizaje No Supervisado (Distancia Euclidiana)

```cpp
#include <vector>
#include <cmath>
#include <numeric>

struct Point {
    std::vector<double> values;
};

double distance(const Point& a, const Point& b) {
    return std::sqrt(std::transform_reduce(
        a.values.begin(), a.values.end(),
        b.values.begin(),
        0.0,
        std::plus<>(),
        [](double x, double y) { return (x - y) * (x - y); }
    ));
}
```

### 3. Pipelines de Preprocesamiento con `std::ranges`

```cpp
#include <ranges>

auto processed = raw_data
    | std::views::filter([](const auto& x) { return valid(x); })
    | std::views::transform([](const auto& x) { return normalize(x); });
```

## 2.2 Características de Modern C++ para Sistemas de IA

* **RAII para gestión determinista:** Uso de `std::unique_ptr<float[]>` para liberar buffers de tensores automáticamente sin fugas de memoria.
* **Vistas multidimensionales (`std::mdspan`, $C++23$):** Permite indexar tensores 2D/ND (`matrix(10, 5) = 3.14f;`) separando el almacenamiento subyacente de la disposición lógica en memoria.
* **Ejecución paralela (`std::execution::par_unseq`):** Permite vectorización SIMD y uso multinúcleo en funciones de activación como ReLU.
* **Control de recursos de memoria (`std::pmr`):** Uso de `std::pmr::monotonic_buffer_resource` para evitar fragmentación y acelerar asignaciones repetidas.

## 2.3 Bibliotecas de Machine Learning en C++

* **TensorFlow Lite:** Inferencia en dispositivos embebidos y móviles.
* **MLPack:** Algoritmos clásicos de ML altamente optimizados.
* **dlib:** Visión por computador y clasificación.
* **ONNX Runtime C++ API:** Inferencia multiplataforma de alto rendimiento.