# Capítulo 1: Introducción a la Inteligencia Artificial y el Rol de C++

## 1.1 Definición de Inteligencia Artificial y sus Dominios Principales

La Inteligencia Artificial (IA) es la disciplina encargada de construir sistemas capaces de realizar tareas que tradicionalmente requieren inteligencia humana: percepción, razonamiento, aprendizaje a partir de datos, toma de decisiones bajo incertidumbre y reconocimiento de patrones.

Los dominios principales de la IA incluyen:

1. **Machine Learning (ML):** Algoritmos que mejoran su rendimiento mediante la exposición a datos (aprendizaje supervisado, no supervisado, semisupervisado y por refuerzo).
2. **Deep Learning (DL):** Subcampo basado en redes neuronales multicapa dominado por multiplicaciones de matrices, operaciones tensoriales y paralelismo masivo.
3. **Procesamiento del Lenguaje Natural (NLP):** Interpretación, análisis y generación de lenguaje humano mediante arquitecturas Transformer y tokenización de alto rendimiento.
4. **Visión por Computador:** Detección de objetos, segmentación, seguimiento e imágenes médicas en tiempo real.
5. **Robótica y Sistemas Autónomos:** Fusión de sensores, localización, mapeo y planificación de movimiento con garantías deterministas.
6. **Sistemas de Planificación y Decisión:** Evaluación de secuencias de acciones bajo restricciones e incertidumbre.

## 1.2 Por qué la IA es Fundamentalmente una Disciplina de Alto Rendimiento

El rendimiento en IA no es solo cuestión de velocidad; afecta directamente a:
* **Precisión del modelo** (tamaño viable y precisión numérica).
* **Latencia del sistema** (crítica en tiempo real).
* **Consumo energético** (dispositivos *edge* y embebidos).
* **Coste de despliegue** (escalado en infraestructura cloud).

Estas cargas estresan las jerarquías de caché de la CPU, el ancho de banda de memoria, las unidades vectoriales SIMD y la ejecución paralela en GPU.

## 1.3 El Rol Histórico de C++ en Sistemas de IA de Alto Rendimiento

C++ proporciona acceso directo al hardware, integración con bibliotecas numéricas optimizadas, control granular de memoria y abstracciones de costo cero (*zero-overhead abstractions*). Por ello constituye el núcleo de motores de tensores, diferenciación automática, kernels GPU, motores de IA en videojuegos y runtimes embebidos.

## 1.4 Comparativa: C++, Python y Java en Ingeniería de IA

| Criterio | Modern C++ | Python | Java |
| :--- | :--- | :--- | :--- |
| **Rendimiento** | Velocidad nativa cercana al metal; vectorización e inlining agresivo. | Depende de extensiones nativas (C/C++) para el cálculo pesado. | Portabilidad sólida, pero con sobrecarga de JVM y menor control de bajo nivel. |
| **Gestión de Memoria** | Control explícito, alineación, RAII y asignadores personalizados. | Recolección de basura (GC); posibles picos de latencia. | Recolección de basura (GC); menos predecible en cargas intensivas de tensores. |
| **Rol en el Ecosistema** | Infraestructura, runtimes, inferencia de baja latencia, sistemas embebidos. | Prototipado rápido, investigación y flujos educativos. | Entornos empresariales y plataformas distribuidas. |