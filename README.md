# 📰 Automação de Clipping de Notícias e Newsletter (n8n)

Este projeto consiste em um workflow automatizado desenvolvido no **n8n** para monitoramento, coleta e curadoria de notícias do mercado publicitário e varejista.

O agente realiza o *web scraping* de múltiplos portais, verifica duplicidade de conteúdo utilizando algoritmos de similaridade textual (sem custos de IA), armazena o histórico em banco de dados e envia um resumo semanal formatado via e-mail.

## 🚀 Funcionalidades

- **Multi-Source Scraping:** Coleta dados simultaneamente de 7 grandes portais do mercado.
- **Extração Inteligente:** Uso de Seletores CSS específicos para capturar títulos e links de cada layout de site.
- **Deduplicação Lógica (Smart Filtering):**
  - **Memória Histórica:** Verifica no Google Sheets se a notícia já foi enviada anteriormente.
  - **Algoritmo de Similaridade:** Compara títulos das notícias extraídas usando *Jaccard Similarity* e *Distância de Levenshtein* para identificar e agrupar notícias sobre o mesmo assunto vindas de fontes diferentes, evitando redundância no e-mail final.
- **Banco de Dados:** Registro automático de todas as notícias processadas para evitar repetições futuras.
- **Geração de Newsletter:** Montagem dinâmica de HTML responsivo com as notícias da semana.

## 🌐 Portais Monitorados

O fluxo está configurado para extrair dados dos seguintes veículos:
* Meio e Mensagem
* Propmark
* Adnews
* Marcas pelo Mundo
* Mercado e Consumo
* Giro News
* Nosso Meio

## 🛠️ Tecnologias Utilizadas

* **n8n** (Workflow Automation)
* **JavaScript** (Lógica de processamento e algoritmos de similaridade)
* **CSS Selectors** (Extração de dados HTML)
* **Google Sheets API** (Persistência de dados/Memória)
* **SMTP** (Disparo de e-mails)

## ⚙️ Como Configurar

### Pré-requisitos
* Uma instância do [n8n](https://n8n.io/) instalada (local ou cloud).
* Credenciais configuradas no n8n para:
  * Google Sheets (OAuth2).
  * Servidor de E-mail (SMTP).

### Passo a Passo
1. **Importar o Workflow:**
   - Baixe o arquivo `.json` deste repositório.
   - No n8n, vá em `Menu > Import from File` e selecione o arquivo.

2. **Configurar o Google Sheets:**
   - Crie uma planilha nova.
   - Na primeira linha, crie as colunas: `titulo`, `link`, `nome`.
   - Copie o ID da planilha (presente na URL) e atualize os nós **"Sheet Existente"** e **"Registra Notícia Nova"**.

3. **Ajustar Credenciais de E-mail:**
   - No nó **"Send email"**, selecione sua credencial SMTP.
   - Altere os campos `From Email` e `To Email` para os endereços desejados.

4. **Verificar Conexões:**
   - Certifique-se de que todos os nós de extração (Switch -> Extract -> Code) estejam conectados ao nó de convergência ("Ponto de Encontro").

## 🧠 Destaque Técnico: Algoritmo de Similaridade

Para evitar o uso de APIs pagas de Inteligência Artificial, foi implementado um nó de código (`Code Node`) em JavaScript puro que calcula a similaridade entre títulos de notícias.

O script utiliza uma combinação de:
1. **Tokenização:** Limpeza de *stopwords* (de, para, com) e normalização de texto.
2. **Índice de Jaccard:** Mede a interseção de palavras entre dois títulos.
3. **Distância de Levenshtein:** Mede o número de edições necessárias para transformar uma string em outra.

Se a similaridade ultrapassar o *threshold* definido, o sistema considera como "mesmo assunto" e mantém apenas a fonte com o título mais descritivo.

## 📝 Licença

Este projeto foi desenvolvido para fins de estudo e automação interna. Sinta-se à vontade para utilizar e adaptar o código.

---
Desenvolvido por [Mayara Cunha](https://github.com/mayaracunha-dev)
