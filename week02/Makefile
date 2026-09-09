SHELL := /bin/bash
.DELETE_ON_ERROR:

ACC = GCF_000008525.1_ASM852v1
NAME = Helicobacter_pylori_26695
BASE = https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/008/525/$(ACC)
CURL = curl -L --fail --retry 5 --retry-delay 3

FA = fasta/$(NAME).fa
FA_GZ = fasta/$(NAME).fna.gz
GFF_RAW = gff/$(NAME).raw.gff.gz
GFF = gff/$(NAME).gff.gz
SUMMARY = reports/summary.tsv

.PHONY: all downloads summary clean

all: $(FA).fai $(GFF).tbi summary

downloads: $(FA_GZ) $(GFF_RAW)

summary: $(SUMMARY)

fasta gff reports:
	mkdir -p $@

$(FA_GZ): | fasta
	$(CURL) $(BASE)/$(ACC)_genomic.fna.gz -o $@

$(GFF_RAW): | gff
	$(CURL) $(BASE)/$(ACC)_genomic.gff.gz -o $@

$(FA): $(FA_GZ)
	gunzip -c $< > $@

$(FA).fai: $(FA)
	samtools faidx $<

# The NCBI GFF contains an embedded FASTA section; remove it before indexing.
$(GFF): $(GFF_RAW)
	gunzip -c $< > gff/$(NAME).tmp.gff
	(awk '/^##FASTA/ { exit } /^#/ { print }' gff/$(NAME).tmp.gff; \
	 awk '/^##FASTA/ { exit } /^[^#]/ { print }' gff/$(NAME).tmp.gff | sort -t '	' -k1,1 -k4,4n -k5,5n) \
		| bgzip > $@.tmp
	mv $@.tmp $@
	rm -f gff/$(NAME).tmp.gff

$(GFF).tbi: $(GFF)
	tabix -p gff $<

$(SUMMARY): $(FA_GZ) $(GFF_RAW) | reports
	@echo -e "metric\tvalue" > $@
	@echo -e "organism\tHelicobacter pylori 26695" >> $@
	@echo -e "assembly\t$(ACC)" >> $@
	@gunzip -c $(FA_GZ) | awk '/^>/ { n++; next } { len += length($$0) } END { print "sequences\t" n; print "genome_bp\t" len }' >> $@
	@gunzip -c $(GFF_RAW) | awk 'BEGIN { FS="\t" } /^##FASTA/ { exit } /^[^#]/ && NF >= 9 { n++ } END { print "gff_rows\t" n }' >> $@
	@gunzip -c $(GFF_RAW) | awk 'BEGIN { FS="\t" } /^##FASTA/ { exit } /^[^#]/ && $$3 == "gene" { n++ } END { print "gene_rows\t" n }' >> $@

clean:
	rm -rf fasta gff reports
