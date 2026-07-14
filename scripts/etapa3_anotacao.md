# Etapa 3: Anotação Funcional e Harmonização de Dados Genômicos

Esta etapa realiza o cruzamento (*join* genômico) entre as variantes filtradas por qualidade e o banco de dados master de transcritos do genoma humano (`TxDb`), mapeando o impacto anatômico e funcional de cada mutação.

## 💻 Código em R

```R
# ==============================================================================
# PIPELINE NGS - ETAPA 3: HARMONIZAÇÃO E ANOTAÇÃO FUNCIONAL
# ==============================================================================

# 1. Carregamento do Banco de Dados de Referência (Genoma Humano hg19 via UCSC)
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
if (!requireNamespace("TxDb.Hsapiens.UCSC.hg19.knownGene", quietly = TRUE)) {
    BiocManager::install("TxDb.Hsapiens.UCSC.hg19.knownGene", ask = FALSE)
}

library(TxDb.Hsapiens.UCSC.hg19.knownGene)
txdb <- TxDb.Hsapiens.UCSC.hg19.knownGene

# 2. Harmonização e Padronização de Strings (Resolução de Incompatibilidade)
# Arquivos VCF do projeto 1000 Genomas utilizam indexação numérica pura ("1", "2"),
# enquanto o ecossistema UCSC exige o prefixo "chr" ("chr1", "chr2").
# Abaixo, a string é normalizada diretamente no metadado do vetor de coordenadas:
nomes_atuais <- seqlevels(variantes_filtradas_gr)
novos_nomes <- paste0("chr", nomes_atuais)
seqlevels(variantes_filtradas_gr) <- novos_nomes

# 3. Cruzamento de Dados (Genomic Join) para Localização das Variantes
cat("Mapeando a localização biológica das variantes...\n")
anotacoes <- locateVariants(variantes_filtradas_gr, txdb, AllVariants())

# 4. Inspeção do Output Estruturado
print(anotacoes)
