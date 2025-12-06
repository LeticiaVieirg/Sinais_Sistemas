# 📡 Repositório de Sinais e Sistemas

## Sobre o Repositório

Este repositório contém as implementações computacionais desenvolvidas para a disciplina de **Sinais e Sistemas**, com foco na análise, simulação e processamento de sistemas lineares invariantes no tempo (LTI). Os códigos foram desenvolvidos em Python e abordam desde sistemas de segunda ordem até técnicas avançadas de modulação e multiplexação.

## Estrutura do Projeto

```
Sinais_Sistemas/
│
├── 📁 src/
│        ├── introdução
│        ├── 1_Trabalho_Computacional
│        └── 2_Trabalho_Computacional  
│   ├── LICENSE
│   └── README.md
│
├── requirements.txt
└── README.md
```

## Requisitos e Instalação

### Dependências Necessárias

```bash
# Instale as dependências
pip install numpy matplotlib scipy control

# Ou usando requirements.txt
pip install -r requirements.txt
```

### Arquivo requirements.txt
```txt
numpy>=1.21.0
matplotlib>=3.5.0
scipy>=1.7.0
control>=0.9.0
jupyter>=1.0.0
```

## 🔬 Principais Tópicos Abordados

### 1. **Sistemas de Segunda Ordem**

Análise de sistemas subamortecidos com cálculo de parâmetros temporais:

- Tempo de subida (rise time)
- Tempo de pico (peak time)
- Sobressinal (overshoot)
- Tempo de acomodação (settling time)

### 2. **Diagramas de Polos e Zeros**

Análise da estabilidade de sistemas através da localização dos polos no plano complexo.

### 3. **Resposta em Frequência**

Classificação de sistemas como filtros (passa-baixas, passa-altas, passa-faixas, rejeita-faixas) através da análise da resposta em frequência.

### 4. **Modulação AM**

Implementação de um modulador AM usando elemento não-linear seguido de filtragem passa-faixas.

### 5. **Multiplexação FDM**

Sistema completo de multiplexação por divisão em frequência com transmissão e recepção de múltiplos sinais.

## 📊 Exemplos de Código

### Sistema de Segunda Ordem - Subamortecido

```python
import numpy as np
import matplotlib.pyplot as plt
import scipy.signal as signal

# Sistema: H(s) = 16/(s² + 4s + 16)
num = [16]
den = [1, 4, 16]

# Cálculo dos parâmetros
ωn = np.sqrt(den[2])  # ωn = 4 rad/s
ζ = den[1] / (2 * ωn) # ζ = 0.5

print(f"Sistema subamortecido: ζ = {ζ:.2f}")

# Resposta ao degrau
sys = signal.TransferFunction(num, den)
t, y = signal.step(sys)

plt.figure(figsize=(10, 6))
plt.plot(t, y)
plt.title('Resposta de Sistema Subamortecido')
plt.xlabel('Tempo (s)')
plt.ylabel('Amplitude')
plt.grid(True)
plt.show()
```

### Modulação AM com Elemento Não-Linear

```python
import numpy as np
import matplotlib.pyplot as plt

# Parâmetros
fs = 100000
t = np.linspace(0, 0.01, int(fs * 0.01))
fm = 1000   # 1 kHz
fc = 10000  # 10 kHz

# Sinais
m_t = np.cos(2 * np.pi * fm * t)  # Mensagem
c_t = np.cos(2 * np.pi * fc * t)  # Portadora

# Elemento não-linear
x_t = m_t + c_t
y_t = x_t + 0.5 * x_t**2

# Filtragem passa-faixas
from scipy import signal
nyquist = fs / 2
b, a = signal.butter(6, [(fc-2000)/nyquist, (fc+2000)/nyquist], 'band')
am_t = signal.filtfilt(b, a, y_t)

# Visualização
plt.figure(figsize=(12, 8))
plt.subplot(3, 1, 1)
plt.plot(t, m_t)
plt.title('Sinal Modulante')

plt.subplot(3, 1, 2)
plt.plot(t, c_t)
plt.title('Portadora')

plt.subplot(3, 1, 3)
plt.plot(t, am_t)
plt.title('Sinal AM Gerado')
plt.tight_layout()
plt.show()
```

## 🎓 Curiosidades Matemáticas

### 🌀 **Transformada de Fourier: O Eterno Ciclo**

A **Transformada de Fourier** nos ensina que **qualquer sinal periódico pode ser representado como uma soma de senos e cossenos**. Isso é tão poderoso que:

> "O que os olhos veem como uma onda quadrada, o Fourier vê como uma orquestra de senoides harmoniosas!"

**Curiosidade:** O matemático francês **Joseph Fourier** desenvolveu essa transformada enquanto estudava a propagação do calor, sem imaginar que seria fundamental para:
- Wi-Fi e celulares
- Imagens médicas (MRI)
- Reconhecimento de voz
- Streaming de música

```python
# Exemplo: Fourier decompondo uma onda quadrada
import numpy as np
import matplotlib.pyplot as plt

t = np.linspace(0, 1, 1000)
sinal_quadrado = np.sign(np.sin(2*np.pi*5*t))

# Série de Fourier (apenas harmônicos ímpares)
reconstrucao = np.zeros_like(t)
for n in range(1, 20, 2):
    reconstrucao += (4/(n*np.pi)) * np.sin(2*np.pi*n*5*t)

plt.figure(figsize=(10, 6))
plt.plot(t, sinal_quadrado, 'b--', label='Onda Quadrada', alpha=0.5)
plt.plot(t, reconstrucao, 'r-', label='Reconstrução Fourier (19 harmônicos)')
plt.title('Decomposição de Fourier: Onda Quadrada')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

### 🎭 **Transformada de Laplace: O Mágico dos Sistemas**

**Pierre-Simon Laplace** criou uma transformada que transforma **equações diferenciais complexas** em **equações algébricas simples**!

**Piada de Engenheiro:**  
Por que o Laplace foi à festa?  
Para transformar os problemas diferenciais em equações algébricas que todos pudessem resolver!

```python
# Antes do Laplace (assustador):
# d²y/dt² + 5dy/dt + 6y = -dx/dt 😱

