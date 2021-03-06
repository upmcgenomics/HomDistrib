# HOMDISTRIB
HomologsDistribution

## ABOUT
Tara Oceans expeditions have produced a huge amount of genomics and transcriptomics data - metaomics in short - which provide a global and unprecedented view of planktonic diversity. In particular, a global atlas of eucaryotic genes has been produced (Carradec et al., submitted) and metaomics data are available. From eukaryotic Tara samples (68 stations, 2 depths, 4 size classes), after high-coverage polyA-based RNA-Seq and cDNA assembly, unigenes have been defined (longest sequences of each cluster of contigs - cutoff identity > 95% and length > 150 pb). Among these data, specific homologs of sequences of interest could be searched in order to explore their distribution.

HomDistrib produces a Blast Database from a compressed FASTA file containing nucleotide sequences. Notice that here unigenes sequences are used to built the database but any compressed FASTA file of nucleotide sequences is adapted. A tBlastn is run and data saved. Then, the blast output data are filtered in order to conserve only relevant ones (supposed real homologs). Filtering parameters can be modulated (see section USAGE - Parameters). Finally, several steps are made to link homologous genes identifiers to geographic positions.

## VERSION
1.0

## REQUIREMENTS
**NCBI-BLAST+**	version 2.2.29+ (Camacho et al., 2009)

**perl** version 5.14.2

**R sofware** version 3.2.5 (R Core Team, 2000)
require packages _plyr_ (v1.8.4, Wickham, 2011), _dplyr_ (v0.7.1, Wickham and Francois, 2015), _stringr_ (v1.2.0, Wickham, 2012), _ggplot2_ (v2.2.1, Wickham, 2010), _reshape2_ (v1.4.2, Wickham, 2007), _maptools_ (v0.9-2, Bivand, 2016) and _gpclib_ (v1.5-5, Peng et al., 2007).

## USAGE

### Datasets

- Geneset file : 116 M unigenes sequences in a compressed fasta file (decompressed file ~ 60 Go).

- MetaGenomics matrix : geneID (used in the BLAST database) in colums (43), sampleID in rows (69 501 598).
This matrix is used to link a gene identifier to its sample identifier.

- Correspondance table (8 columns and 6678 rows table) : SampleID (1st), station (4th) and depth (5th) are used to link sample ID to a station ID (station number and three-letters depth code are concatenated).

- Environmental table (54 columns and 594 rows) : Only the first three columns are used to link station ID (extracted from 1st column) to GPS coordinates (2nd and 3rd columns).

- Continents : This folder contains world contours shapefiles which are used to plot results.

All needed datasets are given in the data folder. This pipeline is specificly implemented to use them. If other matrices are used, they need to be formatted in the same way.

### Parameters
- obligatory parameters
1. filename of compressed FASTA file to produce BLAST database (nucleotide sequences).
1. filename of multifasta : protein sequences of interest.

`perl ./homdistrib.pl --database <compressed_fasta> --fasta <query_sequences_fasta>`

- optional parameters
1. e-value (_evalue_) : the Expect value need to be positive and writen in the scientific notation. By default : 1e-20.
2. coverage (_cov_) : Three blast outputs are used to compute coverage (from 0 to 100) : length of query sequence, length of subject sequence and number of aligned positions. It describes the percentage of aligned positions of the smallest sequence. By default : 80%.
3. identity (_identity_) : The identity is the percentage of exact position match (from 0 to 100). By default : 80%.
4. maximum all (_max\_all_) : Number of best hits (based on e-value) in order to save restrained results.
5. maximum sequences (_max\_seq_) : Number of best hits (based on e-value) by query sequence.
6. number of threads (_nb\_thread_) : By default : 20.

`perl ./homdistrib.pl --database --fasta --evalue --cov --identity --max_all --max_seq --nb_thread`

## OUTPUTS
Three folders will be created at the root of the project :
- **db** : It contains all previously created nucleotide sequences databases. If
- **bl** : It contains all previous blast outputs which are unique for given database and multifasta file of query sequences.
- **res** : It contains all filtered results. Text files names are encoded with applied parameters. Text outputs are always saved in two versions : a detailled one with all alignment characteristics and a synthetic one which only contains sequences identifiers.

In the **res** folder, **plot** folder stocks maps and graphs.
 
## REFERENCES
Bivand, R. (2016). Lewin-Koh N. maptools: Tools for reading and handling spatial objects. R package version 0.8–27. 2013.

Camacho, C., Coulouris, G., Avagyan, V., Ma, N., Papadopoulos, J., Bealer, K., & Madden, T. L. (2009). BLAST+: architecture and applications. BMC bioinformatics, 10(1), 421.

Peng, R. D., Murta, A., & Peng, M. R. D. (2007). The gpclib Package.

Team, R. C. (2000). R language definition. Vienna, Austria: R foundation for statistical computing.

Wickham, H., & Francois, R. (2015). dplyr: A grammar of data manipulation. R package version 0.4, 1, 20.

Wickham, H. (2012). stringr: Make it easier to work with strings. R package version 0.6, 2, 96-7.

Wickham, H. (2011). The Split-Apply-Combine Strategy for Data Analysis. Journal of Statistical Software, 40(1), 1-29. URL http://www.jstatsoft.org/v40/i01/.

Wickham, H. (2010). ggplot2: elegant graphics for data analysis. J Stat Softw, 35(1), 65-88.

Wickham, H. (2007). Reshaping Data with the reshape Package. Journal of Statistical Software, 21(12), 1-20. URL http://www.jstatsoft.org/v21/i12/.
