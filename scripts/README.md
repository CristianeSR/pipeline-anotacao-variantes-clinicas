# Etapa 1: Importação e Estruturação de Dados Genômicos

Este script realiza a leitura de arquivos brutos de variantes genéticas no formato VCF (Variant Call Format) e faz a conversão para estruturas de dados genômicos especializados (`GRanges`) dentro do ecossistema Bioconductor.

## 💻 Código em R

```R
# ==============================================================================
# PIPELINE NGS - ETAPA 1: IMPORTAÇÃO E CONVERSÃO PARA GRANGES
# ==============================================================================

# 1. Instalação e Carregamento dos Pacotes do Bioconductor
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
if (!requireNamespace("VariantAnnotation", quietly = TRUE)) BiocManager::install("VariantAnnotation", ask = FALSE)

library(VariantAnnotation)
library(GenomicRanges)

# 2. Definição do Caminho do Arquivo Input (VCF)
# O arquivo foi baixado previamente a partir dos dados públicos do projeto 1000 Genomas
vcf_path <- "../data/valid-4.0.vcf"

# 3. Leitura e Parsing do Arquivo Bruto
cat("Lendo arquivo VCF...\n")
vcf_dados <- readVcf(vcf_path)

# 4. Extração das Variantes como Coordenadas Genômicas (GRanges)
cat("Convertendo variantes para estrutura GRanges...\n")
variantes_gr <- rowRanges(vcf_dados)

# 5. Inspeção do Objeto Estruturado
cat("Estrutura de dados gerada com sucesso:\n")
print(variantes_gr)