# Depois do Laplace (fácil):
# (s² + 5s + 6)Y(s) = -sX(s) 😊
# H(s) = -s/(s² + 5s + 6)
```

**Fato Curioso:** A Transformada de Laplace é como um "superpoder" que:
- Transforma o tempo (t) em frequência complexa (s)
- Resolve sistemas instáveis
- É essencial para controle de foguetes e aviões

### 🔢 **Transformada Z: A Digitalização do Mundo**

Enquanto Laplace lida com **tempo contínuo**, a Transformada Z é a **versão digital** para processamento de sinais discretos.

**Analogia Engraçada:**
- Laplace: "Vamos analisar este sinal analógico suave..."
- Transformada Z: "Espere! Vou amostrar, digitalizar e processar no computador!"

```python
# Mundo Digital vs Analógico
t_continuo = np.linspace(0, 1, 1000)  # Laplace
t_discreto = np.arange(0, 1, 0.01)    # Transformada Z

# Laplace: H(s) = 1/(s + 1)
# Transformada Z: H(z) = 1/(1 - 0.99z⁻¹)
```

**Por que isso importa?**
- Seu celular usa Transformada Z para compressão de áudio
- Redes sociais usam para processar imagens
- GPS usa para calcular sua posição

## 📈 Exemplos Visuais Interessantes

### Efeito Gibbs - O Fenômeno das Pontas

```python
import numpy as np
import matplotlib.pyplot as plt

# Demonstração do fenômeno de Gibbs
t = np.linspace(0, 1, 1000)
onda_quadrada = np.sign(np.sin(2*np.pi*5*t))

# Número diferente de harmônicos
N_harmonicos = [1, 3, 7, 15, 51]
fig, axes = plt.subplots(3, 2, figsize=(12, 10))

for i, N in enumerate(N_harmonicos):
    ax = axes[i//2, i%2]
    reconstrucao = np.zeros_like(t)
    for n in range(1, N+1, 2):
        reconstrucao += (4/(n*np.pi)) * np.sin(2*np.pi*n*5*t)
    
    ax.plot(t, reconstrucao, 'r-', linewidth=2, label=f'N={N}')
    ax.plot(t, onda_quadrada, 'b--', alpha=0.5, label='Original')
    ax.set_title(f'Reconstrução com {N} harmônicos')
    ax.legend()
    ax.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### Comparação dos Três Planos

```python
fig, axes = plt.subplots(1, 3, figsize=(15, 5))

# Plano s (Laplace)
ax1 = axes[0]
theta = np.linspace(0, 2*np.pi, 100)
ax1.plot(np.cos(theta), np.sin(theta), 'k--', alpha=0.3)
ax1.axhline(y=0, color='k', alpha=0.2)
ax1.axvline(x=0, color='k', alpha=0.2)
ax1.set_title('Plano s (Laplace)')
ax1.set_xlabel('σ (Real)')
ax1.set_ylabel('jω (Imaginário)')
ax1.grid(True, alpha=0.3)

# Plano z (Transformada Z)
ax2 = axes[1]
ax2.plot(np.cos(theta), np.sin(theta), 'k-', alpha=0.5, linewidth=2)
ax2.axhline(y=0, color='k', alpha=0.2)
ax2.axvline(x=0, color='k', alpha=0.2)
ax2.set_title('Plano z (Transformada Z)')
ax2.set_xlabel('Re(z)')
ax2.set_ylabel('Im(z)')
ax2.grid(True, alpha=0.3)
ax2.set_aspect('equal')

# Resposta em Frequência (Fourier)
ax3 = axes[2]
freq = np.linspace(-5, 5, 1000)
H_fourier = 1/(1 + 1j*freq)  # Exemplo de filtro passa-baixas
ax3.plot(freq, np.abs(H_fourier), 'b-', linewidth=2)
ax3.set_title('Resposta em Frequência (Fourier)')
ax3.set_xlabel('Frequência (ω)')
ax3.set_ylabel('|H(jω)|')
ax3.grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

## 🎯 Como Executar os Códigos

1. Acesse [colab.research.google.com](https://colab.research.google.com)
2. Crie um novo notebook
3. Copie e cole os códigos das questões

## 📚 Recursos Adicionais

### Livros Recomendados
1. **"Signals and Systems"** - Alan V. Oppenheim
2. **"Linear Systems and Signals"** - B. P. Lathi
3. **"Digital Signal Processing"** - John G. Proakis

### Ferramentas Online
- [Desmos](https://www.desmos.com/calculator) - Para visualizar funções
- [Wolfram Alpha](https://www.wolframalpha.com/) - Para cálculos simbólicos
- [GeoGebra](https://www.geogebra.org/) - Para gráficos interativos

### Canais do YouTube
- [3Blue1Brown](https://www.youtube.com/c/3blue1brown) - Para visualizações matemáticas
- [Khan Academy](https://www.youtube.com/user/khanacademy) - Para fundamentos
- [Professor Leonard](https://www.youtube.com/user/professorleonard57) - Para aulas completas


## 📄 Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.
