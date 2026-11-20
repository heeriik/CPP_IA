# Capítulo 7: Uso de C++ en Procesamiento del Lenguaje Natural (NLP)

## 7.1 Fundamentos y Por Qué Usar Modern C++ en NLP

El lenguaje humano es ambiguo, dependiente del contexto y semánticamente complejo. En producción, los sistemas de NLP procesan millones de documentos, miles de millones de tokens y matrices de *embeddings* de varios gigabytes. Modern C++ aporta gestión determinista de memoria, estructuras amigables con la caché y paralelismo real sin bloqueo del GIL.

## 7.2 Técnicas Base de NLP en Modern C++

### Tokenización Básica

```cpp
#include <string>
#include <vector>
#include <sstream>

std::vector<std::string> tokenize(const std::string& text) {
    std::vector<std::string> tokens;
    std::istringstream stream(text);
    std::string word;
    while (stream >> word) {
        tokens.push_back(word);
    }
    return tokens;
}
```

### Similitud Coseno para Embeddings

```cpp
#include <vector>
#include <cmath>
#include <numeric>

double cosine_similarity(const std::vector<float>& a, const std::vector<float>& b) {
    float dot = std::inner_product(a.begin(), a.end(), b.begin(), 0.0f);
    float norm_a = std::inner_product(a.begin(), a.end(), a.begin(), 0.0f);
    float norm_b = std::inner_product(b.begin(), b.end(), b.begin(), 0.0f);
    return dot / (std::sqrt(norm_a) * std::sqrt(norm_b));
}
```

## 7.3 FastText y Álgebra Lineal con Eigen

* **FastText:** Biblioteca nativa en C++ para *word embeddings* con subpalabras y clasificación de texto de ultra baja latencia.
* **Eigen:** Biblioteca de plantillas de alto rendimiento para operaciones matriciales densas, PCA y cálculo de atención en Transformers:

```cpp
#include <Eigen/Dense>
#include <iostream>

int main() {
    Eigen::VectorXd v1(3), v2(3);
    v1 << 1.0, 2.0, 3.0;
    v2 << 4.0, 5.0, 6.0;
    double similarity = v1.dot(v2) / (v1.norm() * v2.norm());
    std::cout << similarity << "\n";
}
```

## 7.4 Despliegue de Modelos Transformer en C++

Los modelos basados en Transformers (BERT, variantes GPT) se entrenan habitualmente en Python y se despliegan en C++ mediante **TorchScript** u **ONNX Runtime** para eliminar la sobrecarga del intérprete, reducir la latencia y controlar la memoria en capas de atención con complejidad $O(n^2)$.