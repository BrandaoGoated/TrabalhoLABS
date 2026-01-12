# TrabalhoLABS
# Trabalho – Laboratórios de Bioinformática (2025/2026)

Este repositório contém um **notebook em Python (BioPython)** para análise de 4 genes de um bacteriófago, incluindo tradução, procura de homólogos por BLAST, extração de anotações (GenBank) e filogenia (alinhamento + árvore). [file:221]

## Estrutura do repositório

- `Trabalho.ipynb` – Notebook principal com todo o pipeline.  
- `14296266.fasta`, `14296280.fasta`, `14296281.fasta`, `14296289.fasta` – Sequências DNA de entrada (1 por ficheiro).  
- `resultados/` – Outputs gerados automaticamente pelo notebook:
  - `nossos_genes_traduzidos.faa` – Proteínas traduzidas a partir dos genes (melhor ORF entre 6 frames).
  - `resumo_genes_blast.csv` – Resumo dos top hits do BLAST (acessão, descrição, e-value, % identidade).
  - `features_genbank_por_gene.csv` – Features/qualifiers do GenBank que intersectam cada intervalo do gene.
  - `para_filogenia_com_homologos.faa` – FASTA com as 4 proteínas + homólogos filtrados (por e-value e % identidade).
  - `para_filogenia_com_homologos.aln.fasta` – Alinhamento múltiplo (MUSCLE).
  - `arvore_com_homologos.nwk` – Árvore filogenética (Neighbour-Joining) em formato Newick.
  - `blastp_xml/` – Resultados BLASTP em XML (um ficheiro por gene).
  - `genbank/NC_019914.1.gb` – Registo GenBank do genoma usado para extrair anotações/features.

## O que o notebook faz (pipeline)

1. **Leitura e validação das entradas**  
   Lê os 4 FASTA, confirma que são DNA (A/C/G/T/N) e normaliza a sequência.

2. **Tradução para proteína (ORF)**  
   Procura a melhor tradução (6 frames) e guarda as proteínas em `nossos_genes_traduzidos.faa`.

3. **Homologia (BLASTP)**  
   Corre BLASTP para cada proteína e guarda os resultados em XML dentro de `resultados/blastp_xml/`.  
   Nota: os nomes dos XML usam um `safe_id` (substitui `:` e `-`) para evitar problemas de nomes de ficheiros no Windows.

4. **Resumo BLAST**  
   Extrai os top hits e gera `resumo_genes_blast.csv`.

5. **Download de homólogos (Entrez)**  
   Seleciona homólogos com thresholds (e-value e % identidade), faz download via Entrez e cria `para_filogenia_com_homologos.faa`.

6. **GenBank + features**  
   Faz download do GenBank do genoma e exporta para CSV as CDS/features que intersectam cada gene (`features_genbank_por_gene.csv`).

7. **Alinhamento múltiplo e filogenia**  
   Usa MUSCLE para gerar `para_filogenia_com_homologos.aln.fasta` e constrói uma árvore NJ (distância por identidade), guardando em `arvore_com_homologos.nwk`.

## Como correr

### Requisitos
- Python 3
- Bibliotecas: `biopython`
- Acesso à internet (NCBI BLAST e Entrez)
- MUSCLE 

### Passos
1. Garantir que os 4 ficheiros `.fasta` estão na mesma pasta do notebook.
2. Confirmar que o caminho para o MUSCLE (`muscle_exe`) está correto no notebook.
3. Definir `Entrez.email` com um email válido.
4. Executar o notebook do início ao fim; os outputs ficam em `resultados/`.

## Notas
- O BLAST online pode demorar e está sujeito a limites; o notebook inclui pausas (`sleep`) para reduzir throttling.
- Os ficheiros em `resultados/` podem ser apagados e regenerados voltando a correr o notebook.

Grupo 2:
David Brandão PG59418
Fábio Ernesto PG61008
Pedro Fontão PG59429
Rafael Lourenço PG61010
