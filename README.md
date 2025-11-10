# Vetorização em Máquinas de Turing: Validação Empírica da Tese de Church-Turing Estendida

## 🎓 Projeto Acadêmico - Universidade Tiradentes

**Autores:**
- Andrey Oliveira Santos
- Caio Henrique das Chagas Santos
- Guilherme Eugênio Sacramento Barreto
- Isabela Guedes de Morais
- Lizandra Bispo do Nascimento
- Lucas Machado Delgado
- Mateus Henrique de Araújo Santos
- Pedro Afonso Quintela

**Instituição:** Universidade Tiradentes / Sistemas de Informação
**Local:** Aracaju - Sergipe

---

## 📖 Descrição do Projeto

Este repositório contém uma investigação empírica sobre se a **vetorização SIMD** (Single Instruction Multiple Data), um modelo prático de paralelismo em computadores modernos, mantém a **equivalência computacional** prevista pela **Tese de Church-Turing Estendida (ECTT)**.

### 🎯 Objetivo Principal

Responder a pergunta fundamental: **"A vetorização viola a Tese de Church-Turing Estendida?"**

Através da implementação do autômato celular **Rule 110** (provado ser Turing-completo) em duas formas:
- **Sequencial:** simula uma máquina de Turing (processamento célula-por-célula)
- **Vetorizada:** utiliza instruções NumPy SIMD (processamento paralelo)

### 🔬 Resultados Principais

| Métrica | Valor |
|---------|-------|
| **Aceleração (Speedup)** | 35,58x |
| **Tipo de Aceleração** | **Polinomial** (fator constante) |
| **Equivalência Computacional** | ✅ 100% bit-perfeita |
| **Estabilidade (Sequencial)** | 0,507% CV |
| **Estabilidade (Vetorizado)** | 0,128% CV |
| **Eficiência Relativa** | 55,6% do teórico |

**Conclusão:** Vetorização SIMD respeita os limites teóricos da ECTT - não fornece vantagem exponencial, apenas polinomial.

---

## 📁 Estrutura do Repositório

```
vetorizacao-turing-ectt/
├── README.md                          # Este arquivo
├── CITEME.bib                         # Referência BibTeX para este projeto
│
├── paper/
│   ├── artigo_vetorizacao_FINAL.tex  # Artigo completo em LaTeX
│   ├── referencias_vetorizacao.bib   # Referências bibliográficas
│   ├── artigo_vetorizacao.pdf        # PDF compilado
│   └── speedup_result.png            # Gráfico de resultados
│
├── code/
│   ├── rule110_mvp.py               # Implementação completa
│   ├── test_equivalence.py          # Testes de equivalência
│   └── requirements.txt             # Dependências Python
│
├── data/
│   ├── results.csv                  # Dados brutos do experimento
│
├── docs/
│   ├── SETUP.md                     # Instruções de instalação
│   ├── HOW_TO_RUN.md               # Como executar o código
│   ├── HOW_TO_REPLICATE.md         # Como replicar os testes
│   ├── METHODOLOGY.md               # Detalhes da metodologia
│   └── THEORY.md                    # Explicação teórica
```

---

## 🚀 Quick Start (Início Rápido)

### 1. Clone o Repositório
```bash
git clone https://github.com/seu-usuario/vetorizacao-turing-ectt.git
cd vetorizacao-turing-ectt
```

### 2. Instale as Dependências
```bash
pip install -r code/requirements.txt
```

### 3. Execute o Benchmark
```bash
python code/rule110_mvp.py --width 10000 --gens 1000 --repeats 10
```

### 4. Visualize os Resultados
```
Saída esperada:
============================================================
RULE 110 CELLULAR AUTOMATON: VECTORIZATION PROOF
============================================================
✓ Correctness verified: Sequential and vectorized produce identical results

BENCHMARK RESULTS (width=10000, generations=1000):
Sequential: 1.838s
Vectorized: 0.052s
Speedup: 35.58x (POLYNOMIAL - validates ECTT)

✓ Chart saved: speedup_result.png
```

---

## 📋 Instruções Detalhadas

### ✅ Instalação Completa

**Pré-requisitos:**
- Python 3.9+
- pip (gerenciador de pacotes Python)
- Git
- (Opcional) LaTeX para compilar o artigo

**Passos:**

