# ============================================
# STEP 0: Required packages check + install (agar missing hon)
# ============================================
options(timeout = 1000)

if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

# Zaroori Bioconductor packages
required_packages <- c("DOSE", "enrichplot", "clusterProfiler")

for (pkg in required_packages) {
  if (!requireNamespace(pkg, quietly = TRUE)) {
    BiocManager::install(pkg, repos = BiocManager::repositories(), 
                          type = "binary", update = FALSE, ask = FALSE)
  }
}

# ============================================
# STEP 1: Libraries load karein
# ============================================
library(DOSE)
library(enrichplot)
library(clusterProfiler)

# ============================================
# STEP 2: Study set (upregulated DEGs) load karein
# ============================================
study_set <- read.table("C:/Users/MahRukh/Downloads/Galaxy93-[GO_study_set_upregulated protein ids.txt].txt",
                         header = FALSE,
                         col.names = "ProteinID",
                         stringsAsFactors = FALSE)

nrow(study_set)

# ============================================
# STEP 3: Background/mapping file load karein
# ============================================
background <- read.table("C:/Users/MahRukh/Downloads/Protein_KO_OnePerRow.txt",
                          sep = "\t", header = FALSE,
                          col.names = c("ProteinID", "KO"),
                          stringsAsFactors = FALSE)

nrow(background)

# ============================================
# STEP 4: Study set ke Protein IDs ko KO IDs se merge karein
# ============================================
study_set_KO <- merge(study_set, background, by = "ProteinID")

nrow(study_set_KO)

# ============================================
# STEP 5: Unique KO IDs nikalein (study set aur background)
# ============================================
study_KO <- unique(study_set_KO$KO)
background_KO <- unique(background$KO)

length(study_KO)
length(background_KO)

# ============================================
# STEP 6: KEGG Pathway Enrichment (KO-based) run karein
# ============================================
kegg_result <- enrichKEGG(gene         = study_KO,
                           organism     = "ko",
                           universe     = background_KO,
                           keyType      = "kegg",
                           pvalueCutoff = 0.05,
                           qvalueCutoff = 0.2)

# ============================================
# STEP 7: Result dataframe mein convert karein
# ============================================
result_df <- as.data.frame(kegg_result)

head(result_df)
nrow(result_df)

print(result_df[, c("ID", "Description", "GeneRatio", "BgRatio", 
                     "pvalue", "p.adjust", "qvalue", "Count")])

# ============================================
# STEP 8: Result CSV mein save karein
# ============================================
write.csv(result_df, "KEGG_enrichment_results.csv", row.names = FALSE)

# ============================================
# STEP 9: Visualization (barplot aur dotplot)
# ============================================
barplot(kegg_result, showCategory = 9, title = "KEGG Pathway Enrichment")
dotplot(kegg_result, showCategory = 9, title = "KEGG Pathway Enrichment")
