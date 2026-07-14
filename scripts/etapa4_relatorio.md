# Etapa 4: Extração de Alvos Genômicos e Geração do Relatório Clínico

Esta etapa final do pipeline consolida os dados anotados, isola os identificadores dos genes impactados pelas variantes filtradas e exporta um relatório estruturado para suporte ao laudo médico.

## 💻 Código em R

```R
# ==============================================================================
# PIPELINE NGS - ETAPA 4: EXTRAÇÃO DE ALVOS E EXPORTAÇÃO
# ==============================================================================

# 1. Isolamento dos identificadores Entrez Gene das variantes mapeadas
genes_afetados <- anotacoes$GENEID

# 2. Purificação do Dataset (Remoção de registros nulos/intergênicos e redundâncias)
genes_unicos <- unique(genes_afetados[!is.na(genes_afetados)])

# 3. Exibição dos alvos prioritários no console
cat("--- Alvos Genéticos Identificados ---\n")
print(genes_unicos)

# 4. Geração do Artefato de Saída (Relatório Clínico)
writeLines(genes_unicos, "../output/genes_alvo_laudo.txt")
cat("Processo concluído. Arquivo exportado para a pasta /output.\n")
