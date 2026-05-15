# 🚀 Sensor Virtual de Velocidade (Radar de Visão Computacional)

Este projeto apresenta o desenvolvimento de um sistema de monitoramento de tráfego inteligente capaz de identificar veículos em uma rodovia e estimar sua velocidade escalar média em tempo real. O sistema utiliza a arquitetura **YOLOv8 (Deep Learning)** para detecção e rastreamento, integrada a uma camada de análise geométrica e cinemática baseada em **OpenCV**.

---

## 📊 Desempenho do Modelo (Benchmarking)

Para viabilizar a precisão do radar, foi realizado um processo de *Fine-Tuning* (ajuste fino) do modelo **YOLOv8 Nano** ao longo de 20 épocas. O modelo especialista superou significativamente os benchmarks oficiais da Ultralytics para o modelo generalista (treinado no COCO dataset):

| Métrica de Avaliação | YOLOv8n Padrão (Generalista) | Nosso Modelo Customizado (Especialista) | Status / Impacto no Projeto |
| :--- | :---: | :---: | :--- |
| **mAP50** (Precisão Geral) | 53.3% | **79.0%** | **+25.7%** de ganho (Detecção ultra estável) 📈 |
| **mAP50-95** (Rigor de Encaixe) | 37.3% | **48.4%** | **+11.1%** de ganho (Bounding boxes justas) 🎯 |

> **Nota de Engenharia:** Enquanto o modelo padrão divide sua capacidade computacional para reconhecer 80 classes distintas, o nosso modelo direcionou 100% de seus pesos sinápticos para a geometria de tráfego rodoviário, eliminando oscilações de IDs e garantindo a estabilidade do rastreamento.

---

## 📏 Fundamentação Teórica e Calibração

O cálculo da velocidade baseia-se nos princípios da **Cinemática Escalar**. Foram definidas duas barreiras virtuais no plano da imagem: **Ponto A (Entrada / Linha Amarela em Y=380)** e **Ponto B (Saída / Linha Vermelha em Y=474)**.

### Fórmulas Aplicadas:
1. **Delta Tempo ($\Delta t$):** Calculado com base na contagem de frames em que o objeto permaneceu na zona de monitoramento e no FPS dinâmico do vídeo.
   $$\Delta t = \frac{\text{Frames entre as linhas}}{\text{FPS do Vídeo}}$$

2. **Velocidade Escalar Média ($V$):**
   $$V = \frac{\Delta S}{\Delta t} \times 3.6 \text{ (km/h)}$$

* **Espaço Real ($\Delta S$):** Calibrado em **31.0 metros**, calculado a partir do padrão de sinalização horizontal da via.

---

## 📁 Estrutura do Projeto

```text
projeto-velocidade/
├── notebooks/
│   └── velocimetro_virtual.ipynb  # Notebook principal com a execução do projeto
├── runs/
│   └── detect/
│       └── treino_velocidade_oficialv5/
│           └── weights/
│               └── best.pt        # Pesos do modelo treinado com 79% de mAP
├── Road-traffic.mp4               # Vídeo de teste em alta resolução (amostra leve)
├── pyproject.toml                 # Configuração de dependências do Poetry
└── README.md                      # Documentação do projeto
```
## 🛠️ Como Instalar e Rodar o Projeto

Este projeto utiliza o **Poetry** para o gerenciamento de ambiente virtual e dependências, garantindo que o código rode perfeitamente em qualquer máquina sem conflitos de biblioteca.

### Pré-requisitos:
* Python 3.10 ou superior
* Poetry instalado (`pip install poetry`)

### Passo a Passo:

1. **Clone o repositório:**
   ```bash
   git clone [https://github.com/VitorFantini/projeto-velocidade.git](https://github.com/VitorFantini/projeto-velocidade.git)
   cd projeto-velocidade
   ```

   Instale as dependências e isole o ambiente:

```Bash
poetry install
Este comando lerá o arquivo pyproject.toml e criará um ambiente virtual isolado com as versões exatas do PyTorch, OpenCV e Ultralytics utilizadas no desenvolvimento.
```
Ative o ambiente virtual:

```Bash
poetry shell
Execute o Projeto:
```
Abra o VS Code:

```Bash
code .
Abra o arquivo notebooks/velocimetro_virtual.ipynb.
```
Escolha o Kernel do ambiente virtual do Poetry (.venv) no canto superior direito do VS Code e clique em Run All (Executar Tudo).
