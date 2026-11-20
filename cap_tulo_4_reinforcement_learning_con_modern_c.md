# Capítulo 4: Reinforcement Learning con Modern C++

## 4.1 Conceptos Fundamentales

En el Aprendizaje por Refuerzo (RL), un agente aprende a interactuar con un entorno modelado como un **Proceso de Decisión de Markov (MDP)** definido por estados ($s$), acciones ($a$), recompensas ($r$), política ($\pi$) y factor de descuento ($\gamma$):

$$\max_{\pi} \mathbb{E}_{\pi}\left[\sum_{t=0}^{\infty} \gamma^t R(s_t, a_t)\right]$$

## 4.2 Algoritmos Principales

* **Q-Learning:** Actualización libre de modelo mediante la ecuación de Bellman:
  $$Q(s,a) \leftarrow Q(s,a) + \alpha\left(r + \gamma \max_{a'} Q(s',a') - Q(s,a)\right)$$
* **Deep Q-Network (DQN):** Aproxima la función $Q(s,a)$ con redes neuronales.
* **Policy Gradient y Actor-Critic:** Optimizan directamente la política o combinan estimación de valor y política.

## 4.3 Implementación de Q-Learning en Modern C++

```cpp
#include <vector>
#include <algorithm>

class QTable {
public:
    QTable(size_t states, size_t actions)
        : table_(states, std::vector<double>(actions, 0.0)) {}

    double& operator()(size_t s, size_t a) { return table_[s][a]; }

    double max_action_value(size_t s) const {
        return *std::max_element(table_[s].begin(), table_[s].end());
    }

private:
    std::vector<std::vector<double>> table_;
};

void update(QTable& Q, size_t state, size_t action,
            double reward, size_t next_state,
            double alpha, double gamma) {
    double best_next = Q.max_action_value(next_state);
    Q(state, action) += alpha * (reward + gamma * best_next - Q(state, action));
}
```

## 4.4 Simulación Paralela y Buffers de Experiencia

* **Ejecución paralela de episodios:** `std::for_each(std::execution::par, episodes.begin(), episodes.end(), ...)` acelera el entrenamiento en múltiples núcleos.
* **Replay Buffer preasignado:** Usar `replay_buffer.reserve(100000)` evita reasignaciones dinámicas y fragmentación durante la recolección de experiencias.