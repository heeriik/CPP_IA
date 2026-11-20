# Capítulo 6: C++ en Robótica e Inteligencia Artificial Embebida

La robótica y la IA embebida operan bajo restricciones estrictas: memoria limitada, capacidad de cómputo acotada, requisitos deterministas de temporización, respuesta en tiempo real e interacción directa con el hardware.

## 6.1 C++ en Sistemas Embebidos de IA

En microcontroladores, procesadores *edge* y vehículos autónomos se evita la asignación dinámica en el *heap* empleando contenedores estáticos de tamaño fijo (`std::array`) y ciclo de vida determinista en pila:

```cpp
#include <array>
#include <cstddef>

class SensorBuffer {
public:
    static constexpr std::size_t Size = 256;

    void add_sample(float value) {
        data_[index_] = value;
        index_ = (index_ + 1) % Size;
    }

    const std::array<float, Size>& data() const { return data_; }

private:
    std::array<float, Size> data_{};
    std::size_t index_{0};
};
```

## 6.2 Robótica con Modern C++

### Visión por Computador con OpenCV

```cpp
#include <opencv2/opencv.hpp>

int main() {
    cv::VideoCapture cap(0);
    cv::Mat frame, gray;
    while (cap.read(frame)) {
        cv::cvtColor(frame, gray, cv::COLOR_BGR2GRAY);
        cv::imshow("Gray", gray);
        if (cv::waitKey(1) == 27) break;
    }
}
```

### Middleware Robótico con ROS2 (`rclcpp`)

ROS2 utiliza C++ como lenguaje de primera clase para paso de mensajes de alto rendimiento y lazos de control deterministas:

```cpp
#include "rclcpp/rclcpp.hpp"

class RobotNode : public rclcpp::Node {
public:
    RobotNode() : Node("robot_node") {
        timer_ = create_wall_timer(
            std::chrono::milliseconds(100),
            [this]() { RCLCPP_INFO(get_logger(), "Running"); }
        );
    }

private:
    rclcpp::TimerBase::SharedPtr timer_;
};
```

## 6.3 Vehículos Autónomos e IoT

* **Pipeline de conducción autónoma:** Adquisición de sensores (cámara, LiDAR, radar) $\rightarrow$ Preprocesamiento $\rightarrow$ Detección de objetos $\rightarrow$ Seguimiento temporal $\rightarrow$ Planificación de trayectoria $\rightarrow$ Control de actuadores.
* **Dispositivos IoT:** Serialización binaria eficiente y comunicación ligera (como clientes MQTT en C++) con huella mínima de memoria.

## 6.4 Procesamiento en Tiempo Real y Despliegue de Modelos

* **Operaciones atómicas (`std::atomic`):** Actualizaciones seguras entre hilos sin bloqueos de mutex:

```cpp
#include <atomic>

std::atomic<float> sensor_value{0.0f};

void update_sensor(float new_value) {
    sensor_value.store(new_value, std::memory_order_relaxed);
}
```

* **Inferencia en el borde con TensorFlow Lite C++:** Ejecución directa de modelos `.tflite` sin dependencias de Python ni sobrecarga de recolección de basura.