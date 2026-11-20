# Capítulo 5: Optimización de Rendimiento y Computación Paralela en Modern C++

La optimización del rendimiento es una disciplina fundamental en ingeniería, especialmente en áreas como la Inteligencia Artificial, las simulaciones a gran escala, el renderizado en tiempo real y los sistemas financieros. En estos entornos, la velocidad de ejecución, la eficiencia de memoria y la escalabilidad son requisitos centrales.

## 5.1 Control de Memoria en Modern C++

El comportamiento de la memoria influye directamente en la localidad de caché, la estabilidad de la latencia, la fragmentación y el rendimiento global.

### RAII y Ciclo de Vida Determinista

```cpp
#include <memory>
#include <cstddef>

class Buffer {
public:
    explicit Buffer(std::size_t size)
        : data_(std::make_unique<float[]>(size)), size_(size) {}

    float* data() noexcept { return data_.get(); }
    std::size_t size() const noexcept { return size_; }

private:
    std::unique_ptr<float[]> data_;
    std::size_t size_;
};
```

### Memory Pooling con Asignadores Polimórficos (`std::pmr`)

```cpp
#include <memory_resource>
#include <vector>

std::pmr::monotonic_buffer_resource pool;
std::pmr::vector<float> data(&pool);
data.resize(100000);
```

### Diseño de Datos Orientado a Caché (SoA vs AoS)

El patrón *Structure-of-Arrays* (SoA) supera habitualmente a *Array-of-Structures* (AoS) al mejorar la vectorización SIMD y el uso del ancho de banda de memoria:

```cpp
#include <vector>

struct Particles {
    std::vector<float> x, y, z;
    std::vector<float> vx, vy, vz;
};
```

## 5.2 Computación Paralela en Modern C++

* **Multihilo explícito:** `std::thread` y `std::jthread`.
* **Tareas asíncronas:** `std::async` con `std::launch::async` y `std::future`.
* **Algoritmos paralelos ($C++17$ y posteriores):** Políticas de ejecución que habilitan distribución automática de hilos y vectorización SIMD:

```cpp
#include <algorithm>
#include <execution>
#include <vector>

void scale_vector(std::vector<double>& data) {
    std::for_each(std::execution::par_unseq, data.begin(), data.end(), [](double& v) {
        v *= 2.0;
    });
}
```

## 5.3 Aceleración GPU con CUDA

Para cargas altamente paralelas (deep learning, simulaciones, multiplicación de grandes matrices), las GPUs proporcionan aceleraciones masivas mediante kernels dedicados:

```cpp
__global__ void vector_add(float* a, float* b, float* c, int n) {
    int i = blockIdx.x * blockDim.x + threadIdx.x;
    if (i < n) {
        c[i] = a[i] + b[i];
    }
}
```

## 5.4 OpenCL para Paralelismo Multiplataforma

OpenCL permite la ejecución heterogénea independiente del fabricante entre CPUs, GPUs de distintos proveedores y aceleradores especializados.

## 5.5 Estrategias Paralelas en Algoritmos de IA

* **Particionado de datos por lotes (*Batch parallelism*):** Procesamiento concurrente de subconjuntos mediante `std::execution::par`.
* **Ordenación paralela:** `std::sort(std::execution::par, data.begin(), data.end())`.
* **Colas sin bloqueos (*Lock-free queues*):** Reducción de contención en pipelines de inferencia.