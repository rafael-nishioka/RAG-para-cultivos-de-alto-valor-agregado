# RAG para Cultivos de Alto Valor Agregado

#### Aluno: [Rafael Keniti Gerbasi Nishioka](https://github.com/rafael-nishioka)
#### Orientadora: [Manoela Kohler](https://github.com/manoelakohler).

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- [Link para o código](RAG_cultivos_de_alto_valor.ipynb).


---

### Resumo

Este trabalho apresenta o desenvolvimento e a avaliação de um sistema de Recuperação Aumentada por Geração (Retrieval-Augmented Generation — RAG) aplicado à consulta fitossanitária para culturas protegidas de alto valor econômico — tomate, morango e pimenta. O sistema combina um modelo de linguagem de grande porte (Large Language Model — LLM) com uma base de conhecimento técnico extraída de publicações oficiais da Embrapa, permitindo respostas precisas, rastreáveis e fundamentadas em literatura agronômica nacional. A motivação central do trabalho é a constatação de que modelos de linguagem generalistas, embora fluentes e convincentes, tendem a produzir respostas tecnicamente incorretas ou desatualizadas quando consultados sobre manejo fitossanitário — podendo inclusive recomendar produtos sem registro vigente no Brasil. A arquitetura proposta utiliza exclusivamente componentes de código aberto e gratuito: modelo de linguagem Llama 3.3 70B Versatile servido via Groq, embeddings multilíngues via Sentence Transformers, e banco de dados vetorial ChromaDB. Os resultados da avaliação comparativa demonstram que o sistema com recuperação de contexto (RAG) produz respostas significativamente mais precisas, específicas ao contexto brasileiro e auditáveis do que o mesmo modelo de linguagem operando sem acesso à base documental, evidenciando o valor da abordagem para aplicações de tomada de decisão técnica em contextos de alta responsabilidade.

### Abstract

This work presents the development and evaluation of a Retrieval-Augmented Generation (RAG) system applied to phytosanitary consultation for high-value protected crops — tomato, strawberry, and pepper. The system combines a Large Language Model (LLM) with a technical knowledge base extracted from official Embrapa publications, enabling accurate, traceable, and agronomically grounded responses. The central motivation of this work is the observation that general-purpose language models, while fluent and persuasive, tend to produce technically incorrect or outdated answers when queried about pest and disease management — potentially recommending products no longer authorized for use in Brazil. The proposed architecture relies exclusively on open-source and free components: the Llama 3.3 70B Versatile language model served via Groq, multilingual embeddings via Sentence Transformers, and the ChromaDB vector database. Comparative evaluation results demonstrate that the context-retrieval (RAG) system produces significantly more accurate, locally relevant, and auditable responses than the same language model operating without access to the documental base, highlighting the value of this approach for high-responsibility technical decision support.

### 1. Introdução

O Brasil ocupa posição de grande visibilidade na produção agrícola mundial, sendo um dos principais produtores alimentícios do planeta. Culturas protegidas de alto valor econômico agregado — como tomate, morango e pimenta — representam segmentos importantes da olericultura nacional, caracterizados por ciclos produtivos intensivos, alta sensibilidade a pragas e doenças, e necessidade de manejo fitossanitário técnico e tempestivo.

O diagnóstico correto e o manejo adequado de pragas e doenças são fatores determinantes para a produtividade e a viabilidade econômica dessas culturas. Esse conhecimento está concentrado em publicações técnicas da Embrapa — públicas e gratuitas — mas cujo acesso exige tempo e familiaridade com terminologia que nem sempre está disponível ao produtor no momento da decisão. Paralelamente, modelos de linguagem generalistas apresentam limitação estrutural relevante: operam com conhecimento estático, sem garantia de atualização ou conformidade regulatória, podendo recomendar produtos agrotóxicos sem registro vigente no Brasil — comportamento conhecido como alucinação.

### 2. Modelagem

O sistema segue a arquitetura RAG clássica com cinco componentes principais: base documental (16 publicações técnicas da Embrapa cobrindo tomate, morango, pimenta e pimentão), pipeline de processamento e segmentação (chunking de 1.000 caracteres com overlap de 150), modelo de embeddings multilíngue (paraphrase-multilingual-MiniLM-L12-v2 via Sentence Transformers), banco de dados vetorial (ChromaDB, recuperação top-k=5 por similaridade de cosseno), e modelo de linguagem gerador (Llama 3.3 70B Versatile via Groq, temperature=0.1).

A engenharia de prompt foi calibrada para equilibrar dois riscos opostos: alucinação (modelo extrapola além do contexto) e subutilização (modelo recusa responder quando há informação parcialmente relevante). A base documental foi construída iterativamente — partindo de 7 documentos e expandida para 16 à medida que lacunas de cobertura eram identificadas durante os testes.

### 3. Resultados

A avaliação comparativa demonstrou diferença estrutural de comportamento entre o LLM isolado e o sistema RAG. No caso principal (condições climáticas favoráveis à mosca-branca no tomateiro), o modelo sem RAG respondeu com precisão numérica aparente — inventando faixas de temperatura e umidade sem qualquer fonte verificável. O sistema RAG reconheceu explicitamente a ausência de informação na base, declarando que "as condições climáticas que favorecem a mosca-branca no tomate não são explicitamente mencionadas no contexto fornecido" — comportamento de honestidade epistêmica diretamente atribuível à engenharia de prompt.

No caso complementar (ácaro rajado no tomateiro), o sistema RAG recuperou e sintetizou informação de múltiplas páginas, produzindo resposta tecnicamente detalhada com protocolo de inspeção, nível de controle quantitativo e medidas de manejo — todas com página de origem citada.

### 4. Conclusões

A integração de um modelo de linguagem com uma base de conhecimento técnico oficial e rastreável produz respostas substancialmente mais precisas, contextualmente relevantes e auditáveis do que o mesmo modelo operando isoladamente. A contribuição central deste trabalho não reside na criação de novo conhecimento agronômico — toda a informação utilizada já é pública e disponibilizada pela Embrapa — mas na redução da barreira de acesso a esse conhecimento, tornando-o consultável em linguagem natural, de forma rápida e potencialmente operável em condições de conectividade limitada, situação frequente em propriedades rurais brasileiras.

---

Matrícula: 232100394

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
