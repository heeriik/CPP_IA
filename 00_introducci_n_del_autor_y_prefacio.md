# Introducción del Autor y Prefacio

## Introducción del Autor

La Inteligencia Artificial ya no es un nicho especializado reservado para laboratorios de investigación. Se ha convertido en una capa fundamental del software moderno: integrada en búsqueda, visión, voz, recomendaciones, seguridad, finanzas, medicina, automatización industrial y, hoy más visiblemente que nunca, en herramientas de desarrollo y modelos grandes de lenguaje (LLMs). Para los ingenieros de software, la pregunta ya no es si la IA importa, sino cómo construir sistemas habilitados por IA que sean correctos, rápidos, seguros y mantenibles en entornos de producción reales.

Durante muchos años, la educación en IA y la experimentación diaria han estado fuertemente asociadas con Python. Sin embargo, en el momento en que una idea de IA debe convertirse en un producto —ejecutándose a escala, bajo presupuestos estrictos de latencia, dentro de dispositivos limitados, a través de GPUs/NPUs y con altos requisitos de fiabilidad— **C++ aparece en todas partes**.

Esta segunda edición está escrita para el desarrollador de C++ que desea participar en la IA sin abandonar su identidad de ingeniería, enfocándose en dos vías paralelas:

* **Fundamentos de IA para ingenieros de C++:** Qué produce el entrenamiento, qué hace realmente la inferencia, por qué los modelos se comportan de manera probabilística y cómo se mueven los cuellos de botella entre CPU, memoria, caché, GPU e I/O.
* **Técnicas de Modern C++ que desbloquean rendimiento y robustez:** Seguridad de recursos impulsada por RAII, control determinista del ciclo de vida, abstracciones de costo cero, diseños de datos amigables con la caché, asignadores personalizados (`std::pmr`), concurrencia sin bloqueos (*lock-free*), vectorización SIMD y límites arquitectónicos limpios.

---

## Prefacio

La Inteligencia Artificial ha entrado en una nueva fase: el centro de gravedad se ha desplazado de la experimentación al despliegue en producción. Las capas fundamentales de la mayoría de los frameworks de IA en producción —kernels de entrenamiento, motores de tensores, runtimes de inferencia, optimizadores de grafos y asignadores de memoria— están implementados en **C y C++**.

Modern C++ ($C++20$, $C++23$ y posteriores) ofrece herramientas que se alinean notablemente bien con las cargas de trabajo de IA: sistemas de tipos fuertes, RAII, programación genérica, algoritmos paralelos (`std::execution`), vistas multidimensionales estandarizadas (`std::mdspan`) y recursos de memoria polimórficos.