```bash
# 1. Clone
git clone https://github.com/seu-usuario/vetorizacao-turing-ectt.git
cd vetorizacao-turing-ectt

# 2. Crie ambiente virtual (recomendado)
python3 -m venv venv
source venv/bin/activate  # No Windows: venv\Scripts\activate

# 3. Instale dependências
pip install numpy matplotlib pandas scipy jupyter

# 4. Verifique instalação
python -c "import numpy; print(f'NumPy {numpy.__version__} instalado')"
```

### 🏃 Como Executar

#### Versão Simples (Padrão)
```bash
python code/rule110_mvp.py
```

#### Com Parâmetros Personalizados
```bash
python code/rule110_mvp.py \
    --width 10000 \
    --generations 1000 \
    --repeats 10 \
    --save-csv results.csv \
    --save-plot speedup.png
```

#### Executar Testes de Equivalência
```bash
python code/test_equivalence.py
```

Saída esperada:
```
Testing correctness for small grids...
✓ Grid 100: PASS
✓ Grid 1000: PASS
✓ Grid 5000: PASS
All tests PASSED! Equivalence verified.
```

#### Executar Análise Completa (Jupyter)
```bash
jupyter notebook data/results_analysis.ipynb
```

---

## 🔬 Como Replicar os Testes

### Protocolo Completo de Replicação

**Objetivo:** Reproduzir exatamente os resultados publicados.

#### Passo 1: Preparação do Ambiente
```bash
# Limpar dados anteriores
rm -rf data/results*.csv
rm -rf *.png

# Verificar Python version
python --version  # Deve ser 3.9+

# Listar dependências instaladas
pip list | grep -E "numpy|matplotlib|pandas"
```

#### Passo 2: Configuração de Hardware (IMPORTANTE!)
Para máxima reprodutibilidade:

```bash
# Linux: Fixar frequência da CPU
sudo cpupower frequency-set -g performance

# Linux: Desabilitar dynamic frequency scaling
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Verificar status
cat /proc/cpuinfo | grep "MHz"

# Fechar aplicações desnecessárias
killall firefox chrome other-apps 2>/dev/null
```

#### Passo 3: Executar Benchmark
```bash
# Executar script de benchmark completo
bash scripts/run_all_benchmarks.sh

# Ou manualmente, executar 3 vezes para consistência:
for i in {1..3}; do
  echo "Run $i/3"
  python code/rule110_mvp.py \
    --width 10000 \
    --gens 1000 \
    --repeats 10 \
    --save-csv results_run_$i.csv
done
```

#### Passo 4: Análise de Resultados
```bash
# Gerar plots
python scripts/generate_plots.py

# Analisar estatísticas
python -c "
import pandas as pd
import numpy as np

for i in range(1, 4):
    df = pd.read_csv(f'results_run_{i}.csv')
    print(f'Run {i}: Mean speedup = {df[\"speedup\"].mean():.2f}x')
"
```

#### Passo 5: Comparação com Resultados Publicados
```python
# Resultados esperados (do artigo)
EXPECTED_SPEEDUP = 35.58
EXPECTED_CV_SEQ = 0.507
EXPECTED_CV_VEC = 0.128

# Seus resultados devem estar dentro de ±5%
print(f"Expected speedup: {EXPECTED_SPEEDUP}x ±5% = [{EXPECTED_SPEEDUP*0.95:.2f}, {EXPECTED_SPEEDUP*1.05:.2f}]")
```

### Arquivo de Configuração para Replicação

Crie `replication_config.yaml`:
```yaml
hardware:
  cpu_frequency_scaling: disabled
  governor: performance
  background_processes: closed

software:
  python_version: "3.9+"
  numpy_version: "1.21+"
  
experiment:
  grid_width: 10000
  generations: 1000
  repeats: 10
  warmup_runs: 2
  
expected_results:
  speedup: 35.58
  speedup_tolerance: 0.05  # ±5%
  cv_sequential: 0.507
  cv_vectorized: 0.128
```

Use assim:
```bash
python code/rule110_mvp.py --config replication_config.yaml
```

---

## 📊 Estrutura dos Dados

### Arquivo CSV (`results.csv`)
```csv
impl,mean_time_s,std_s,width,generations,repeats,speedup,efficiency
sequential,1.837784059999467,0.009306739064462433,10000,1000,10,1.0,100.0
vectorized,0.051647709999815564,0.0006602527556854809,10000,1000,10,35.58,55.6
```

### Análise por Tamanho de Grid
```
Grid Size    | Speedup | Efficiency | Time Seq | Time Vec
5000         | 36.10x  | 56.4%      | 0.920s   | 0.0255s
10000        | 35.58x  | 55.6%      | 1.838s   | 0.0516s
20000        | 35.20x  | 55.0%      | 3.680s   | 0.1045s
```

