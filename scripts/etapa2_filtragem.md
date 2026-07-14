# Etapa 2: Filtragem de Qualidade e Limpeza de Dados (Data Cleansing)

Esta etapa do pipeline aplica critérios de controle de qualidade (QC) sobre o arquivo de variantes importado, eliminando potenciais falsos-positivos e ruídos gerados pela leitura física da máquina de sequenciamento (NGS).

## 💻 Código em R

```R
# ==============================================================================
# PIPELINE NGS - ETAPA 2: FILTRAGEM DE QUALIDADE (QC)
# ==============================================================================

# 1. Inspeção das métricas de qualidade fixas do arquivo VCF
head(fixed(vcf_dados))

# 2. Aplicação do filtro de corte clínico (QUAL > 20)
# Utilizada a função which() para tratar índices nulos (NAs) na coluna de qualidade,
# garantindo a robustez do pipeline contra exceções de dados.
vcf_filtrado <- vcf_dados[which(fixed(vcf_dados)$QUAL > 20), ]

# 3. Extração das coordenadas genômicas filtradas
variantes_filtradas_gr <- rowRanges(vcf_filtrado)

# 4. Auditoria de Dados (Métricas de Saída)
cat("--- Relatório de Controle de Qualidade ---\n")
cat("Variantes brutas injetadas:", length(rowRanges(vcf_dados)), "\n")
cat("Variantes aprovadas no filtro (QUAL > 20):", length(variantes_filtradas_gr), "\n")
cat("Registros descartados (ruído):", length(rowRanges(vcf_dados)) - length(variantes_filtradas_gr), "\n")
