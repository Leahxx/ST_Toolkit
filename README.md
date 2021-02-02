# ST_Toolkit
tools to handle spatial transcriptome data

## compile
## Platform & Environment
* python3.6.5+

## Prerequisties

| Package       | Version  | Description                                                |
| ------------- | -------- | ---------------------------------------------------------- |
| pandas        | <=1.0.1  | handle dataframe                                           |
| numpy         | 1.19.1   |                                                            |
| scipy         | 1.5.2    | generate sparse matrix                                     |
| anndata       | 0.7.4    | generate h5ad format file to store gene expression matrix  |
| optparse      | 0.1.1    | manage command options.                                    |
| scanpy        | 1.6.0    | read h5ad format file and do cell cluster                  |
| numexpr       | 2.7.1    | accellerate operation on numpy array                       |
| opencv-python |4.4.0.42  | process image matrix                                       |

## Run

### ST_Handle_Exp.py Usage
```
python3 ST_Handle_Exp.py 

Usage: ST_Handle_Exp.py action [options]

Options:
  -h, --help            show this help message and exit
  -i INFILE, --in=INFILE
                        input gene expression matrix file path.
  -o OUT, --out=OUT     output file or directory path.
  -s BINSIZE, --binSize=BINSIZE
                        The bin size or max bin szie that to combine the dnbs.
                        default=50
  -t THREAD, --thread=THREAD
                        number of thread that will be used to run this
                        program. default=2
  -w PROGRESS, --progress=PROGRESS
                        number of progress that will be used to run this
                        program, only useful for visulization. default=4
```
Required parameters:
* -i INFILE, --in=INFILE input gene expression matrix file path.
* -o OUT, --out=OUT  output file or directory path.

Optional parameters:
* -s BINSIZE, --binSize=BINSIZE The bin size or max bin szie that to combine the dnbs.default=50.
* -t THREAD, --thread=THREAD number of thread that will be used to run this program. default=2.
* -w PROGRESS, --progress=PROGRESS number of progress that will be used to run this program, only useful for visulization. default=4.

#### action-tsv2h5ad
```
description:
  merge number of binSize*binSize dnb data to one spot and save new spatial gene expression matrix to h5ad format file. 
e.g:
  python3 ST_Handle_Exp.py tsv2h5ad -i merge_GetExp_gene.txt -o merge_GetExp_gene_bin50.h5ad -s 50
``` 
#### action-cellCluster
```
description:
  after transfered gene expression mtrix from tsv format to h5ad format, you can use the h5ad formt file as input for cellCluster action to cluster bins to different cell types.
e.g:
  python3 ST_Handle_Exp.py cellCluster -i merge_GetExp_gene_bin50.h5ad -o cell_cluster.h5ad
```
#### action-visualization
```
description:
  generate pickle format file with spatial gene expression matrix that has been grouped by different binSize.
e.g:
  python3 ST_Handle_Exp.py visualization -i merge_GetExp_gene.txt -o visualization -s 1000
```
#### action-convertBinData
```
description:
  when you get the gene expression matrix with specified binSize using the lesso tool of stereomics which can extract data of certain areas, and want to get the original gene expression matrix of binSize 1 under the certain areas. The convertBinData action can help to achieve. Please give the same binSize option as your lesso data, and the binSize of lesso data shouldn't be greater than 50.
e.g:
  python3 ST_Handle_Exp.py convertBinData -i merge_GetExp_gene.txt -m gene_bin50_lesso.txt -o tissue_gene_expression.txt -s 50
```

### ST_CellCluster.py Usage
```
python3 ST_CellCluster.py action [options]

Usage: ST_CellCluster.py [options]

Options:
  -h, --help            show this help message and exit
  -i INFILE, --in=INFILE
                        input file path.
  --h5ad=H5AD           h5ad format output file path.
  ```
  #### action-loom2h5ad
  ```
  description:
    This function can transform loom format file generated by seurat into h5ad format file that can be loaded by stereomap system.
  e.g:
    python3 ST_CellCluster.py loom2h5ad -i cell_cluster.loom --h5ad cell_cluster.h5ad

```
#### action-markerGene2h5ad
```
description:
  This function can add csv format marker gene list file generated by seurat into h5ad file.
e.g:
  python3 ST_CellCluster.py markerGene2h5ad -i marker_gene.csv --h5ad cell_cluster.h5ad
```