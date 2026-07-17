Dicionário de Língua Fictícia — Baldurês

Projeto da disciplina C04 – Algoritmos 3 (Inatel): um dicionário interativo de terminal para uma língua fictícia (Baldurês ↔ Português), com sincronização automática dos dados para uma planilha no Google Sheets.

O programa começa com 50 palavras pré-cadastradas e permite consultar significados, descobrir sinônimos, ordenar o vocabulário e medir a "similaridade" entre palavras.

Funcionalidades


Cadastrar palavra fictícia com um ou mais significados em português e coordenadas (x, y, z)
Listar os significados de uma palavra
Listar sinônimos (palavras que compartilham pelo menos um significado)
Listar palavras em ordem alfabética
Listar palavras por ordem de tamanho
Remover palavra (atualizando todas as estruturas)
Calcular a similaridade entre duas palavras pela distância Euclidiana das coordenadas
Buscar palavra na árvore binária de busca (BST)
Imprimir a BST em ordem (in-ordem), com a altura da árvore


Toda inclusão ou remoção dispara a sincronização automática: o programa exporta o dicionário em CSV e chama o script Python que atualiza a planilha no Google Sheets.

Estruturas de dados e conceitos aplicados


Grafo bipartido (unordered_map + unordered_set): liga palavras fictícias aos seus significados nos dois sentidos. É ele que permite encontrar sinônimos — duas palavras são sinônimas quando estão conectadas a um mesmo significado.
Árvore binária de busca (BST) implementada do zero: inserção, busca, remoção (incluindo o caso de dois filhos, usando o sucessor), percurso in-ordem e cálculo de altura.
Tabela hash (unordered_map) como índice de acesso O(1) da palavra para sua posição no vetor.
Ordenações com sort e stable_sort, incluindo comparador customizado (ordenação por tamanho com desempate alfabético).
Remoção O(1) em vetor com a técnica de troca com o último elemento.
Distância Euclidiana em 3D como métrica de similaridade entre palavras.
Normalização de strings (minúsculas e remoção de espaços) para evitar duplicatas e falhas de busca.


Integração com o Google Sheets

dicionario.cpp  →  exporta dicionario.csv  →  tabela_significados.py  →  Google Sheets

O script Python lê o CSV com pandas e usa as bibliotecas gspread e gspread-dataframe para limpar e reescrever a planilha, congelando a linha de cabeçalho.

Como executar

Programa principal (C++)

bashg++ -std=c++17 dicionario.cpp -o dicionario
./dicionario

Antes de compilar, ajuste as duas constantes no início do dicionario.cpp para os caminhos da sua máquina: CAMINHO_CSV (onde o CSV será salvo) e CMD_ATUALIZAR_SHEETS (caminho do script Python).

O programa funciona normalmente mesmo sem a parte do Google Sheets configurada — a sincronização apenas exibe um aviso de falha.

Sincronização com o Sheets (opcional)

bashpip install pandas gspread gspread-dataframe oauth2client

Também é necessário um arquivo credentials.json de uma conta de serviço do Google Cloud (com a API do Google Sheets habilitada e a planilha compartilhada com o e-mail da conta de serviço), salvo na mesma pasta do script. Por segurança, esse arquivo não vai para o repositório.

Estrutura do repositório

PROJETO-ALGORITMOS-3/
├── dicionario.cpp            → programa principal (menu, estruturas e lógica)
├── tabela_significados.py    → sincronização do CSV com o Google Sheets
└── README.md