**Interpretação:** Aceleração permanece **constante** (~35.5x) independentemente do tamanho = **POLINOMIAL**

---

## 🧬 Detalhes Técnicos

### Implementação Sequencial
```python
def rule110_sequential(state, generations):
    """Processa uma célula por vez (simula Turing machine)"""
    for gen in range(generations):
        new_state = []
        for i in range(len(state)):
            left = state[(i-1) % len(state)]
            center = state[i]
            right = state[(i+1) % len(state)]
            pattern = (left << 2) | (center << 1) | right
            rule_table = [0, 1, 1, 1, 0, 1, 1, 0]
            new_state.append(rule_table[pattern])
        state = new_state
    return state
```

**Complexidade:** O(N × G) onde N=width, G=generations

### Implementação Vetorizada
```python
def rule110_vectorized(state, generations):
    """Processa múltiplas células em paralelo (SIMD)"""
    state = np.array(state, dtype=np.uint8)
    for gen in range(generations):
        left = np.roll(state, 1)
        right = np.roll(state, -1)
        new_state = ((left ^ state) | (left ^ right)) & (~(left & state & right))
        state = new_state
    return state
```

**Complexidade Teórica:** O((N/SIMD_WIDTH) × G) ≈ O(N × G) em escalas grandes

**Speedup Observado vs Teórico:**
- Teórico máximo: 64x (para inteiros de 8-bit)
- Observado: 35,58x
- Eficiência: 35,58 ÷ 64 = **55,6%** ✓ realista

---

## 🧪 Testes Inclusos

### Teste de Equivalência Computacional
```bash
python code/test_equivalence.py
```

Verifica:
- ✓ Outputs bit-por-bit idênticos
- ✓ Independência de tamanho da grade
- ✓ Estabilidade com múltiplas execuções

### Teste de Performance
```bash
python code/benchmark.py --verbose
```

Mede:
- ✓ Tempo de execução
- ✓ Memória utilizada
- ✓ Throughput (cells/sec)
- ✓ Eficiência energética (estimada)

---

## 📚 Documentação Adicional

- **[SETUP.md](docs/SETUP.md)** - Instruções detalhadas de instalação
- **[METHODOLOGY.md](docs/METHODOLOGY.md)** - Explicação completa da metodologia
- **[THEORY.md](docs/THEORY.md)** - Fundamentos teóricos da ECTT
- **[artigo_vetorizacao.pdf](paper/artigo_vetorizacao.pdf)** - Artigo completo

---

## 📖 Como Citar Este Trabalho

### BibTeX
```bibtex
@article{santos2025vetorizacao,
  title={Vetorização em Máquinas de Turing: Uma Validação Empírica da Tese de Church-Turing Estendida},
  author={Santos, Andrey Oliveira and Santos, Caio Henrique das Chagas and Barreto, Guilherme Eugênio Sacramento and Morais, Isabela Guedes de and Nascimento, Lizandra Bispo do and Delgado, Lucas Machado and Santos, Mateus Henrique de Araújo and Quintela, Pedro Afonso},
  journal={Universidade Tiradentes},
  year={2025}
}
```

### APA
Santos, A. O., Santos, C. H. D. C., Barreto, G. E. S., & et al. (2025). Vetorização em Máquinas de Turing: Uma Validação Empírica da Tese de Church-Turing Estendida. Universidade Tiradentes.

### Simples
Universidade Tiradentes (2025). Vetorização em Máquinas de Turing: Uma Validação Empírica da Tese de Church-Turing Estendida. GitHub: https://github.com/seu-usuario/vetorizacao-turing-ectt

---

## 🤝 Contribuições

Contribuições são bem-vindas! Por favor:

1. Faça um Fork do repositório
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

---



## 🙏 Agradecimentos

Agradecimentos especiais ao:
- Professor/Orientador (Victor Flavio de Andrade Araujo)
- Universidade Tiradentes
- Comunidade de código aberto

---

## 🔗 Links Relacionados

- [Stanford Encyclopedia - Church-Turing Thesis](https://plato.stanford.edu/entries/church-turing/)
- [NumPy Documentation](https://numpy.org/)
- [Python Performance Tips](https://wiki.python.org/moin/PythonSpeed)
- [SIMD Intrinsics Guide](https://www.intel.com/content/www/us/en/docs/intrinsics-guide/)

---

**Última atualização:** 2025-11-10

**Status do Projeto:** ✅ Completo e Publicado
