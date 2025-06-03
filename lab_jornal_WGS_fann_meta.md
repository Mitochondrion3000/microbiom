spades.py --meta \
    --pe1-1 /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_1.fastq.gz \
    --pe1-2 /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SRR29762644_2.fastq.gz \
    --disable-gzip-output \
    --threads 12 \
    -o SPAdes_meta_k33_77_111 \
    -k 33,77,111

качество quast.py -o quast_report /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SPAdes_meta_k33_77_111_meta/contigs.fasta

All statistics are based on contigs of size >= 500 bp, unless otherwise noted (e.g., "# contigs (>= 0 bp)" and "Total length (>= 0 bp)" include all contigs).

Assembly                    contigs 
# contigs (>= 0 bp)         46827   
# contigs (>= 1000 bp)      1541    
# contigs (>= 5000 bp)      136     
# contigs (>= 10000 bp)     64      
# contigs (>= 25000 bp)     17      
# contigs (>= 50000 bp)     0       
Total length (>= 0 bp)      21561607
Total length (>= 1000 bp)   4118643 
Total length (>= 5000 bp)   1801406 
Total length (>= 10000 bp)  1303472 
Total length (>= 25000 bp)  563839  
Total length (>= 50000 bp)  0       
# contigs                   9107    
Largest contig              42830   
Total length                9036332 
GC (%)                      56.82   
N50                         909     
N90                         542     
auN                         4798.0  
L50                         1961    
L90                         7367    
# N's per 100 kbp           0.00    

потом кидаем в prokka и аннотируем

prokka --outdir prokka_no_bin --prefix contigs_no_bin /home/ivan/Desktop/itmo/microbiom/skin_microbiome/SPAdes_meta_k33_77_111_meta/contigs.fasta

достаем все с COG аннотацией
awk -F'\t' '$6 != "" {print $0}' contigs_no_bin.tsv > genes_with_cog.tsv

получаем вот такой файл 
locus_tag	ftype	length_bp	gene	EC_number	COG	product
EIEIELNM_00003	CDS	1746	proS_1	6.1.1.15	COG0442	Proline--tRNA ligase
EIEIELNM_00005	CDS	768	yfcA_1		COG0730	putative membrane transporter protein YfcA
EIEIELNM_00007	CDS	537	rimP_1		COG0779	Ribosome maturation factor RimP
EIEIELNM_00008	CDS	972	nusA_1		COG0195	Transcription termination/antitermination protein NusA
EIEIELNM_00015	CDS	1374	egsA	1.1.1.261	COG0371	Glycerol-1-phosphate dehydrogenase [NAD(P)+]
EIEIELNM_00017	CDS	369	srlB_1		COG3731	PTS system glucitol/sorbitol-specific EIIA component
EIEIELNM_00018	CDS	1050	srlE	2.7.1.198	COG3732	PTS system glucitol/sorbitol-specific EIIB component
EIEIELNM_00019	CDS	579	srlA		COG3730	PTS system glucitol/sorbitol-specific EIIC component
EIEIELNM_00021	CDS	1923	licR		COG1762	putative licABCH operon regulator
EIEIELNM_00024	CDS	888	truB_1	5.4.99.25	COG0130	tRNA pseudouridine synthase B
EIEIELNM_00026	CDS	1368	opuAA	7.6.2.9	COG4175	Glycine betaine transport ATP-binding protein OpuAA
EIEIELNM_00027	CDS	867	opuAB		COG4176	Glycine betaine transport system permease protein OpuAB
EIEIELNM_00032	CDS	2202	pnp_1	2.7.7.8	COG1185	Polyribonucleotide nucleotidyltransferase

и я не знаю как адекватно визуализировать этот файл тбо все представленное тут это CDS, категорий разных по столбцам gene, EC_number, COG, product тысячи



