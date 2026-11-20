# Capítulo 3: Deep Learning con Modern C++

## 3.1 Qué es el Deep Learning y su Rol en la IA

El Deep Learning utiliza redes neuronales artificiales multicapa que aprenden representaciones jerárquicas automáticamente a partir de datos sin procesar:

$$x \xrightarrow{L_1} \xrightarrow{L_2} \dots \xrightarrow{L_n} y$$

Desde el punto de vista de ingeniería, está dominado por operaciones tensoriales, multiplicación de matrices, convoluciones, mecanismos de atención y optimización basada en gradientes.

## 3.2 Bibliotecas de C++ para Deep Learning

### PyTorch C++ API (LibTorch)

Permite construir, entrenar y desplegar redes neuronales directamente en C++ con diferenciación automática y soporte CUDA:

```cpp
#include <torch/torch.h>

struct Net : torch::nn::Module {
    Net() : fc1(10, 64), fc2(64, 32), fc3(32, 1) {
        register_module("fc1", fc1);
        register_module("fc2", fc2);
        register_module("fc3", fc3);
    }

    torch::Tensor forward(torch::Tensor x) {
        x = torch::relu(fc1(x));
        x = torch::relu(fc2(x));
        x = fc3(x);
        return x;
    }

    torch::nn::Linear fc1, fc2, fc3;
};
```

## 3.3 Construcción de una Capa Densa en Modern C++

```cpp
#include <vector>
#include <numeric>
#include <algorithm>

double relu(double x) {
    return std::max(0.0, x);
}

double dense_layer(const std::vector<double>& input,
                   const std::vector<double>& weights,
                   double bias) {
    double sum = std::inner_product(input.begin(), input.end(), weights.begin(), 0.0);
    return relu(sum + bias);
}
```

## 3.4 Caso de Estudio: Sistema de Percepción para Conducción Autónoma

Un pipeline de percepción exige latencia de milisegundos y fusión multisensor (cámara, LiDAR, radar):

```cpp
#include <torch/script.h>

torch::jit::script::Module module = torch::jit::load("model.pt");
std::vector<torch::jit::IValue> inputs;
inputs.push_back(input_tensor);
at::Tensor output = module.forward(inputs).toTensor();
```

En producción, se combina con **NVIDIA TensorRT** para compilar grafos de inferencia y orquestar streams CUDA de forma asíncrona desde C++.