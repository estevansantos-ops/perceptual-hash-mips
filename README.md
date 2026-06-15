# Perceptual Hash (pHash) para Autenticação de Imagens

Este projeto implementa um sistema de verificação de autenticidade de imagens digitais baseado em funções de Perceptual Hash (pHash). O objetivo é auxiliar artistas na proteção de suas obras contra uso não autorizado, um problema crescente devido ao avanço de ferramentas de Inteligência Artificial generativa.

Projeto desenvolvido para a disciplina de Arquitetura e Organização de Computadores.

## Arquitetura e Tecnologias

O grande diferencial deste projeto é a integração de linguagens de alto e baixo nível para criar um fluxo de processamento eficiente:
* **Python:** Responsável pelo pré-processamento das imagens (escala de cinza e redimensionamento) e pela extração de características matemáticas complexas.
* **Assembly MIPS (32 bits):** Responsável pelas operações lógicas de baixo nível, como o cálculo dos hashes e as métricas de similaridade bit a bit direto no processador.

## Como Funciona

O algoritmo é dividido em quatro etapas principais:
1. **Pré-processamento:** Conversão da imagem para escala de cinza e redimensionamento para 32x32 pixels, focando apenas na luminância.
2. **Extração de Características:** Aplicação da Transformada Discreta de Cosseno (DCT) para obter os componentes de frequência da imagem, focando nas baixas frequências que são mais estáveis a manipulações.
3. **Geração do pHash:** Cálculo do valor mediano dos coeficientes DCT e geração de um hash binário de 64 bits.
4. **Métrica de Similaridade:** Cálculo da Distância de Hamming Normalizada entre dois hashes. Valores próximos a 0 indicam alta semelhança, enquanto valores próximos a 0.5 indicam imagens completamente distintas.

## Estrutura do Repositório

O projeto está organizado da seguinte forma:

* `/Codigos/get_input.asm`: Script em Assembly responsável por capturar os caminhos dos diretórios das imagens que serão comparadas e salvá-los em um arquivo de texto.
* `/Codigos/phash_DCT.py`: Script em Python que lê as imagens de entrada, aplica a matriz DCT e exporta os coeficientes extraídos para arquivos `.bin`.
* `/Codigos/phash_64bits_hamming.asm`: Código principal em Assembly que lê os arquivos binários , calcula os hashes de 64 bits (divididos em registradores de 32 bits) e computa a Distância de Hamming para retornar o grau de similaridade.
* `Documentacao.pdf`: Relatório completo com a fundamentação teórica detalhada, modelagem matemática e análise de resultados.

## Resultados de Testes

O sistema foi testado com obras de arte reais e provou ser capaz de identificar manipulações e edições. Os resultados obtidos (Distância de Hamming Normalizada) foram:
* **Variações sutis da mesma obra:** 0.015 a 0.093 (Alta similaridade detectada).
* **Imagens com edições e marcas d'água:** 0.062 a 0.156 (Autenticidade detectada, imune a ruídos).
* **Obra original vs. Imagem gerada por IA:** 0.468 (Identificada corretamente como imagem distinta/não autêntica).
