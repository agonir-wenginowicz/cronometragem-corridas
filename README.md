# Sistema de Cronometragem de Corridas com RFID

## 📌 Visão Geral

Este sistema em Python foi desenvolvido para cronometrar corredores em provas de rua utilizando tecnologia RFID UHF. Ele lê tags RFID dos chips dos atletas, associa com um banco de dados pré-cadastrado e registra os tempos de passagem em um arquivo CSV.

## 🎯 Objetivo

Criar um sistema de cronometragem acessível e confiável para eventos de corrida que:
- Registra com precisão a passagem de cada atleta nos pontos de controle
- Associa as tags RFID aos nomes dos atletas
- Armazena os horários para análise posterior
- Evita leituras duplicadas dentro de um intervalo configurável

## 🛠 Tecnologias Utilizadas

- **Python 3** (com bibliotecas padrão)
- **PySerial** - Para comunicação serial com o leitor RFID
- **Manipulação de arquivos CSV** - Para armazenamento de dados e mapeamento
- **Leitor RFID UHF** - Componente físico para detecção das tags
- **Tags RFID UHF passivas** - Chips de identificação dos atletas

## 🔧 Instruções de Configuração

### Requisitos de Hardware
1. Leitor RFID UHF conectado via porta COM
2. Tags RFID passivas para os atletas
3. Computador com Python 3 instalado

### Configuração de Software
1. Clone este repositório
2. Instale os pacotes necessários:
   ```bash
   pip install pyserial
   ```
3. Edite a seção de configuração no script:
   ```python
   serial_port = 'COM3'          # Altere para sua porta COM
   baud_rate = 115200            # Ajuste para a taxa do seu leitor
   read_interval = 5             # Segundos entre leituras duplicadas
   csv_file = 'etiquetas_passagens.csv'  # Arquivo de saída
   map_file = 'atletas.csv'      # Arquivo de mapeamento
   ```

### Banco de Dados de Atletas
Crie um arquivo CSV chamado `atletas.csv` com o formato:
```
etiqueta,nome
RFID1,João Silva
RFID2,Maria Souza
...
```

## 🚀 Como Usar
1. Conecte o leitor RFID ao computador
2. Execute o script:
   ```bash
   python codepython
   ```
3. O sistema irá:
   - Monitorar continuamente as tags RFID
   - Associar tags aos nomes dos atletas
   - Registrar passagens no arquivo CSV de saída
   - Evitar leituras duplicadas no intervalo configurado

## 📊 Saída
O sistema gera um arquivo CSV (`etiquetas_passagens.csv`) com colunas:
- `etiqueta`: ID da tag RFID
- `nome`: Nome do atleta (ou "Desconhecido" se não cadastrado)
- `timestamp`: Data e hora da passagem (AAAA-MM-DD HH:MM:SS)

## ⚙️ Funcionamento
1. **Inicialização**:
   - Carrega o mapeamento de atletas do CSV
   - Abre conexão serial com o leitor RFID
   - Cria/verifica o arquivo CSV de saída

2. **Loop de Leitura**:
   - Verifica dados recebidos pela serial
   - Converte bytes para string hexadecimal
   - Extrai o ID da tag RFID
   - Busca o nome do atleta no mapeamento
   - Aplica lógica anti-duplicação (intervalo configurável)
   - Registra leituras válidas com timestamp

3. **Anti-Duplicação**:
   - Usa uma janela de tempo (`read_interval`) para evitar múltiplos registros
   - Limpa a memória de tags lidas após cada intervalo

## 📝 Observações
- Ajuste o `read_interval` conforme as características da prova
- O script assume uma posição específica do byte da tag RFID (posição 45-47 na string hex) - modifique se seu leitor usar formato diferente

## 📜 Licença
Este projeto é open-source e disponível para modificação e distribuição.
