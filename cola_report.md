cola Report for Consensus Partitioning
==================

**Date**: 2019-12-04 01:03:47 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21288    52
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:kmeans](#SD-kmeans)     |          3| 1.000|           0.993|       0.984|** |           |
|[SD:pam](#SD-pam)           |          3| 1.000|           0.986|       0.994|** |2          |
|[CV:kmeans](#CV-kmeans)     |          3| 1.000|           0.979|       0.969|** |           |
|[CV:pam](#CV-pam)           |          2| 1.000|           0.966|       0.988|** |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 1.000|           0.971|       0.975|** |           |
|[MAD:pam](#MAD-pam)         |          3| 1.000|           0.965|       0.986|** |2          |
|[MAD:NMF](#MAD-NMF)         |          3| 1.000|           0.970|       0.988|** |2          |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           0.981|       0.993|** |           |
|[ATC:pam](#ATC-pam)         |          3| 1.000|           0.959|       0.987|** |2          |
|[ATC:NMF](#ATC-NMF)         |          3| 1.000|           0.947|       0.982|** |2          |
|[SD:mclust](#SD-mclust)     |          3| 0.999|           0.964|       0.979|** |           |
|[SD:hclust](#SD-hclust)     |          3| 0.939|           0.908|       0.967|*  |           |
|[SD:skmeans](#SD-skmeans)   |          5| 0.935|           0.916|       0.959|*  |3          |
|[ATC:hclust](#ATC-hclust)   |          6| 0.926|           0.877|       0.938|*  |2,3,4      |
|[MAD:skmeans](#MAD-skmeans) |          5| 0.922|           0.860|       0.938|*  |3          |
|[ATC:skmeans](#ATC-skmeans) |          3| 0.912|           0.923|       0.967|*  |2          |
|[CV:NMF](#CV-NMF)           |          6| 0.906|           0.854|       0.915|*  |2,3,5      |
|[SD:NMF](#SD-NMF)           |          5| 0.905|           0.863|       0.937|*  |2,3        |
|[CV:skmeans](#CV-skmeans)   |          6| 0.905|           0.799|       0.894|*  |2,3,5      |
|[CV:mclust](#CV-mclust)     |          3| 0.891|           0.882|       0.949|   |           |
|[MAD:hclust](#MAD-hclust)   |          3| 0.887|           0.952|       0.979|   |           |
|[MAD:mclust](#MAD-mclust)   |          3| 0.747|           0.857|       0.935|   |           |
|[ATC:mclust](#ATC-mclust)   |          3| 0.743|           0.819|       0.916|   |           |
|[CV:hclust](#CV-hclust)     |          3| 0.650|           0.875|       0.917|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 1.000           0.966       0.986          0.386 0.618   0.618
#&gt; CV:NMF      2 1.000           0.951       0.983          0.382 0.618   0.618
#&gt; MAD:NMF     2 1.000           0.980       0.991          0.395 0.599   0.599
#&gt; ATC:NMF     2 0.919           0.924       0.968          0.441 0.566   0.566
#&gt; SD:skmeans  2 0.753           0.811       0.916          0.441 0.517   0.517
#&gt; CV:skmeans  2 0.964           0.970       0.985          0.410 0.599   0.599
#&gt; MAD:skmeans 2 0.720           0.890       0.950          0.434 0.581   0.581
#&gt; ATC:skmeans 2 1.000           0.933       0.974          0.439 0.581   0.581
#&gt; SD:mclust   2 0.699           0.935       0.958          0.466 0.509   0.509
#&gt; CV:mclust   2 0.699           0.743       0.879          0.426 0.509   0.509
#&gt; MAD:mclust  2 0.656           0.758       0.897          0.422 0.638   0.638
#&gt; ATC:mclust  2 0.629           0.840       0.926          0.419 0.618   0.618
#&gt; SD:kmeans   2 0.509           0.806       0.828          0.339 0.638   0.638
#&gt; CV:kmeans   2 0.393           0.825       0.834          0.355 0.638   0.638
#&gt; MAD:kmeans  2 0.369           0.788       0.812          0.364 0.638   0.638
#&gt; ATC:kmeans  2 0.514           0.747       0.798          0.331 0.660   0.660
#&gt; SD:pam      2 1.000           0.988       0.994          0.383 0.618   0.618
#&gt; CV:pam      2 1.000           0.966       0.988          0.375 0.638   0.638
#&gt; MAD:pam     2 1.000           0.995       0.998          0.385 0.618   0.618
#&gt; ATC:pam     2 1.000           0.979       0.992          0.331 0.660   0.660
#&gt; SD:hclust   2 0.485           0.720       0.805          0.320 0.660   0.660
#&gt; CV:hclust   2 0.494           0.911       0.903          0.351 0.638   0.638
#&gt; MAD:hclust  2 0.591           0.945       0.949          0.347 0.660   0.660
#&gt; ATC:hclust  2 1.000           0.997       0.997          0.293 0.708   0.708
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 1.000           0.997       0.998          0.487 0.765   0.633
#&gt; CV:NMF      3 0.937           0.931       0.974          0.553 0.750   0.610
#&gt; MAD:NMF     3 1.000           0.970       0.988          0.485 0.729   0.576
#&gt; ATC:NMF     3 1.000           0.947       0.982          0.364 0.681   0.502
#&gt; SD:skmeans  3 0.940           0.959       0.982          0.452 0.762   0.571
#&gt; CV:skmeans  3 0.967           0.909       0.964          0.595 0.687   0.501
#&gt; MAD:skmeans 3 0.969           0.912       0.966          0.508 0.703   0.512
#&gt; ATC:skmeans 3 0.912           0.923       0.967          0.458 0.736   0.567
#&gt; SD:mclust   3 0.999           0.964       0.979          0.258 0.919   0.840
#&gt; CV:mclust   3 0.891           0.882       0.949          0.422 0.829   0.686
#&gt; MAD:mclust  3 0.747           0.857       0.935          0.443 0.759   0.623
#&gt; ATC:mclust  3 0.743           0.819       0.916          0.425 0.750   0.601
#&gt; SD:kmeans   3 1.000           0.993       0.984          0.662 0.790   0.670
#&gt; CV:kmeans   3 1.000           0.979       0.969          0.608 0.790   0.670
#&gt; MAD:kmeans  3 1.000           0.971       0.975          0.525 0.790   0.670
#&gt; ATC:kmeans  3 1.000           0.981       0.993          0.625 0.738   0.621
#&gt; SD:pam      3 1.000           0.986       0.994          0.454 0.817   0.706
#&gt; CV:pam      3 0.881           0.941       0.976          0.543 0.790   0.670
#&gt; MAD:pam     3 1.000           0.965       0.986          0.484 0.798   0.676
#&gt; ATC:pam     3 1.000           0.959       0.987          0.591 0.801   0.698
#&gt; SD:hclust   3 0.939           0.908       0.967          0.717 0.783   0.671
#&gt; CV:hclust   3 0.650           0.875       0.917          0.585 0.826   0.727
#&gt; MAD:hclust  3 0.887           0.952       0.979          0.554 0.821   0.728
#&gt; ATC:hclust  3 0.908           0.962       0.983          0.903 0.719   0.604
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.751           0.801       0.871         0.2560 0.861   0.675
#&gt; CV:NMF      4 0.755           0.762       0.867         0.2333 0.854   0.652
#&gt; MAD:NMF     4 0.723           0.780       0.884         0.2428 0.880   0.712
#&gt; ATC:NMF     4 0.713           0.763       0.876         0.1524 0.905   0.767
#&gt; SD:skmeans  4 0.781           0.761       0.880         0.1711 0.848   0.596
#&gt; CV:skmeans  4 0.753           0.643       0.846         0.1527 0.834   0.557
#&gt; MAD:skmeans 4 0.783           0.785       0.901         0.1587 0.845   0.576
#&gt; ATC:skmeans 4 0.807           0.778       0.904         0.1717 0.833   0.571
#&gt; SD:mclust   4 0.736           0.807       0.887         0.2251 0.873   0.704
#&gt; CV:mclust   4 0.740           0.781       0.855         0.2018 0.854   0.661
#&gt; MAD:mclust  4 0.771           0.806       0.894         0.1628 0.848   0.647
#&gt; ATC:mclust  4 0.575           0.653       0.824         0.1702 0.880   0.721
#&gt; SD:kmeans   4 0.728           0.677       0.844         0.2345 0.919   0.810
#&gt; CV:kmeans   4 0.731           0.755       0.828         0.2373 0.873   0.704
#&gt; MAD:kmeans  4 0.696           0.561       0.802         0.2567 0.902   0.771
#&gt; ATC:kmeans  4 0.684           0.704       0.861         0.2173 0.953   0.899
#&gt; SD:pam      4 0.683           0.724       0.875         0.2297 0.898   0.771
#&gt; CV:pam      4 0.659           0.575       0.781         0.2331 0.834   0.612
#&gt; MAD:pam     4 0.744           0.803       0.892         0.2448 0.839   0.633
#&gt; ATC:pam     4 0.665           0.692       0.815         0.2014 0.940   0.873
#&gt; SD:hclust   4 0.751           0.781       0.877         0.1521 0.989   0.976
#&gt; CV:hclust   4 0.651           0.731       0.858         0.2695 0.810   0.590
#&gt; MAD:hclust  4 0.777           0.892       0.924         0.2761 0.857   0.703
#&gt; ATC:hclust  4 0.956           0.930       0.973         0.0889 0.977   0.947
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.905           0.863       0.937         0.1058 0.881   0.611
#&gt; CV:NMF      5 0.923           0.875       0.943         0.1004 0.896   0.639
#&gt; MAD:NMF     5 0.820           0.804       0.898         0.0917 0.881   0.619
#&gt; ATC:NMF     5 0.687           0.587       0.784         0.1124 0.876   0.640
#&gt; SD:skmeans  5 0.935           0.916       0.959         0.0764 0.888   0.594
#&gt; CV:skmeans  5 0.940           0.906       0.955         0.0755 0.904   0.634
#&gt; MAD:skmeans 5 0.922           0.860       0.938         0.0653 0.916   0.674
#&gt; ATC:skmeans 5 0.764           0.690       0.806         0.0592 0.889   0.593
#&gt; SD:mclust   5 0.713           0.540       0.801         0.0596 0.983   0.942
#&gt; CV:mclust   5 0.730           0.651       0.821         0.0827 0.878   0.601
#&gt; MAD:mclust  5 0.685           0.741       0.822         0.0964 0.865   0.583
#&gt; ATC:mclust  5 0.543           0.533       0.718         0.0548 0.860   0.649
#&gt; SD:kmeans   5 0.711           0.838       0.823         0.0977 0.835   0.535
#&gt; CV:kmeans   5 0.722           0.814       0.851         0.0990 0.910   0.702
#&gt; MAD:kmeans  5 0.669           0.747       0.808         0.1037 0.830   0.514
#&gt; ATC:kmeans  5 0.630           0.619       0.778         0.1189 0.851   0.651
#&gt; SD:pam      5 0.741           0.621       0.845         0.1196 0.916   0.758
#&gt; CV:pam      5 0.729           0.614       0.807         0.0847 0.842   0.514
#&gt; MAD:pam     5 0.725           0.627       0.817         0.0846 0.913   0.714
#&gt; ATC:pam     5 0.708           0.766       0.889         0.0812 0.894   0.756
#&gt; SD:hclust   5 0.704           0.761       0.870         0.0928 0.919   0.816
#&gt; CV:hclust   5 0.695           0.706       0.817         0.0794 0.907   0.674
#&gt; MAD:hclust  5 0.717           0.611       0.821         0.0898 0.893   0.691
#&gt; ATC:hclust  5 0.915           0.841       0.912         0.0223 0.986   0.966
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.876           0.770       0.888         0.0410 0.956   0.790
#&gt; CV:NMF      6 0.906           0.854       0.915         0.0402 0.952   0.759
#&gt; MAD:NMF     6 0.855           0.729       0.869         0.0450 0.943   0.741
#&gt; ATC:NMF     6 0.721           0.644       0.794         0.0456 0.931   0.736
#&gt; SD:skmeans  6 0.885           0.847       0.911         0.0372 0.953   0.765
#&gt; CV:skmeans  6 0.905           0.799       0.894         0.0357 0.943   0.714
#&gt; MAD:skmeans 6 0.859           0.767       0.866         0.0358 0.953   0.765
#&gt; ATC:skmeans 6 0.782           0.557       0.773         0.0342 0.914   0.637
#&gt; SD:mclust   6 0.740           0.502       0.736         0.0259 0.840   0.499
#&gt; CV:mclust   6 0.698           0.525       0.711         0.0280 0.950   0.749
#&gt; MAD:mclust  6 0.725           0.642       0.740         0.0430 0.934   0.720
#&gt; ATC:mclust  6 0.604           0.509       0.721         0.0479 0.864   0.598
#&gt; SD:kmeans   6 0.726           0.768       0.831         0.0659 1.000   1.000
#&gt; CV:kmeans   6 0.814           0.771       0.811         0.0555 1.000   1.000
#&gt; MAD:kmeans  6 0.746           0.669       0.812         0.0557 0.992   0.958
#&gt; ATC:kmeans  6 0.665           0.640       0.782         0.0622 0.899   0.659
#&gt; SD:pam      6 0.745           0.689       0.830         0.0611 0.885   0.590
#&gt; CV:pam      6 0.821           0.768       0.890         0.0600 0.894   0.589
#&gt; MAD:pam     6 0.787           0.789       0.828         0.0606 0.854   0.497
#&gt; ATC:pam     6 0.727           0.722       0.876         0.0254 0.971   0.913
#&gt; SD:hclust   6 0.834           0.792       0.894         0.1295 0.873   0.654
#&gt; CV:hclust   6 0.784           0.797       0.836         0.0674 0.954   0.781
#&gt; MAD:hclust  6 0.764           0.654       0.825         0.0655 0.902   0.637
#&gt; ATC:hclust  6 0.926           0.877       0.938         0.0356 0.958   0.892
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      51     0.395 2
#&gt; CV:NMF      50     0.394 2
#&gt; MAD:NMF     52     0.396 2
#&gt; ATC:NMF     51     0.395 2
#&gt; SD:skmeans  43     0.386 2
#&gt; CV:skmeans  52     0.396 2
#&gt; MAD:skmeans 52     0.396 2
#&gt; ATC:skmeans 49     0.393 2
#&gt; SD:mclust   52     0.396 2
#&gt; CV:mclust   41     0.383 2
#&gt; MAD:mclust  42     0.384 2
#&gt; ATC:mclust  50     0.394 2
#&gt; SD:kmeans   43     0.386 2
#&gt; CV:kmeans   52     0.396 2
#&gt; MAD:kmeans  52     0.396 2
#&gt; ATC:kmeans  42     0.384 2
#&gt; SD:pam      52     0.396 2
#&gt; CV:pam      51     0.395 2
#&gt; MAD:pam     52     0.396 2
#&gt; ATC:pam     51     0.395 2
#&gt; SD:hclust   42     0.384 2
#&gt; CV:hclust   52     0.396 2
#&gt; MAD:hclust  51     0.395 2
#&gt; ATC:hclust  52     0.396 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      52     0.372 3
#&gt; CV:NMF      51     0.371 3
#&gt; MAD:NMF     51     0.371 3
#&gt; ATC:NMF     50     0.370 3
#&gt; SD:skmeans  52     0.450 3
#&gt; CV:skmeans  49     0.446 3
#&gt; MAD:skmeans 49     0.446 3
#&gt; ATC:skmeans 50     0.370 3
#&gt; SD:mclust   52     0.372 3
#&gt; CV:mclust   49     0.368 3
#&gt; MAD:mclust  49     0.368 3
#&gt; ATC:mclust  47     0.366 3
#&gt; SD:kmeans   52     0.372 3
#&gt; CV:kmeans   52     0.372 3
#&gt; MAD:kmeans  51     0.371 3
#&gt; ATC:kmeans  52     0.372 3
#&gt; SD:pam      52     0.372 3
#&gt; CV:pam      52     0.372 3
#&gt; MAD:pam     51     0.371 3
#&gt; ATC:pam     51     0.371 3
#&gt; SD:hclust   49     0.368 3
#&gt; CV:hclust   50     0.370 3
#&gt; MAD:hclust  51     0.371 3
#&gt; ATC:hclust  52     0.372 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      50     0.454 4
#&gt; CV:NMF      48     0.451 4
#&gt; MAD:NMF     47     0.462 4
#&gt; ATC:NMF     45     0.341 4
#&gt; SD:skmeans  47     0.453 4
#&gt; CV:skmeans  39     0.473 4
#&gt; MAD:skmeans 46     0.412 4
#&gt; ATC:skmeans 47     0.440 4
#&gt; SD:mclust   46     0.447 4
#&gt; CV:mclust   47     0.450 4
#&gt; MAD:mclust  48     0.462 4
#&gt; ATC:mclust  39     0.405 4
#&gt; SD:kmeans   44     0.500 4
#&gt; CV:kmeans   47     0.450 4
#&gt; MAD:kmeans  23     0.389 4
#&gt; ATC:kmeans  45     0.363 4
#&gt; SD:pam      48     0.508 4
#&gt; CV:pam      24     0.392 4
#&gt; MAD:pam     48     0.448 4
#&gt; ATC:pam     38     0.351 4
#&gt; SD:hclust   44     0.410 4
#&gt; CV:hclust   44     0.497 4
#&gt; MAD:hclust  51     0.453 4
#&gt; ATC:hclust  51     0.371 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      48     0.425 5
#&gt; CV:NMF      49     0.436 5
#&gt; MAD:NMF     47     0.424 5
#&gt; ATC:NMF     34     0.398 5
#&gt; SD:skmeans  51     0.442 5
#&gt; CV:skmeans  51     0.437 5
#&gt; MAD:skmeans 48     0.449 5
#&gt; ATC:skmeans 43     0.400 5
#&gt; SD:mclust   35     0.456 5
#&gt; CV:mclust   38     0.417 5
#&gt; MAD:mclust  42     0.428 5
#&gt; ATC:mclust  30     0.403 5
#&gt; SD:kmeans   51     0.434 5
#&gt; CV:kmeans   50     0.431 5
#&gt; MAD:kmeans  44     0.451 5
#&gt; ATC:kmeans  40     0.332 5
#&gt; SD:pam      34     0.388 5
#&gt; CV:pam      34     0.398 5
#&gt; MAD:pam     41     0.488 5
#&gt; ATC:pam     48     0.328 5
#&gt; SD:hclust   46     0.498 5
#&gt; CV:hclust   42     0.499 5
#&gt; MAD:hclust  40     0.414 5
#&gt; ATC:hclust  50     0.349 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n tissue(p) k
#&gt; SD:NMF      43     0.425 6
#&gt; CV:NMF      50     0.422 6
#&gt; MAD:NMF     41     0.423 6
#&gt; ATC:NMF     39     0.431 6
#&gt; SD:skmeans  49     0.436 6
#&gt; CV:skmeans  43     0.436 6
#&gt; MAD:skmeans 43     0.441 6
#&gt; ATC:skmeans 29     0.379 6
#&gt; SD:mclust   28     0.388 6
#&gt; CV:mclust   26     0.384 6
#&gt; MAD:mclust  42     0.452 6
#&gt; ATC:mclust  24     0.392 6
#&gt; SD:kmeans   49     0.428 6
#&gt; CV:kmeans   46     0.431 6
#&gt; MAD:kmeans  41     0.501 6
#&gt; ATC:kmeans  42     0.423 6
#&gt; SD:pam      41     0.492 6
#&gt; CV:pam      45     0.475 6
#&gt; MAD:pam     47     0.484 6
#&gt; ATC:pam     47     0.326 6
#&gt; SD:hclust   47     0.471 6
#&gt; CV:hclust   48     0.398 6
#&gt; MAD:hclust  35     0.394 6
#&gt; ATC:hclust  50     0.331 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.485           0.720       0.805         0.3205 0.660   0.660
#> 3 3 0.939           0.908       0.967         0.7169 0.783   0.671
#> 4 4 0.751           0.781       0.877         0.1521 0.989   0.976
#> 5 5 0.704           0.761       0.870         0.0928 0.919   0.816
#> 6 6 0.834           0.792       0.894         0.1295 0.873   0.654
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.770 1.000 0.000
#&gt; GSM28789     1  0.0000      0.770 1.000 0.000
#&gt; GSM28790     1  0.0000      0.770 1.000 0.000
#&gt; GSM11300     1  0.9522      0.434 0.628 0.372
#&gt; GSM28798     2  1.0000      1.000 0.496 0.504
#&gt; GSM11296     2  1.0000      1.000 0.496 0.504
#&gt; GSM28801     2  1.0000      1.000 0.496 0.504
#&gt; GSM11319     2  1.0000      1.000 0.496 0.504
#&gt; GSM28781     2  1.0000      1.000 0.496 0.504
#&gt; GSM11305     2  1.0000      1.000 0.496 0.504
#&gt; GSM28784     2  1.0000      1.000 0.496 0.504
#&gt; GSM11307     2  1.0000      1.000 0.496 0.504
#&gt; GSM11313     2  1.0000      1.000 0.496 0.504
#&gt; GSM28785     2  1.0000      1.000 0.496 0.504
#&gt; GSM11318     1  0.0000      0.770 1.000 0.000
#&gt; GSM28792     1  0.0000      0.770 1.000 0.000
#&gt; GSM11295     1  0.9998      0.362 0.508 0.492
#&gt; GSM28793     1  0.0000      0.770 1.000 0.000
#&gt; GSM11312     1  0.0000      0.770 1.000 0.000
#&gt; GSM28778     1  0.0000      0.770 1.000 0.000
#&gt; GSM28796     1  0.0000      0.770 1.000 0.000
#&gt; GSM11309     1  0.0376      0.767 0.996 0.004
#&gt; GSM11315     1  0.0000      0.770 1.000 0.000
#&gt; GSM11306     1  0.0000      0.770 1.000 0.000
#&gt; GSM28776     1  0.0000      0.770 1.000 0.000
#&gt; GSM28777     1  1.0000      0.360 0.504 0.496
#&gt; GSM11316     1  1.0000      0.360 0.504 0.496
#&gt; GSM11320     1  1.0000      0.360 0.504 0.496
#&gt; GSM28797     1  0.0376      0.767 0.996 0.004
#&gt; GSM28786     1  0.0376      0.767 0.996 0.004
#&gt; GSM28800     1  0.0000      0.770 1.000 0.000
#&gt; GSM11310     1  0.0000      0.770 1.000 0.000
#&gt; GSM28787     1  0.9998      0.362 0.508 0.492
#&gt; GSM11304     1  0.2236      0.737 0.964 0.036
#&gt; GSM11303     1  1.0000      0.360 0.504 0.496
#&gt; GSM11317     1  1.0000      0.360 0.504 0.496
#&gt; GSM11311     1  0.0376      0.767 0.996 0.004
#&gt; GSM28799     1  0.0000      0.770 1.000 0.000
#&gt; GSM28791     1  0.0000      0.770 1.000 0.000
#&gt; GSM28794     1  0.9686     -0.748 0.604 0.396
#&gt; GSM28780     1  0.0000      0.770 1.000 0.000
#&gt; GSM28795     1  0.0000      0.770 1.000 0.000
#&gt; GSM11301     2  1.0000      1.000 0.496 0.504
#&gt; GSM11297     1  0.2236      0.737 0.964 0.036
#&gt; GSM11298     1  0.0000      0.770 1.000 0.000
#&gt; GSM11314     1  0.0000      0.770 1.000 0.000
#&gt; GSM11299     1  0.9522      0.434 0.628 0.372
#&gt; GSM28783     1  0.0000      0.770 1.000 0.000
#&gt; GSM11308     1  0.0000      0.770 1.000 0.000
#&gt; GSM28782     1  0.0000      0.770 1.000 0.000
#&gt; GSM28779     1  0.0000      0.770 1.000 0.000
#&gt; GSM11302     1  0.0000      0.770 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11300     3  0.6509      0.185 0.472 0.004 0.524
#&gt; GSM28798     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11296     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM28801     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11319     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM28781     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11305     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM28784     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11307     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11313     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM28785     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11318     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11295     3  0.0424      0.822 0.008 0.000 0.992
#&gt; GSM28793     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11309     1  0.0237      0.970 0.996 0.004 0.000
#&gt; GSM11315     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.824 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.824 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.824 0.000 0.000 1.000
#&gt; GSM28797     1  0.0237      0.970 0.996 0.004 0.000
#&gt; GSM28786     1  0.0237      0.970 0.996 0.004 0.000
#&gt; GSM28800     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28787     3  0.0424      0.822 0.008 0.000 0.992
#&gt; GSM11304     1  0.3784      0.815 0.864 0.004 0.132
#&gt; GSM11303     3  0.0000      0.824 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.824 0.000 0.000 1.000
#&gt; GSM11311     1  0.0237      0.970 0.996 0.004 0.000
#&gt; GSM28799     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28794     1  0.6225      0.249 0.568 0.432 0.000
#&gt; GSM28780     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11301     2  0.0237      1.000 0.004 0.996 0.000
#&gt; GSM11297     1  0.3784      0.815 0.864 0.004 0.132
#&gt; GSM11298     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11299     3  0.6509      0.185 0.472 0.004 0.524
#&gt; GSM28783     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.973 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.1118      0.819 0.964 0.000 0.000 0.036
#&gt; GSM28789     1  0.1118      0.819 0.964 0.000 0.000 0.036
#&gt; GSM28790     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11300     4  0.6096      1.000 0.184 0.000 0.136 0.680
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11295     3  0.0188      0.492 0.000 0.000 0.996 0.004
#&gt; GSM28793     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11312     1  0.0707      0.827 0.980 0.000 0.000 0.020
#&gt; GSM28778     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM28796     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11309     1  0.4522      0.458 0.680 0.000 0.000 0.320
#&gt; GSM11315     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.1118      0.819 0.964 0.000 0.000 0.036
#&gt; GSM28776     1  0.1118      0.819 0.964 0.000 0.000 0.036
#&gt; GSM28777     3  0.4985      0.788 0.000 0.000 0.532 0.468
#&gt; GSM11316     3  0.4985      0.788 0.000 0.000 0.532 0.468
#&gt; GSM11320     3  0.4985      0.788 0.000 0.000 0.532 0.468
#&gt; GSM28797     1  0.4522      0.458 0.680 0.000 0.000 0.320
#&gt; GSM28786     1  0.4522      0.458 0.680 0.000 0.000 0.320
#&gt; GSM28800     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM28787     3  0.0336      0.488 0.000 0.000 0.992 0.008
#&gt; GSM11304     1  0.6739      0.150 0.576 0.000 0.120 0.304
#&gt; GSM11303     3  0.4985      0.788 0.000 0.000 0.532 0.468
#&gt; GSM11317     3  0.4985      0.788 0.000 0.000 0.532 0.468
#&gt; GSM11311     1  0.2149      0.776 0.912 0.000 0.000 0.088
#&gt; GSM28799     1  0.0000      0.830 1.000 0.000 0.000 0.000
#&gt; GSM28791     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM28794     1  0.4933      0.296 0.568 0.432 0.000 0.000
#&gt; GSM28780     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM28795     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.6739      0.150 0.576 0.000 0.120 0.304
#&gt; GSM11298     1  0.0188      0.829 0.996 0.000 0.000 0.004
#&gt; GSM11314     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM11299     4  0.6096      1.000 0.184 0.000 0.136 0.680
#&gt; GSM28783     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM11308     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM28782     1  0.3688      0.738 0.792 0.000 0.000 0.208
#&gt; GSM28779     1  0.0188      0.829 0.996 0.000 0.000 0.004
#&gt; GSM11302     1  0.0188      0.829 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1  0.3452      0.657 0.756 0.000 0.000 0.244 0.000
#&gt; GSM28789     1  0.3452      0.657 0.756 0.000 0.000 0.244 0.000
#&gt; GSM28790     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     3  0.7766      0.101 0.148 0.000 0.412 0.336 0.104
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11295     5  0.3074      0.936 0.000 0.000 0.196 0.000 0.804
#&gt; GSM28793     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     1  0.0898      0.805 0.972 0.000 0.000 0.020 0.008
#&gt; GSM28778     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM28796     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.2074      1.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM11315     1  0.0000      0.806 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.3452      0.657 0.756 0.000 0.000 0.244 0.000
#&gt; GSM28776     1  0.3452      0.657 0.756 0.000 0.000 0.244 0.000
#&gt; GSM28777     3  0.0000      0.704 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.704 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000      0.704 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.2074      1.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM28786     4  0.2074      1.000 0.104 0.000 0.000 0.896 0.000
#&gt; GSM28800     1  0.0290      0.805 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11310     1  0.0290      0.805 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28787     5  0.2843      0.938 0.000 0.000 0.144 0.008 0.848
#&gt; GSM11304     1  0.6305      0.074 0.536 0.000 0.020 0.340 0.104
#&gt; GSM11303     3  0.0000      0.704 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.704 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     1  0.3774      0.448 0.704 0.000 0.000 0.296 0.000
#&gt; GSM28799     1  0.0290      0.805 0.992 0.000 0.000 0.008 0.000
#&gt; GSM28791     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM28794     1  0.4249      0.367 0.568 0.432 0.000 0.000 0.000
#&gt; GSM28780     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM28795     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     1  0.6305      0.074 0.536 0.000 0.020 0.340 0.104
#&gt; GSM11298     1  0.0290      0.805 0.992 0.000 0.000 0.008 0.000
#&gt; GSM11314     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM11299     3  0.7766      0.101 0.148 0.000 0.412 0.336 0.104
#&gt; GSM28783     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM11308     1  0.4457      0.739 0.756 0.000 0.000 0.092 0.152
#&gt; GSM28782     1  0.4350      0.743 0.764 0.000 0.000 0.084 0.152
#&gt; GSM28779     1  0.0510      0.803 0.984 0.000 0.000 0.016 0.000
#&gt; GSM11302     1  0.0404      0.804 0.988 0.000 0.000 0.012 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     1  0.4062      0.568 0.660 0.000 0.000 0.316 0.024 0.000
#&gt; GSM28789     1  0.4062      0.568 0.660 0.000 0.000 0.316 0.024 0.000
#&gt; GSM28790     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     3  0.7520      0.143 0.136 0.000 0.392 0.348 0.048 0.076
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11295     6  0.2378      0.826 0.000 0.000 0.152 0.000 0.000 0.848
#&gt; GSM28793     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     1  0.2060      0.775 0.900 0.000 0.000 0.016 0.084 0.000
#&gt; GSM28778     5  0.1296      0.951 0.044 0.000 0.000 0.004 0.948 0.004
#&gt; GSM28796     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0547      1.000 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM11315     1  0.0000      0.815 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.4062      0.568 0.660 0.000 0.000 0.316 0.024 0.000
#&gt; GSM28776     1  0.4045      0.573 0.664 0.000 0.000 0.312 0.024 0.000
#&gt; GSM28777     3  0.0000      0.727 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.727 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000      0.727 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0547      1.000 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM28786     4  0.0547      1.000 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM28800     1  0.0363      0.815 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM11310     1  0.0363      0.815 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM28787     6  0.0146      0.833 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; GSM11304     1  0.5848      0.189 0.524 0.000 0.000 0.352 0.048 0.076
#&gt; GSM11303     3  0.0000      0.727 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.727 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     1  0.3446      0.510 0.692 0.000 0.000 0.308 0.000 0.000
#&gt; GSM28799     1  0.0363      0.815 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM28791     5  0.1663      0.950 0.088 0.000 0.000 0.000 0.912 0.000
#&gt; GSM28794     1  0.3817      0.323 0.568 0.432 0.000 0.000 0.000 0.000
#&gt; GSM28780     5  0.1387      0.956 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; GSM28795     5  0.1296      0.951 0.044 0.000 0.000 0.004 0.948 0.004
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     1  0.5848      0.189 0.524 0.000 0.000 0.352 0.048 0.076
#&gt; GSM11298     1  0.0260      0.816 0.992 0.000 0.000 0.008 0.000 0.000
#&gt; GSM11314     5  0.1296      0.951 0.044 0.000 0.000 0.004 0.948 0.004
#&gt; GSM11299     3  0.7520      0.143 0.136 0.000 0.392 0.348 0.048 0.076
#&gt; GSM28783     5  0.1814      0.936 0.100 0.000 0.000 0.000 0.900 0.000
#&gt; GSM11308     5  0.1387      0.956 0.068 0.000 0.000 0.000 0.932 0.000
#&gt; GSM28782     5  0.2219      0.896 0.136 0.000 0.000 0.000 0.864 0.000
#&gt; GSM28779     1  0.1176      0.806 0.956 0.000 0.000 0.020 0.024 0.000
#&gt; GSM11302     1  0.1088      0.807 0.960 0.000 0.000 0.016 0.024 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:hclust 42     0.384 2
#> SD:hclust 49     0.368 3
#> SD:hclust 44     0.410 4
#> SD:hclust 46     0.498 5
#> SD:hclust 47     0.471 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.509           0.806       0.828         0.3385 0.638   0.638
#> 3 3 1.000           0.993       0.984         0.6617 0.790   0.670
#> 4 4 0.728           0.677       0.844         0.2345 0.919   0.810
#> 5 5 0.711           0.838       0.823         0.0977 0.835   0.535
#> 6 6 0.726           0.768       0.831         0.0659 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.9795      0.827 0.584 0.416
#&gt; GSM28789     1  0.9795      0.827 0.584 0.416
#&gt; GSM28790     1  0.9795      0.827 0.584 0.416
#&gt; GSM11300     1  0.0000      0.494 1.000 0.000
#&gt; GSM28798     2  0.0000      0.999 0.000 1.000
#&gt; GSM11296     2  0.0000      0.999 0.000 1.000
#&gt; GSM28801     2  0.0000      0.999 0.000 1.000
#&gt; GSM11319     2  0.0000      0.999 0.000 1.000
#&gt; GSM28781     2  0.0000      0.999 0.000 1.000
#&gt; GSM11305     2  0.0000      0.999 0.000 1.000
#&gt; GSM28784     2  0.0000      0.999 0.000 1.000
#&gt; GSM11307     2  0.0000      0.999 0.000 1.000
#&gt; GSM11313     2  0.0000      0.999 0.000 1.000
#&gt; GSM28785     2  0.0000      0.999 0.000 1.000
#&gt; GSM11318     1  0.9795      0.827 0.584 0.416
#&gt; GSM28792     1  0.9795      0.827 0.584 0.416
#&gt; GSM11295     1  0.1184      0.483 0.984 0.016
#&gt; GSM28793     1  0.9795      0.827 0.584 0.416
#&gt; GSM11312     1  0.9795      0.827 0.584 0.416
#&gt; GSM28778     1  0.9795      0.827 0.584 0.416
#&gt; GSM28796     1  0.9795      0.827 0.584 0.416
#&gt; GSM11309     1  0.9635      0.813 0.612 0.388
#&gt; GSM11315     1  0.9795      0.827 0.584 0.416
#&gt; GSM11306     1  0.9795      0.827 0.584 0.416
#&gt; GSM28776     1  0.9795      0.827 0.584 0.416
#&gt; GSM28777     1  0.1184      0.483 0.984 0.016
#&gt; GSM11316     1  0.1184      0.483 0.984 0.016
#&gt; GSM11320     1  0.1184      0.483 0.984 0.016
#&gt; GSM28797     1  0.9635      0.813 0.612 0.388
#&gt; GSM28786     1  0.9635      0.813 0.612 0.388
#&gt; GSM28800     1  0.9795      0.827 0.584 0.416
#&gt; GSM11310     1  0.9795      0.827 0.584 0.416
#&gt; GSM28787     1  0.1184      0.483 0.984 0.016
#&gt; GSM11304     1  0.9635      0.813 0.612 0.388
#&gt; GSM11303     1  0.1184      0.483 0.984 0.016
#&gt; GSM11317     1  0.1184      0.483 0.984 0.016
#&gt; GSM11311     1  0.9754      0.824 0.592 0.408
#&gt; GSM28799     1  0.9754      0.824 0.592 0.408
#&gt; GSM28791     1  0.9795      0.827 0.584 0.416
#&gt; GSM28794     2  0.0672      0.986 0.008 0.992
#&gt; GSM28780     1  0.9795      0.827 0.584 0.416
#&gt; GSM28795     1  0.9795      0.827 0.584 0.416
#&gt; GSM11301     2  0.0000      0.999 0.000 1.000
#&gt; GSM11297     1  0.9661      0.816 0.608 0.392
#&gt; GSM11298     1  0.9795      0.827 0.584 0.416
#&gt; GSM11314     1  0.9795      0.827 0.584 0.416
#&gt; GSM11299     1  0.0000      0.494 1.000 0.000
#&gt; GSM28783     1  0.9795      0.827 0.584 0.416
#&gt; GSM11308     1  0.9732      0.822 0.596 0.404
#&gt; GSM28782     1  0.9795      0.827 0.584 0.416
#&gt; GSM28779     1  0.9795      0.827 0.584 0.416
#&gt; GSM11302     1  0.9795      0.827 0.584 0.416
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0424      0.993 0.992 0.000 0.008
#&gt; GSM28789     1  0.0424      0.993 0.992 0.000 0.008
#&gt; GSM28790     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11300     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM28798     2  0.1585      0.982 0.008 0.964 0.028
#&gt; GSM11296     2  0.0424      0.986 0.008 0.992 0.000
#&gt; GSM28801     2  0.0424      0.986 0.008 0.992 0.000
#&gt; GSM11319     2  0.0424      0.986 0.008 0.992 0.000
#&gt; GSM28781     2  0.0424      0.986 0.008 0.992 0.000
#&gt; GSM11305     2  0.1585      0.982 0.008 0.964 0.028
#&gt; GSM28784     2  0.1170      0.982 0.008 0.976 0.016
#&gt; GSM11307     2  0.1585      0.982 0.008 0.964 0.028
#&gt; GSM11313     2  0.1585      0.982 0.008 0.964 0.028
#&gt; GSM28785     2  0.0424      0.986 0.008 0.992 0.000
#&gt; GSM11318     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11295     3  0.2096      0.998 0.052 0.004 0.944
#&gt; GSM28793     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11309     1  0.0661      0.990 0.988 0.004 0.008
#&gt; GSM11315     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11306     1  0.0424      0.993 0.992 0.000 0.008
#&gt; GSM28776     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28777     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM11316     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM11320     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM28797     1  0.0661      0.990 0.988 0.004 0.008
#&gt; GSM28786     1  0.0661      0.990 0.988 0.004 0.008
#&gt; GSM28800     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28787     3  0.2096      0.998 0.052 0.004 0.944
#&gt; GSM11304     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11303     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM11317     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM11311     1  0.0661      0.990 0.988 0.004 0.008
#&gt; GSM28799     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28794     2  0.2569      0.957 0.032 0.936 0.032
#&gt; GSM28780     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11301     2  0.1170      0.982 0.008 0.976 0.016
#&gt; GSM11297     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11299     3  0.1860      0.999 0.052 0.000 0.948
#&gt; GSM28783     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.998 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.998 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.4998     -0.730 0.512 0.000 0.000 0.488
#&gt; GSM28789     1  0.4998     -0.730 0.512 0.000 0.000 0.488
#&gt; GSM28790     1  0.3172      0.641 0.840 0.000 0.000 0.160
#&gt; GSM11300     3  0.0524      0.982 0.004 0.000 0.988 0.008
#&gt; GSM28798     2  0.1824      0.956 0.000 0.936 0.004 0.060
#&gt; GSM11296     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0188      0.964 0.000 0.996 0.000 0.004
#&gt; GSM11319     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.1824      0.956 0.000 0.936 0.004 0.060
#&gt; GSM28784     2  0.0921      0.958 0.000 0.972 0.000 0.028
#&gt; GSM11307     2  0.1824      0.956 0.000 0.936 0.004 0.060
#&gt; GSM11313     2  0.1824      0.956 0.000 0.936 0.004 0.060
#&gt; GSM28785     2  0.0000      0.965 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.3444      0.635 0.816 0.000 0.000 0.184
#&gt; GSM28792     1  0.1557      0.614 0.944 0.000 0.000 0.056
#&gt; GSM11295     3  0.1824      0.960 0.004 0.000 0.936 0.060
#&gt; GSM28793     1  0.1557      0.614 0.944 0.000 0.000 0.056
#&gt; GSM11312     1  0.3873      0.631 0.772 0.000 0.000 0.228
#&gt; GSM28778     1  0.4250      0.612 0.724 0.000 0.000 0.276
#&gt; GSM28796     1  0.1557      0.614 0.944 0.000 0.000 0.056
#&gt; GSM11309     4  0.4955      0.941 0.444 0.000 0.000 0.556
#&gt; GSM11315     1  0.1557      0.614 0.944 0.000 0.000 0.056
#&gt; GSM11306     1  0.5000     -0.745 0.504 0.000 0.000 0.496
#&gt; GSM28776     1  0.2973      0.422 0.856 0.000 0.000 0.144
#&gt; GSM28777     3  0.0188      0.984 0.004 0.000 0.996 0.000
#&gt; GSM11316     3  0.0524      0.985 0.004 0.000 0.988 0.008
#&gt; GSM11320     3  0.0524      0.985 0.004 0.000 0.988 0.008
#&gt; GSM28797     4  0.4955      0.941 0.444 0.000 0.000 0.556
#&gt; GSM28786     4  0.4955      0.941 0.444 0.000 0.000 0.556
#&gt; GSM28800     1  0.0000      0.624 1.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.3873      0.158 0.772 0.000 0.000 0.228
#&gt; GSM28787     3  0.2125      0.952 0.004 0.000 0.920 0.076
#&gt; GSM11304     1  0.3123      0.420 0.844 0.000 0.000 0.156
#&gt; GSM11303     3  0.0524      0.985 0.004 0.000 0.988 0.008
#&gt; GSM11317     3  0.0524      0.985 0.004 0.000 0.988 0.008
#&gt; GSM11311     4  0.4977      0.815 0.460 0.000 0.000 0.540
#&gt; GSM28799     1  0.3356      0.343 0.824 0.000 0.000 0.176
#&gt; GSM28791     1  0.3975      0.626 0.760 0.000 0.000 0.240
#&gt; GSM28794     2  0.4296      0.870 0.060 0.824 0.004 0.112
#&gt; GSM28780     1  0.4193      0.618 0.732 0.000 0.000 0.268
#&gt; GSM28795     1  0.4277      0.609 0.720 0.000 0.000 0.280
#&gt; GSM11301     2  0.1389      0.950 0.000 0.952 0.000 0.048
#&gt; GSM11297     1  0.3172      0.411 0.840 0.000 0.000 0.160
#&gt; GSM11298     1  0.1637      0.616 0.940 0.000 0.000 0.060
#&gt; GSM11314     1  0.4277      0.609 0.720 0.000 0.000 0.280
#&gt; GSM11299     3  0.0524      0.982 0.004 0.000 0.988 0.008
#&gt; GSM28783     1  0.4193      0.618 0.732 0.000 0.000 0.268
#&gt; GSM11308     1  0.4193      0.618 0.732 0.000 0.000 0.268
#&gt; GSM28782     1  0.3649      0.635 0.796 0.000 0.000 0.204
#&gt; GSM28779     1  0.0921      0.627 0.972 0.000 0.000 0.028
#&gt; GSM11302     1  0.1022      0.632 0.968 0.000 0.000 0.032
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.6084      0.772 0.208 0.000 0.000 0.572 0.220
#&gt; GSM28789     4  0.6084      0.772 0.208 0.000 0.000 0.572 0.220
#&gt; GSM28790     1  0.4288      0.760 0.612 0.000 0.000 0.004 0.384
#&gt; GSM11300     3  0.1568      0.933 0.020 0.000 0.944 0.036 0.000
#&gt; GSM28798     2  0.2645      0.916 0.068 0.888 0.000 0.044 0.000
#&gt; GSM11296     2  0.0162      0.930 0.004 0.996 0.000 0.000 0.000
#&gt; GSM28801     2  0.0451      0.929 0.008 0.988 0.000 0.004 0.000
#&gt; GSM11319     2  0.0000      0.931 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0162      0.930 0.004 0.996 0.000 0.000 0.000
#&gt; GSM11305     2  0.2645      0.916 0.068 0.888 0.000 0.044 0.000
#&gt; GSM28784     2  0.2149      0.905 0.048 0.916 0.000 0.036 0.000
#&gt; GSM11307     2  0.2645      0.916 0.068 0.888 0.000 0.044 0.000
#&gt; GSM11313     2  0.2645      0.916 0.068 0.888 0.000 0.044 0.000
#&gt; GSM28785     2  0.0000      0.931 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4367      0.686 0.580 0.000 0.000 0.004 0.416
#&gt; GSM28792     1  0.4066      0.826 0.672 0.000 0.000 0.004 0.324
#&gt; GSM11295     3  0.3702      0.872 0.084 0.000 0.820 0.096 0.000
#&gt; GSM28793     1  0.4066      0.826 0.672 0.000 0.000 0.004 0.324
#&gt; GSM11312     5  0.2852      0.685 0.172 0.000 0.000 0.000 0.828
#&gt; GSM28778     5  0.0000      0.899 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28796     1  0.4066      0.826 0.672 0.000 0.000 0.004 0.324
#&gt; GSM11309     4  0.4113      0.827 0.140 0.000 0.000 0.784 0.076
#&gt; GSM11315     1  0.4066      0.826 0.672 0.000 0.000 0.004 0.324
#&gt; GSM11306     4  0.6035      0.773 0.204 0.000 0.000 0.580 0.216
#&gt; GSM28776     1  0.5873      0.749 0.564 0.000 0.000 0.124 0.312
#&gt; GSM28777     3  0.0566      0.944 0.012 0.000 0.984 0.004 0.000
#&gt; GSM11316     3  0.0451      0.947 0.008 0.000 0.988 0.004 0.000
#&gt; GSM11320     3  0.0451      0.947 0.008 0.000 0.988 0.004 0.000
#&gt; GSM28797     4  0.4113      0.827 0.140 0.000 0.000 0.784 0.076
#&gt; GSM28786     4  0.4113      0.827 0.140 0.000 0.000 0.784 0.076
#&gt; GSM28800     1  0.4639      0.792 0.612 0.000 0.000 0.020 0.368
#&gt; GSM11310     1  0.5887      0.709 0.596 0.000 0.000 0.164 0.240
#&gt; GSM28787     3  0.4459      0.846 0.104 0.000 0.780 0.104 0.012
#&gt; GSM11304     1  0.6152      0.655 0.524 0.000 0.000 0.152 0.324
#&gt; GSM11303     3  0.0451      0.947 0.008 0.000 0.988 0.004 0.000
#&gt; GSM11317     3  0.0451      0.947 0.008 0.000 0.988 0.004 0.000
#&gt; GSM11311     4  0.4817      0.688 0.300 0.000 0.000 0.656 0.044
#&gt; GSM28799     1  0.5923      0.725 0.572 0.000 0.000 0.140 0.288
#&gt; GSM28791     5  0.0671      0.892 0.016 0.000 0.000 0.004 0.980
#&gt; GSM28794     2  0.5849      0.766 0.160 0.688 0.000 0.084 0.068
#&gt; GSM28780     5  0.0798      0.894 0.016 0.000 0.000 0.008 0.976
#&gt; GSM28795     5  0.0000      0.899 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11301     2  0.2645      0.891 0.068 0.888 0.000 0.044 0.000
#&gt; GSM11297     1  0.6152      0.655 0.524 0.000 0.000 0.152 0.324
#&gt; GSM11298     1  0.4047      0.825 0.676 0.000 0.000 0.004 0.320
#&gt; GSM11314     5  0.0404      0.894 0.012 0.000 0.000 0.000 0.988
#&gt; GSM11299     3  0.1830      0.928 0.028 0.000 0.932 0.040 0.000
#&gt; GSM28783     5  0.0955      0.884 0.028 0.000 0.000 0.004 0.968
#&gt; GSM11308     5  0.1281      0.881 0.032 0.000 0.000 0.012 0.956
#&gt; GSM28782     5  0.3700      0.472 0.240 0.000 0.000 0.008 0.752
#&gt; GSM28779     1  0.4592      0.814 0.644 0.000 0.000 0.024 0.332
#&gt; GSM11302     1  0.3966      0.820 0.664 0.000 0.000 0.000 0.336
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28788     4  0.6310      0.675 0.068 0.000 0.000 0.556 0.152 NA
#&gt; GSM28789     4  0.6310      0.675 0.068 0.000 0.000 0.556 0.152 NA
#&gt; GSM28790     1  0.0713      0.762 0.972 0.000 0.000 0.000 0.028 NA
#&gt; GSM11300     3  0.3043      0.832 0.000 0.000 0.828 0.012 0.012 NA
#&gt; GSM28798     2  0.2300      0.869 0.000 0.856 0.000 0.000 0.000 NA
#&gt; GSM11296     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28801     2  0.1082      0.880 0.000 0.956 0.000 0.000 0.004 NA
#&gt; GSM11319     2  0.0146      0.889 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM28781     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11305     2  0.2300      0.869 0.000 0.856 0.000 0.000 0.000 NA
#&gt; GSM28784     2  0.2278      0.844 0.000 0.868 0.000 0.000 0.004 NA
#&gt; GSM11307     2  0.2300      0.869 0.000 0.856 0.000 0.000 0.000 NA
#&gt; GSM11313     2  0.2300      0.869 0.000 0.856 0.000 0.000 0.000 NA
#&gt; GSM28785     2  0.0000      0.890 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11318     1  0.0790      0.759 0.968 0.000 0.000 0.000 0.032 NA
#&gt; GSM28792     1  0.0458      0.770 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11295     3  0.4138      0.793 0.000 0.000 0.752 0.020 0.044 NA
#&gt; GSM28793     1  0.0458      0.770 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11312     5  0.5628      0.448 0.276 0.000 0.000 0.008 0.560 NA
#&gt; GSM28778     5  0.2741      0.849 0.092 0.000 0.000 0.008 0.868 NA
#&gt; GSM28796     1  0.0458      0.770 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11309     4  0.0891      0.765 0.024 0.000 0.000 0.968 0.008 NA
#&gt; GSM11315     1  0.0458      0.770 0.984 0.000 0.000 0.000 0.016 NA
#&gt; GSM11306     4  0.6339      0.671 0.068 0.000 0.000 0.552 0.156 NA
#&gt; GSM28776     1  0.5644      0.680 0.640 0.000 0.000 0.100 0.064 NA
#&gt; GSM28777     3  0.0665      0.898 0.000 0.000 0.980 0.004 0.008 NA
#&gt; GSM11316     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11320     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM28797     4  0.0891      0.765 0.024 0.000 0.000 0.968 0.008 NA
#&gt; GSM28786     4  0.0891      0.765 0.024 0.000 0.000 0.968 0.008 NA
#&gt; GSM28800     1  0.4717      0.721 0.704 0.000 0.000 0.032 0.056 NA
#&gt; GSM11310     1  0.5735      0.672 0.620 0.000 0.000 0.124 0.048 NA
#&gt; GSM28787     3  0.4731      0.739 0.000 0.000 0.684 0.020 0.060 NA
#&gt; GSM11304     1  0.6581      0.533 0.472 0.000 0.000 0.088 0.112 NA
#&gt; GSM11303     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11317     3  0.0000      0.900 0.000 0.000 1.000 0.000 0.000 NA
#&gt; GSM11311     4  0.2994      0.626 0.208 0.000 0.000 0.788 0.004 NA
#&gt; GSM28799     1  0.5847      0.663 0.604 0.000 0.000 0.092 0.068 NA
#&gt; GSM28791     5  0.2170      0.851 0.100 0.000 0.000 0.000 0.888 NA
#&gt; GSM28794     2  0.5845      0.485 0.028 0.488 0.000 0.004 0.084 NA
#&gt; GSM28780     5  0.2301      0.849 0.096 0.000 0.000 0.000 0.884 NA
#&gt; GSM28795     5  0.2588      0.849 0.092 0.000 0.000 0.008 0.876 NA
#&gt; GSM11301     2  0.2669      0.825 0.000 0.836 0.000 0.000 0.008 NA
#&gt; GSM11297     1  0.6581      0.533 0.472 0.000 0.000 0.088 0.112 NA
#&gt; GSM11298     1  0.0508      0.770 0.984 0.000 0.000 0.000 0.012 NA
#&gt; GSM11314     5  0.2510      0.846 0.080 0.000 0.000 0.008 0.884 NA
#&gt; GSM11299     3  0.3394      0.804 0.000 0.000 0.788 0.012 0.012 NA
#&gt; GSM28783     5  0.2264      0.838 0.096 0.000 0.000 0.004 0.888 NA
#&gt; GSM11308     5  0.2383      0.843 0.096 0.000 0.000 0.000 0.880 NA
#&gt; GSM28782     5  0.4962      0.255 0.416 0.000 0.000 0.000 0.516 NA
#&gt; GSM28779     1  0.4701      0.722 0.712 0.000 0.000 0.036 0.056 NA
#&gt; GSM11302     1  0.3249      0.743 0.824 0.000 0.000 0.004 0.044 NA
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:kmeans 43     0.386 2
#> SD:kmeans 52     0.372 3
#> SD:kmeans 44     0.500 4
#> SD:kmeans 51     0.434 5
#> SD:kmeans 49     0.428 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.753           0.811       0.916         0.4414 0.517   0.517
#> 3 3 0.940           0.959       0.982         0.4516 0.762   0.571
#> 4 4 0.781           0.761       0.880         0.1711 0.848   0.596
#> 5 5 0.935           0.916       0.959         0.0764 0.888   0.594
#> 6 6 0.885           0.847       0.911         0.0372 0.953   0.765
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.2423     0.9270 0.960 0.040
#&gt; GSM28789     2  0.9998     0.0464 0.492 0.508
#&gt; GSM28790     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11300     1  0.0376     0.9709 0.996 0.004
#&gt; GSM28798     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11296     2  0.0000     0.7769 0.000 1.000
#&gt; GSM28801     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11319     2  0.0000     0.7769 0.000 1.000
#&gt; GSM28781     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11305     2  0.0000     0.7769 0.000 1.000
#&gt; GSM28784     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11307     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11313     2  0.0000     0.7769 0.000 1.000
#&gt; GSM28785     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11318     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28792     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11295     2  0.9977     0.3848 0.472 0.528
#&gt; GSM28793     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11312     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28778     1  0.9963     0.0102 0.536 0.464
#&gt; GSM28796     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11309     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11315     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11306     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28776     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28777     2  0.9977     0.3848 0.472 0.528
#&gt; GSM11316     2  0.9963     0.3962 0.464 0.536
#&gt; GSM11320     2  0.9977     0.3848 0.472 0.528
#&gt; GSM28797     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28786     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28800     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11310     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28787     2  0.9977     0.3848 0.472 0.528
#&gt; GSM11304     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11303     2  0.9977     0.3848 0.472 0.528
#&gt; GSM11317     2  0.9977     0.3848 0.472 0.528
#&gt; GSM11311     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28799     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28791     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28794     2  0.0000     0.7769 0.000 1.000
#&gt; GSM28780     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28795     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11301     2  0.0000     0.7769 0.000 1.000
#&gt; GSM11297     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11298     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11314     1  0.3733     0.8797 0.928 0.072
#&gt; GSM11299     1  0.0376     0.9709 0.996 0.004
#&gt; GSM28783     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11308     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28782     1  0.0000     0.9750 1.000 0.000
#&gt; GSM28779     1  0.0000     0.9750 1.000 0.000
#&gt; GSM11302     1  0.0000     0.9750 1.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1   0.177      0.958 0.960 0.024 0.016
#&gt; GSM28789     2   0.575      0.577 0.296 0.700 0.004
#&gt; GSM28790     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11300     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM28798     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11296     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM28801     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11319     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM28781     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11305     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM28784     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11307     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11313     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM28785     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11318     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28792     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11295     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM28793     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11312     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28778     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28796     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11309     3   0.382      0.839 0.148 0.000 0.852
#&gt; GSM11315     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11306     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28776     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28777     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11316     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11320     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM28797     3   0.382      0.839 0.148 0.000 0.852
#&gt; GSM28786     3   0.369      0.846 0.140 0.000 0.860
#&gt; GSM28800     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11310     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28787     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11304     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11303     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11317     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11311     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28799     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28791     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28794     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM28780     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28795     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11301     2   0.000      0.970 0.000 1.000 0.000
#&gt; GSM11297     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM11298     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11314     1   0.129      0.963 0.968 0.000 0.032
#&gt; GSM11299     3   0.000      0.959 0.000 0.000 1.000
#&gt; GSM28783     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11308     1   0.334      0.857 0.880 0.000 0.120
#&gt; GSM28782     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM28779     1   0.000      0.992 1.000 0.000 0.000
#&gt; GSM11302     1   0.000      0.992 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.0469      0.660 0.012 0.000 0.000 0.988
#&gt; GSM28789     4  0.2081      0.607 0.000 0.084 0.000 0.916
#&gt; GSM28790     1  0.0469      0.693 0.988 0.000 0.000 0.012
#&gt; GSM11300     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.691 1.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11295     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11312     1  0.3726      0.624 0.788 0.000 0.000 0.212
#&gt; GSM28778     1  0.4643      0.502 0.656 0.000 0.000 0.344
#&gt; GSM28796     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11309     4  0.2466      0.673 0.004 0.000 0.096 0.900
#&gt; GSM11315     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11306     4  0.0469      0.657 0.012 0.000 0.000 0.988
#&gt; GSM28776     4  0.4996      0.271 0.484 0.000 0.000 0.516
#&gt; GSM28777     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.2466      0.673 0.004 0.000 0.096 0.900
#&gt; GSM28786     4  0.2999      0.657 0.004 0.000 0.132 0.864
#&gt; GSM28800     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11310     4  0.4948      0.374 0.440 0.000 0.000 0.560
#&gt; GSM28787     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11304     3  0.0592      0.983 0.000 0.000 0.984 0.016
#&gt; GSM11303     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.4164      0.569 0.264 0.000 0.000 0.736
#&gt; GSM28799     4  0.4941      0.378 0.436 0.000 0.000 0.564
#&gt; GSM28791     1  0.3528      0.631 0.808 0.000 0.000 0.192
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28780     1  0.4564      0.522 0.672 0.000 0.000 0.328
#&gt; GSM28795     1  0.4643      0.502 0.656 0.000 0.000 0.344
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11297     3  0.1109      0.968 0.004 0.000 0.968 0.028
#&gt; GSM11298     1  0.3074      0.683 0.848 0.000 0.000 0.152
#&gt; GSM11314     4  0.5861     -0.213 0.476 0.000 0.032 0.492
#&gt; GSM11299     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM28783     1  0.4543      0.527 0.676 0.000 0.000 0.324
#&gt; GSM11308     1  0.5792      0.497 0.648 0.000 0.056 0.296
#&gt; GSM28782     1  0.1474      0.684 0.948 0.000 0.000 0.052
#&gt; GSM28779     1  0.3123      0.679 0.844 0.000 0.000 0.156
#&gt; GSM11302     1  0.3074      0.683 0.848 0.000 0.000 0.152
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.0162      0.947 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28789     4  0.0162      0.947 0.000 0.000 0.000 0.996 0.004
#&gt; GSM28790     1  0.0703      0.897 0.976 0.000 0.000 0.000 0.024
#&gt; GSM11300     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0880      0.892 0.968 0.000 0.000 0.000 0.032
#&gt; GSM28792     1  0.0162      0.901 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11295     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     1  0.0162      0.901 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11312     5  0.3209      0.786 0.180 0.000 0.000 0.008 0.812
#&gt; GSM28778     5  0.0290      0.942 0.008 0.000 0.000 0.000 0.992
#&gt; GSM28796     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0162      0.947 0.000 0.000 0.004 0.996 0.000
#&gt; GSM11315     1  0.0000      0.900 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.0671      0.938 0.004 0.000 0.000 0.980 0.016
#&gt; GSM28776     1  0.4114      0.641 0.712 0.000 0.000 0.272 0.016
#&gt; GSM28777     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.0000      0.947 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28786     4  0.0162      0.947 0.000 0.000 0.004 0.996 0.000
#&gt; GSM28800     1  0.1121      0.882 0.956 0.000 0.000 0.000 0.044
#&gt; GSM11310     1  0.4184      0.622 0.700 0.000 0.000 0.284 0.016
#&gt; GSM28787     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11304     3  0.2673      0.901 0.020 0.000 0.900 0.036 0.044
#&gt; GSM11303     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.3534      0.633 0.256 0.000 0.000 0.744 0.000
#&gt; GSM28799     1  0.4734      0.442 0.604 0.000 0.000 0.372 0.024
#&gt; GSM28791     5  0.0162      0.944 0.004 0.000 0.000 0.000 0.996
#&gt; GSM28794     2  0.0162      0.996 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28780     5  0.0000      0.944 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28795     5  0.0000      0.944 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     3  0.4452      0.795 0.048 0.000 0.796 0.104 0.052
#&gt; GSM11298     1  0.0579      0.900 0.984 0.000 0.000 0.008 0.008
#&gt; GSM11314     5  0.1012      0.926 0.000 0.000 0.012 0.020 0.968
#&gt; GSM11299     3  0.0000      0.972 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28783     5  0.0000      0.944 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11308     5  0.0000      0.944 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28782     5  0.2773      0.813 0.164 0.000 0.000 0.000 0.836
#&gt; GSM28779     1  0.0693      0.900 0.980 0.000 0.000 0.008 0.012
#&gt; GSM11302     1  0.0693      0.900 0.980 0.000 0.000 0.008 0.012
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.1970      0.819 0.000 0.000 0.000 0.900 0.008 0.092
#&gt; GSM28789     4  0.1970      0.819 0.000 0.000 0.000 0.900 0.008 0.092
#&gt; GSM28790     1  0.0405      0.903 0.988 0.000 0.000 0.000 0.008 0.004
#&gt; GSM11300     3  0.1556      0.921 0.000 0.000 0.920 0.000 0.000 0.080
#&gt; GSM28798     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11307     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.998 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0291      0.908 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; GSM28792     1  0.0146      0.909 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11295     3  0.0603      0.961 0.000 0.000 0.980 0.000 0.004 0.016
#&gt; GSM28793     1  0.0146      0.909 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11312     5  0.5598      0.540 0.108 0.000 0.000 0.044 0.628 0.220
#&gt; GSM28778     5  0.0260      0.883 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28796     1  0.0260      0.909 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11309     4  0.1588      0.843 0.000 0.000 0.004 0.924 0.000 0.072
#&gt; GSM11315     1  0.0260      0.909 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11306     4  0.1913      0.801 0.000 0.000 0.000 0.908 0.012 0.080
#&gt; GSM28776     6  0.6193      0.236 0.360 0.000 0.000 0.228 0.008 0.404
#&gt; GSM28777     3  0.0000      0.969 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.969 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0146      0.969 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28797     4  0.1471      0.844 0.000 0.000 0.004 0.932 0.000 0.064
#&gt; GSM28786     4  0.1588      0.843 0.000 0.000 0.004 0.924 0.000 0.072
#&gt; GSM28800     6  0.3445      0.568 0.260 0.000 0.000 0.008 0.000 0.732
#&gt; GSM11310     6  0.4154      0.654 0.144 0.000 0.000 0.112 0.000 0.744
#&gt; GSM28787     3  0.0692      0.958 0.000 0.000 0.976 0.000 0.004 0.020
#&gt; GSM11304     6  0.4248      0.549 0.008 0.000 0.220 0.036 0.008 0.728
#&gt; GSM11303     3  0.0146      0.969 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11317     3  0.0146      0.969 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11311     4  0.4810      0.477 0.292 0.000 0.000 0.624 0.000 0.084
#&gt; GSM28799     6  0.3815      0.659 0.132 0.000 0.000 0.092 0.000 0.776
#&gt; GSM28791     5  0.1176      0.879 0.024 0.000 0.000 0.000 0.956 0.020
#&gt; GSM28794     2  0.0520      0.986 0.000 0.984 0.000 0.008 0.000 0.008
#&gt; GSM28780     5  0.0858      0.883 0.004 0.000 0.000 0.000 0.968 0.028
#&gt; GSM28795     5  0.0260      0.883 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM11301     2  0.0146      0.996 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11297     6  0.4085      0.581 0.012 0.000 0.176 0.040 0.008 0.764
#&gt; GSM11298     1  0.0935      0.893 0.964 0.000 0.000 0.004 0.000 0.032
#&gt; GSM11314     5  0.0951      0.877 0.000 0.000 0.004 0.008 0.968 0.020
#&gt; GSM11299     3  0.1957      0.892 0.000 0.000 0.888 0.000 0.000 0.112
#&gt; GSM28783     5  0.1411      0.876 0.000 0.000 0.000 0.004 0.936 0.060
#&gt; GSM11308     5  0.1531      0.874 0.004 0.000 0.000 0.000 0.928 0.068
#&gt; GSM28782     5  0.4895      0.574 0.108 0.000 0.000 0.000 0.636 0.256
#&gt; GSM28779     1  0.4359      0.448 0.652 0.000 0.000 0.028 0.008 0.312
#&gt; GSM11302     1  0.3280      0.741 0.808 0.000 0.000 0.028 0.004 0.160
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> SD:skmeans 43     0.386 2
#> SD:skmeans 52     0.450 3
#> SD:skmeans 47     0.453 4
#> SD:skmeans 51     0.442 5
#> SD:skmeans 49     0.436 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.988       0.994         0.3835 0.618   0.618
#> 3 3 1.000           0.986       0.994         0.4541 0.817   0.706
#> 4 4 0.683           0.724       0.875         0.2297 0.898   0.771
#> 5 5 0.741           0.621       0.845         0.1196 0.916   0.758
#> 6 6 0.745           0.689       0.830         0.0611 0.885   0.590
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.000      0.996 1.000 0.000
#&gt; GSM28789     1   0.000      0.996 1.000 0.000
#&gt; GSM28790     1   0.000      0.996 1.000 0.000
#&gt; GSM11300     1   0.000      0.996 1.000 0.000
#&gt; GSM28798     2   0.000      0.989 0.000 1.000
#&gt; GSM11296     2   0.000      0.989 0.000 1.000
#&gt; GSM28801     2   0.000      0.989 0.000 1.000
#&gt; GSM11319     2   0.000      0.989 0.000 1.000
#&gt; GSM28781     2   0.000      0.989 0.000 1.000
#&gt; GSM11305     2   0.000      0.989 0.000 1.000
#&gt; GSM28784     2   0.000      0.989 0.000 1.000
#&gt; GSM11307     2   0.000      0.989 0.000 1.000
#&gt; GSM11313     2   0.000      0.989 0.000 1.000
#&gt; GSM28785     2   0.000      0.989 0.000 1.000
#&gt; GSM11318     1   0.000      0.996 1.000 0.000
#&gt; GSM28792     1   0.000      0.996 1.000 0.000
#&gt; GSM11295     1   0.000      0.996 1.000 0.000
#&gt; GSM28793     1   0.000      0.996 1.000 0.000
#&gt; GSM11312     1   0.000      0.996 1.000 0.000
#&gt; GSM28778     1   0.000      0.996 1.000 0.000
#&gt; GSM28796     1   0.000      0.996 1.000 0.000
#&gt; GSM11309     1   0.000      0.996 1.000 0.000
#&gt; GSM11315     1   0.000      0.996 1.000 0.000
#&gt; GSM11306     1   0.000      0.996 1.000 0.000
#&gt; GSM28776     1   0.000      0.996 1.000 0.000
#&gt; GSM28777     1   0.000      0.996 1.000 0.000
#&gt; GSM11316     2   0.552      0.852 0.128 0.872
#&gt; GSM11320     1   0.000      0.996 1.000 0.000
#&gt; GSM28797     1   0.000      0.996 1.000 0.000
#&gt; GSM28786     1   0.000      0.996 1.000 0.000
#&gt; GSM28800     1   0.000      0.996 1.000 0.000
#&gt; GSM11310     1   0.000      0.996 1.000 0.000
#&gt; GSM28787     1   0.000      0.996 1.000 0.000
#&gt; GSM11304     1   0.000      0.996 1.000 0.000
#&gt; GSM11303     1   0.204      0.964 0.968 0.032
#&gt; GSM11317     1   0.574      0.842 0.864 0.136
#&gt; GSM11311     1   0.000      0.996 1.000 0.000
#&gt; GSM28799     1   0.000      0.996 1.000 0.000
#&gt; GSM28791     1   0.000      0.996 1.000 0.000
#&gt; GSM28794     2   0.000      0.989 0.000 1.000
#&gt; GSM28780     1   0.000      0.996 1.000 0.000
#&gt; GSM28795     1   0.000      0.996 1.000 0.000
#&gt; GSM11301     2   0.000      0.989 0.000 1.000
#&gt; GSM11297     1   0.000      0.996 1.000 0.000
#&gt; GSM11298     1   0.000      0.996 1.000 0.000
#&gt; GSM11314     1   0.000      0.996 1.000 0.000
#&gt; GSM11299     1   0.000      0.996 1.000 0.000
#&gt; GSM28783     1   0.000      0.996 1.000 0.000
#&gt; GSM11308     1   0.000      0.996 1.000 0.000
#&gt; GSM28782     1   0.000      0.996 1.000 0.000
#&gt; GSM28779     1   0.000      0.996 1.000 0.000
#&gt; GSM11302     1   0.000      0.996 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28786     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28800     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28787     1  0.4555      0.750 0.800 0.000 0.200
#&gt; GSM11304     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11303     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.999 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28794     2  0.2625      0.870 0.084 0.916 0.000
#&gt; GSM28780     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      0.989 0.000 1.000 0.000
#&gt; GSM11297     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11299     3  0.0237      0.994 0.004 0.000 0.996
#&gt; GSM28783     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.993 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.993 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.4955    -0.4017 0.556 0.000 0.000 0.444
#&gt; GSM28789     1  0.4790    -0.2239 0.620 0.000 0.000 0.380
#&gt; GSM28790     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM11300     3  0.3873     0.7678 0.000 0.000 0.772 0.228
#&gt; GSM28798     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM28792     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM11295     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM11312     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM28778     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM28796     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM11309     4  0.2973     0.5568 0.144 0.000 0.000 0.856
#&gt; GSM11315     1  0.3172     0.7106 0.840 0.000 0.000 0.160
#&gt; GSM11306     4  0.4981     0.5978 0.464 0.000 0.000 0.536
#&gt; GSM28776     1  0.2408     0.7348 0.896 0.000 0.000 0.104
#&gt; GSM28777     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.4643     0.7021 0.344 0.000 0.000 0.656
#&gt; GSM28786     4  0.4406     0.7073 0.300 0.000 0.000 0.700
#&gt; GSM28800     1  0.4713     0.5492 0.640 0.000 0.000 0.360
#&gt; GSM11310     1  0.2011     0.7235 0.920 0.000 0.000 0.080
#&gt; GSM28787     1  0.4630     0.4417 0.768 0.000 0.196 0.036
#&gt; GSM11304     1  0.4843     0.5235 0.604 0.000 0.000 0.396
#&gt; GSM11303     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9299 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.4996     0.0619 0.484 0.000 0.000 0.516
#&gt; GSM28799     1  0.3942     0.5748 0.764 0.000 0.000 0.236
#&gt; GSM28791     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM28794     2  0.3219     0.7377 0.164 0.836 0.000 0.000
#&gt; GSM28780     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM28795     1  0.3444     0.6269 0.816 0.000 0.000 0.184
#&gt; GSM11301     2  0.0000     0.9801 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.4843     0.5235 0.604 0.000 0.000 0.396
#&gt; GSM11298     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM11314     1  0.1474     0.7474 0.948 0.000 0.000 0.052
#&gt; GSM11299     3  0.4122     0.7556 0.004 0.000 0.760 0.236
#&gt; GSM28783     1  0.1302     0.7323 0.956 0.000 0.000 0.044
#&gt; GSM11308     1  0.3837     0.5901 0.776 0.000 0.000 0.224
#&gt; GSM28782     1  0.0592     0.7432 0.984 0.000 0.000 0.016
#&gt; GSM28779     1  0.0000     0.7475 1.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.7475 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1  0.4909  -0.045348 0.560 0.000 0.000 0.412 0.028
#&gt; GSM28789     1  0.4620   0.038436 0.592 0.000 0.000 0.392 0.016
#&gt; GSM28790     1  0.4430   0.403083 0.540 0.000 0.000 0.004 0.456
#&gt; GSM11300     5  0.4287   0.180135 0.000 0.000 0.460 0.000 0.540
#&gt; GSM28798     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4430   0.403083 0.540 0.000 0.000 0.004 0.456
#&gt; GSM28792     1  0.4434   0.400483 0.536 0.000 0.000 0.004 0.460
#&gt; GSM11295     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     1  0.4434   0.400483 0.536 0.000 0.000 0.004 0.460
#&gt; GSM11312     1  0.0000   0.660729 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28778     1  0.0000   0.660729 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28796     1  0.4434   0.400483 0.536 0.000 0.000 0.004 0.460
#&gt; GSM11309     4  0.0162   0.704071 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11315     1  0.4434   0.400483 0.536 0.000 0.000 0.004 0.460
#&gt; GSM11306     4  0.4294   0.116864 0.468 0.000 0.000 0.532 0.000
#&gt; GSM28776     1  0.4135   0.489466 0.656 0.000 0.000 0.004 0.340
#&gt; GSM28777     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.0162   0.703447 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28786     4  0.0162   0.704071 0.000 0.000 0.000 0.996 0.004
#&gt; GSM28800     5  0.1965   0.546415 0.096 0.000 0.000 0.000 0.904
#&gt; GSM11310     1  0.3707   0.422755 0.716 0.000 0.000 0.000 0.284
#&gt; GSM28787     1  0.5354   0.345833 0.668 0.000 0.192 0.000 0.140
#&gt; GSM11304     5  0.0000   0.554744 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11303     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000   1.000000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.4404   0.430915 0.264 0.000 0.000 0.704 0.032
#&gt; GSM28799     1  0.4262  -0.000992 0.560 0.000 0.000 0.000 0.440
#&gt; GSM28791     1  0.0000   0.660729 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28794     2  0.4138   0.321019 0.384 0.616 0.000 0.000 0.000
#&gt; GSM28780     1  0.0290   0.660489 0.992 0.000 0.000 0.000 0.008
#&gt; GSM28795     1  0.0794   0.655442 0.972 0.000 0.000 0.000 0.028
#&gt; GSM11301     2  0.0000   0.952320 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     5  0.0000   0.554744 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11298     1  0.0162   0.660639 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11314     1  0.2230   0.631429 0.884 0.000 0.000 0.000 0.116
#&gt; GSM11299     5  0.4287   0.180135 0.000 0.000 0.460 0.000 0.540
#&gt; GSM28783     1  0.2605   0.568305 0.852 0.000 0.000 0.000 0.148
#&gt; GSM11308     5  0.4287   0.073284 0.460 0.000 0.000 0.000 0.540
#&gt; GSM28782     1  0.2074   0.610884 0.896 0.000 0.000 0.000 0.104
#&gt; GSM28779     1  0.0000   0.660729 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000   0.660729 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     5  0.3554     0.6416 0.108 0.000 0.000 0.080 0.808 0.004
#&gt; GSM28789     5  0.3598     0.6395 0.112 0.000 0.000 0.080 0.804 0.004
#&gt; GSM28790     1  0.4819     0.8832 0.664 0.000 0.000 0.000 0.132 0.204
#&gt; GSM11300     6  0.2969     0.6057 0.000 0.000 0.224 0.000 0.000 0.776
#&gt; GSM28798     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4819     0.8832 0.664 0.000 0.000 0.000 0.132 0.204
#&gt; GSM28792     1  0.4783     0.8843 0.668 0.000 0.000 0.000 0.128 0.204
#&gt; GSM11295     3  0.2454     0.7815 0.160 0.000 0.840 0.000 0.000 0.000
#&gt; GSM28793     1  0.4957     0.8587 0.648 0.000 0.000 0.000 0.148 0.204
#&gt; GSM11312     5  0.3390     0.6117 0.296 0.000 0.000 0.000 0.704 0.000
#&gt; GSM28778     5  0.1765     0.7455 0.096 0.000 0.000 0.000 0.904 0.000
#&gt; GSM28796     1  0.4707     0.8828 0.676 0.000 0.000 0.000 0.120 0.204
#&gt; GSM11309     4  0.0000     0.6610 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11315     1  0.4707     0.8828 0.676 0.000 0.000 0.000 0.120 0.204
#&gt; GSM11306     4  0.5959    -0.0937 0.224 0.000 0.000 0.416 0.360 0.000
#&gt; GSM28776     1  0.3563     0.0868 0.664 0.000 0.000 0.000 0.336 0.000
#&gt; GSM28777     3  0.0000     0.9590 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9590 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9590 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0000     0.6610 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28786     4  0.0000     0.6610 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28800     6  0.4503     0.4532 0.192 0.000 0.000 0.000 0.108 0.700
#&gt; GSM11310     5  0.4787     0.3850 0.432 0.000 0.000 0.000 0.516 0.052
#&gt; GSM28787     6  0.7586     0.2045 0.224 0.000 0.188 0.000 0.240 0.348
#&gt; GSM11304     6  0.0146     0.6493 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM11303     3  0.0000     0.9590 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9590 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.5873    -0.1415 0.376 0.000 0.000 0.492 0.104 0.028
#&gt; GSM28799     6  0.5377     0.4466 0.272 0.000 0.000 0.000 0.156 0.572
#&gt; GSM28791     5  0.0713     0.7474 0.028 0.000 0.000 0.000 0.972 0.000
#&gt; GSM28794     2  0.3817     0.1683 0.000 0.568 0.000 0.000 0.432 0.000
#&gt; GSM28780     5  0.4377    -0.0737 0.024 0.000 0.000 0.000 0.540 0.436
#&gt; GSM28795     5  0.4905     0.4471 0.096 0.000 0.000 0.000 0.620 0.284
#&gt; GSM11301     2  0.0000     0.9500 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.0146     0.6493 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM11298     5  0.1556     0.7523 0.080 0.000 0.000 0.000 0.920 0.000
#&gt; GSM11314     5  0.4468     0.4747 0.408 0.000 0.000 0.000 0.560 0.032
#&gt; GSM11299     6  0.2854     0.6166 0.000 0.000 0.208 0.000 0.000 0.792
#&gt; GSM28783     5  0.2985     0.7406 0.100 0.000 0.000 0.000 0.844 0.056
#&gt; GSM11308     6  0.3266     0.5618 0.000 0.000 0.000 0.000 0.272 0.728
#&gt; GSM28782     5  0.1010     0.7366 0.004 0.000 0.000 0.000 0.960 0.036
#&gt; GSM28779     5  0.1444     0.7532 0.072 0.000 0.000 0.000 0.928 0.000
#&gt; GSM11302     5  0.1387     0.7538 0.068 0.000 0.000 0.000 0.932 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:pam 52     0.396 2
#> SD:pam 52     0.372 3
#> SD:pam 48     0.508 4
#> SD:pam 34     0.388 5
#> SD:pam 41     0.492 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.699           0.935       0.958         0.4658 0.509   0.509
#> 3 3 0.999           0.964       0.979         0.2579 0.919   0.840
#> 4 4 0.736           0.807       0.887         0.2251 0.873   0.704
#> 5 5 0.713           0.540       0.801         0.0596 0.983   0.942
#> 6 6 0.740           0.502       0.736         0.0259 0.840   0.499
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.000      0.994 1.000 0.000
#&gt; GSM28789     1   0.000      0.994 1.000 0.000
#&gt; GSM28790     1   0.000      0.994 1.000 0.000
#&gt; GSM11300     2   0.697      0.854 0.188 0.812
#&gt; GSM28798     2   0.000      0.892 0.000 1.000
#&gt; GSM11296     2   0.000      0.892 0.000 1.000
#&gt; GSM28801     2   0.000      0.892 0.000 1.000
#&gt; GSM11319     2   0.000      0.892 0.000 1.000
#&gt; GSM28781     2   0.000      0.892 0.000 1.000
#&gt; GSM11305     2   0.000      0.892 0.000 1.000
#&gt; GSM28784     2   0.000      0.892 0.000 1.000
#&gt; GSM11307     2   0.000      0.892 0.000 1.000
#&gt; GSM11313     2   0.000      0.892 0.000 1.000
#&gt; GSM28785     2   0.000      0.892 0.000 1.000
#&gt; GSM11318     1   0.000      0.994 1.000 0.000
#&gt; GSM28792     1   0.000      0.994 1.000 0.000
#&gt; GSM11295     2   0.697      0.854 0.188 0.812
#&gt; GSM28793     1   0.000      0.994 1.000 0.000
#&gt; GSM11312     1   0.000      0.994 1.000 0.000
#&gt; GSM28778     1   0.000      0.994 1.000 0.000
#&gt; GSM28796     1   0.000      0.994 1.000 0.000
#&gt; GSM11309     1   0.000      0.994 1.000 0.000
#&gt; GSM11315     1   0.000      0.994 1.000 0.000
#&gt; GSM11306     1   0.000      0.994 1.000 0.000
#&gt; GSM28776     1   0.000      0.994 1.000 0.000
#&gt; GSM28777     2   0.697      0.854 0.188 0.812
#&gt; GSM11316     2   0.697      0.854 0.188 0.812
#&gt; GSM11320     2   0.697      0.854 0.188 0.812
#&gt; GSM28797     1   0.000      0.994 1.000 0.000
#&gt; GSM28786     1   0.000      0.994 1.000 0.000
#&gt; GSM28800     1   0.000      0.994 1.000 0.000
#&gt; GSM11310     1   0.000      0.994 1.000 0.000
#&gt; GSM28787     2   0.697      0.854 0.188 0.812
#&gt; GSM11304     1   0.625      0.781 0.844 0.156
#&gt; GSM11303     2   0.697      0.854 0.188 0.812
#&gt; GSM11317     2   0.697      0.854 0.188 0.812
#&gt; GSM11311     1   0.000      0.994 1.000 0.000
#&gt; GSM28799     1   0.000      0.994 1.000 0.000
#&gt; GSM28791     1   0.000      0.994 1.000 0.000
#&gt; GSM28794     2   0.900      0.543 0.316 0.684
#&gt; GSM28780     1   0.000      0.994 1.000 0.000
#&gt; GSM28795     1   0.000      0.994 1.000 0.000
#&gt; GSM11301     2   0.000      0.892 0.000 1.000
#&gt; GSM11297     1   0.000      0.994 1.000 0.000
#&gt; GSM11298     1   0.000      0.994 1.000 0.000
#&gt; GSM11314     1   0.000      0.994 1.000 0.000
#&gt; GSM11299     2   0.697      0.854 0.188 0.812
#&gt; GSM28783     1   0.000      0.994 1.000 0.000
#&gt; GSM11308     1   0.000      0.994 1.000 0.000
#&gt; GSM28782     1   0.000      0.994 1.000 0.000
#&gt; GSM28779     1   0.000      0.994 1.000 0.000
#&gt; GSM11302     1   0.000      0.994 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.2939      0.926 0.916 0.012 0.072
#&gt; GSM28789     1  0.4370      0.888 0.868 0.056 0.076
#&gt; GSM28790     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28778     1  0.1411      0.952 0.964 0.000 0.036
#&gt; GSM28796     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11309     1  0.2356      0.932 0.928 0.000 0.072
#&gt; GSM11315     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11306     1  0.2774      0.928 0.920 0.008 0.072
#&gt; GSM28776     1  0.0424      0.961 0.992 0.008 0.000
#&gt; GSM28777     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28797     1  0.2356      0.932 0.928 0.000 0.072
#&gt; GSM28786     1  0.2356      0.932 0.928 0.000 0.072
#&gt; GSM28800     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11304     1  0.4750      0.741 0.784 0.000 0.216
#&gt; GSM11303     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11311     1  0.2584      0.934 0.928 0.008 0.064
#&gt; GSM28799     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28794     2  0.1647      0.952 0.004 0.960 0.036
#&gt; GSM28780     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      0.996 0.000 1.000 0.000
#&gt; GSM11297     1  0.3412      0.865 0.876 0.000 0.124
#&gt; GSM11298     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11314     1  0.2711      0.921 0.912 0.000 0.088
#&gt; GSM11299     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11308     1  0.0747      0.958 0.984 0.000 0.016
#&gt; GSM28782     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.965 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.965 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4
#&gt; GSM28788     4  0.4277      0.753 0.280 0.00 0.000 0.720
#&gt; GSM28789     4  0.3900      0.810 0.164 0.02 0.000 0.816
#&gt; GSM28790     1  0.0707      0.801 0.980 0.00 0.000 0.020
#&gt; GSM11300     3  0.0188      0.995 0.000 0.00 0.996 0.004
#&gt; GSM28798     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11296     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM28801     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11319     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM28781     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11305     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM28784     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11307     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11313     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM28785     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11318     1  0.1716      0.781 0.936 0.00 0.000 0.064
#&gt; GSM28792     1  0.1792      0.799 0.932 0.00 0.000 0.068
#&gt; GSM11295     3  0.0000      0.997 0.000 0.00 1.000 0.000
#&gt; GSM28793     1  0.1867      0.798 0.928 0.00 0.000 0.072
#&gt; GSM11312     1  0.0000      0.798 1.000 0.00 0.000 0.000
#&gt; GSM28778     1  0.3569      0.668 0.804 0.00 0.000 0.196
#&gt; GSM28796     1  0.1792      0.799 0.932 0.00 0.000 0.068
#&gt; GSM11309     4  0.2149      0.818 0.088 0.00 0.000 0.912
#&gt; GSM11315     1  0.1867      0.797 0.928 0.00 0.000 0.072
#&gt; GSM11306     4  0.4585      0.691 0.332 0.00 0.000 0.668
#&gt; GSM28776     1  0.3444      0.722 0.816 0.00 0.000 0.184
#&gt; GSM28777     3  0.0000      0.997 0.000 0.00 1.000 0.000
#&gt; GSM11316     3  0.0188      0.997 0.000 0.00 0.996 0.004
#&gt; GSM11320     3  0.0188      0.997 0.000 0.00 0.996 0.004
#&gt; GSM28797     4  0.2149      0.818 0.088 0.00 0.000 0.912
#&gt; GSM28786     4  0.2149      0.818 0.088 0.00 0.000 0.912
#&gt; GSM28800     1  0.1716      0.800 0.936 0.00 0.000 0.064
#&gt; GSM11310     1  0.3528      0.664 0.808 0.00 0.000 0.192
#&gt; GSM28787     3  0.0188      0.994 0.004 0.00 0.996 0.000
#&gt; GSM11304     1  0.6991      0.301 0.524 0.00 0.128 0.348
#&gt; GSM11303     3  0.0188      0.997 0.000 0.00 0.996 0.004
#&gt; GSM11317     3  0.0188      0.997 0.000 0.00 0.996 0.004
#&gt; GSM11311     4  0.4730      0.633 0.364 0.00 0.000 0.636
#&gt; GSM28799     1  0.5174      0.474 0.620 0.00 0.012 0.368
#&gt; GSM28791     1  0.2081      0.773 0.916 0.00 0.000 0.084
#&gt; GSM28794     2  0.4907      0.311 0.000 0.58 0.000 0.420
#&gt; GSM28780     1  0.2216      0.773 0.908 0.00 0.000 0.092
#&gt; GSM28795     1  0.2814      0.763 0.868 0.00 0.000 0.132
#&gt; GSM11301     2  0.0000      0.961 0.000 1.00 0.000 0.000
#&gt; GSM11297     1  0.6296      0.367 0.548 0.00 0.064 0.388
#&gt; GSM11298     1  0.1792      0.799 0.932 0.00 0.000 0.068
#&gt; GSM11314     1  0.5055      0.497 0.624 0.00 0.008 0.368
#&gt; GSM11299     3  0.0188      0.995 0.000 0.00 0.996 0.004
#&gt; GSM28783     1  0.2149      0.774 0.912 0.00 0.000 0.088
#&gt; GSM11308     1  0.4855      0.465 0.600 0.00 0.000 0.400
#&gt; GSM28782     1  0.2011      0.774 0.920 0.00 0.000 0.080
#&gt; GSM28779     1  0.2081      0.791 0.916 0.00 0.000 0.084
#&gt; GSM11302     1  0.1792      0.799 0.932 0.00 0.000 0.068
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.3061    0.79454 0.136 0.000 0.000 0.844 0.020
#&gt; GSM28789     4  0.3059    0.79974 0.120 0.008 0.000 0.856 0.016
#&gt; GSM28790     1  0.2127    0.46794 0.892 0.000 0.000 0.000 0.108
#&gt; GSM11300     3  0.3750    0.84035 0.000 0.000 0.756 0.012 0.232
#&gt; GSM28798     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.2471    0.43189 0.864 0.000 0.000 0.000 0.136
#&gt; GSM28792     1  0.1981    0.52687 0.924 0.000 0.000 0.028 0.048
#&gt; GSM11295     3  0.3857    0.81957 0.000 0.000 0.688 0.000 0.312
#&gt; GSM28793     1  0.1830    0.52886 0.924 0.000 0.000 0.068 0.008
#&gt; GSM11312     1  0.2331    0.49801 0.900 0.000 0.000 0.020 0.080
#&gt; GSM28778     1  0.4597    0.15912 0.696 0.000 0.000 0.044 0.260
#&gt; GSM28796     1  0.1386    0.53304 0.952 0.000 0.000 0.032 0.016
#&gt; GSM11309     4  0.1914    0.79824 0.016 0.000 0.000 0.924 0.060
#&gt; GSM11315     1  0.2470    0.51202 0.884 0.000 0.000 0.104 0.012
#&gt; GSM11306     4  0.3690    0.73304 0.200 0.000 0.000 0.780 0.020
#&gt; GSM28776     1  0.4823    0.21409 0.672 0.000 0.000 0.276 0.052
#&gt; GSM28777     3  0.1908    0.86096 0.000 0.000 0.908 0.000 0.092
#&gt; GSM11316     3  0.0162    0.85987 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11320     3  0.0000    0.85985 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.1914    0.79824 0.016 0.000 0.000 0.924 0.060
#&gt; GSM28786     4  0.1914    0.79824 0.016 0.000 0.000 0.924 0.060
#&gt; GSM28800     1  0.1195    0.53362 0.960 0.000 0.000 0.028 0.012
#&gt; GSM11310     1  0.5087    0.18442 0.644 0.000 0.000 0.292 0.064
#&gt; GSM28787     3  0.4713    0.72852 0.000 0.000 0.544 0.016 0.440
#&gt; GSM11304     1  0.7532   -0.23361 0.448 0.000 0.072 0.168 0.312
#&gt; GSM11303     3  0.0000    0.85985 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000    0.85985 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.5255    0.32764 0.388 0.000 0.000 0.560 0.052
#&gt; GSM28799     1  0.5840   -0.00614 0.604 0.000 0.000 0.232 0.164
#&gt; GSM28791     1  0.4150   -0.23553 0.612 0.000 0.000 0.000 0.388
#&gt; GSM28794     2  0.5636    0.56951 0.072 0.680 0.000 0.208 0.040
#&gt; GSM28780     1  0.4235   -0.35935 0.576 0.000 0.000 0.000 0.424
#&gt; GSM28795     1  0.4291   -0.48810 0.536 0.000 0.000 0.000 0.464
#&gt; GSM11301     2  0.0000    0.97134 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     1  0.7291   -0.20908 0.460 0.000 0.044 0.192 0.304
#&gt; GSM11298     1  0.1469    0.53063 0.948 0.000 0.000 0.036 0.016
#&gt; GSM11314     1  0.6392   -0.65756 0.472 0.000 0.012 0.120 0.396
#&gt; GSM11299     3  0.4444    0.79101 0.000 0.000 0.624 0.012 0.364
#&gt; GSM28783     1  0.3837   -0.02753 0.692 0.000 0.000 0.000 0.308
#&gt; GSM11308     5  0.5488    0.00000 0.428 0.000 0.000 0.064 0.508
#&gt; GSM28782     1  0.2773    0.37060 0.836 0.000 0.000 0.000 0.164
#&gt; GSM28779     1  0.2362    0.51559 0.900 0.000 0.000 0.076 0.024
#&gt; GSM11302     1  0.1648    0.53063 0.940 0.000 0.000 0.040 0.020
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     1  0.5595    -0.1549 0.536 0.000 0.000 0.148 0.004 0.312
#&gt; GSM28789     1  0.5317    -0.1113 0.568 0.000 0.000 0.112 0.004 0.316
#&gt; GSM28790     5  0.2762     0.5421 0.196 0.000 0.000 0.000 0.804 0.000
#&gt; GSM11300     3  0.0000     0.7947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28798     2  0.0000     0.9612 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9612 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0146     0.9601 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9612 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9612 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0146     0.9603 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28784     2  0.0146     0.9601 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; GSM11307     2  0.0146     0.9603 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11313     2  0.0146     0.9603 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28785     2  0.0000     0.9612 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     5  0.2048     0.5883 0.120 0.000 0.000 0.000 0.880 0.000
#&gt; GSM28792     5  0.4018     0.1042 0.412 0.000 0.000 0.008 0.580 0.000
#&gt; GSM11295     3  0.1814     0.7395 0.000 0.000 0.900 0.000 0.000 0.100
#&gt; GSM28793     1  0.4929     0.3488 0.556 0.000 0.000 0.024 0.392 0.028
#&gt; GSM11312     5  0.3797     0.0337 0.420 0.000 0.000 0.000 0.580 0.000
#&gt; GSM28778     5  0.2593     0.5702 0.148 0.000 0.008 0.000 0.844 0.000
#&gt; GSM28796     1  0.4002     0.3284 0.588 0.000 0.000 0.008 0.404 0.000
#&gt; GSM11309     4  0.0547     0.9936 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM11315     1  0.6591     0.1636 0.408 0.000 0.000 0.124 0.396 0.072
#&gt; GSM11306     1  0.5317    -0.1077 0.568 0.000 0.000 0.112 0.004 0.316
#&gt; GSM28776     1  0.6269     0.3175 0.456 0.000 0.000 0.076 0.388 0.080
#&gt; GSM28777     3  0.3578     0.1944 0.000 0.000 0.660 0.000 0.000 0.340
#&gt; GSM11316     6  0.3866     0.2508 0.000 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11320     6  0.3866     0.2508 0.000 0.000 0.484 0.000 0.000 0.516
#&gt; GSM28797     4  0.0458     0.9968 0.016 0.000 0.000 0.984 0.000 0.000
#&gt; GSM28786     4  0.0458     0.9968 0.016 0.000 0.000 0.984 0.000 0.000
#&gt; GSM28800     1  0.3804     0.2982 0.576 0.000 0.000 0.000 0.424 0.000
#&gt; GSM11310     5  0.7335    -0.0233 0.160 0.000 0.000 0.164 0.392 0.284
#&gt; GSM28787     3  0.1856     0.7459 0.048 0.000 0.920 0.000 0.000 0.032
#&gt; GSM11304     5  0.6244    -0.2721 0.424 0.000 0.096 0.008 0.432 0.040
#&gt; GSM11303     6  0.3866     0.2508 0.000 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11317     6  0.3866     0.2508 0.000 0.000 0.484 0.000 0.000 0.516
#&gt; GSM11311     6  0.7567    -0.5478 0.256 0.000 0.000 0.152 0.296 0.296
#&gt; GSM28799     1  0.6150     0.3313 0.492 0.000 0.020 0.056 0.384 0.048
#&gt; GSM28791     5  0.2553     0.5996 0.000 0.000 0.000 0.008 0.848 0.144
#&gt; GSM28794     2  0.4631     0.4682 0.364 0.600 0.000 0.008 0.008 0.020
#&gt; GSM28780     5  0.2805     0.5936 0.000 0.000 0.000 0.012 0.828 0.160
#&gt; GSM28795     5  0.3056     0.5949 0.008 0.000 0.000 0.012 0.820 0.160
#&gt; GSM11301     2  0.0260     0.9572 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM11297     1  0.6302     0.2819 0.472 0.000 0.072 0.024 0.392 0.040
#&gt; GSM11298     1  0.3915     0.3408 0.584 0.000 0.000 0.004 0.412 0.000
#&gt; GSM11314     5  0.3610     0.5564 0.136 0.000 0.024 0.020 0.812 0.008
#&gt; GSM11299     3  0.0000     0.7947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28783     5  0.2631     0.5566 0.152 0.000 0.000 0.000 0.840 0.008
#&gt; GSM11308     5  0.3465     0.5948 0.024 0.000 0.000 0.016 0.804 0.156
#&gt; GSM28782     5  0.2373     0.6055 0.084 0.000 0.000 0.004 0.888 0.024
#&gt; GSM28779     1  0.4018     0.3432 0.580 0.000 0.000 0.008 0.412 0.000
#&gt; GSM11302     1  0.3804     0.3192 0.576 0.000 0.000 0.000 0.424 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> SD:mclust 52     0.396 2
#> SD:mclust 52     0.372 3
#> SD:mclust 46     0.447 4
#> SD:mclust 35     0.456 5
#> SD:mclust 28     0.388 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.966       0.986          0.386 0.618   0.618
#> 3 3 1.000           0.997       0.998          0.487 0.765   0.633
#> 4 4 0.751           0.801       0.871          0.256 0.861   0.675
#> 5 5 0.905           0.863       0.937          0.106 0.881   0.611
#> 6 6 0.876           0.770       0.888          0.041 0.956   0.790
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.1184      0.973 0.984 0.016
#&gt; GSM28789     2  0.8327      0.631 0.264 0.736
#&gt; GSM28790     1  0.0000      0.988 1.000 0.000
#&gt; GSM11300     1  0.0000      0.988 1.000 0.000
#&gt; GSM28798     2  0.0000      0.977 0.000 1.000
#&gt; GSM11296     2  0.0000      0.977 0.000 1.000
#&gt; GSM28801     2  0.0000      0.977 0.000 1.000
#&gt; GSM11319     2  0.0000      0.977 0.000 1.000
#&gt; GSM28781     2  0.0000      0.977 0.000 1.000
#&gt; GSM11305     2  0.0000      0.977 0.000 1.000
#&gt; GSM28784     2  0.0000      0.977 0.000 1.000
#&gt; GSM11307     2  0.0000      0.977 0.000 1.000
#&gt; GSM11313     2  0.0000      0.977 0.000 1.000
#&gt; GSM28785     2  0.0000      0.977 0.000 1.000
#&gt; GSM11318     1  0.0000      0.988 1.000 0.000
#&gt; GSM28792     1  0.0000      0.988 1.000 0.000
#&gt; GSM11295     1  0.0000      0.988 1.000 0.000
#&gt; GSM28793     1  0.0000      0.988 1.000 0.000
#&gt; GSM11312     1  0.0000      0.988 1.000 0.000
#&gt; GSM28778     1  0.9170      0.488 0.668 0.332
#&gt; GSM28796     1  0.0000      0.988 1.000 0.000
#&gt; GSM11309     1  0.0000      0.988 1.000 0.000
#&gt; GSM11315     1  0.0000      0.988 1.000 0.000
#&gt; GSM11306     1  0.0000      0.988 1.000 0.000
#&gt; GSM28776     1  0.0000      0.988 1.000 0.000
#&gt; GSM28777     1  0.0000      0.988 1.000 0.000
#&gt; GSM11316     1  0.4690      0.882 0.900 0.100
#&gt; GSM11320     1  0.0000      0.988 1.000 0.000
#&gt; GSM28797     1  0.0000      0.988 1.000 0.000
#&gt; GSM28786     1  0.0000      0.988 1.000 0.000
#&gt; GSM28800     1  0.0000      0.988 1.000 0.000
#&gt; GSM11310     1  0.0000      0.988 1.000 0.000
#&gt; GSM28787     1  0.0376      0.984 0.996 0.004
#&gt; GSM11304     1  0.0000      0.988 1.000 0.000
#&gt; GSM11303     1  0.0000      0.988 1.000 0.000
#&gt; GSM11317     1  0.0000      0.988 1.000 0.000
#&gt; GSM11311     1  0.0000      0.988 1.000 0.000
#&gt; GSM28799     1  0.0000      0.988 1.000 0.000
#&gt; GSM28791     1  0.0000      0.988 1.000 0.000
#&gt; GSM28794     2  0.0000      0.977 0.000 1.000
#&gt; GSM28780     1  0.0000      0.988 1.000 0.000
#&gt; GSM28795     1  0.0000      0.988 1.000 0.000
#&gt; GSM11301     2  0.0000      0.977 0.000 1.000
#&gt; GSM11297     1  0.0000      0.988 1.000 0.000
#&gt; GSM11298     1  0.0000      0.988 1.000 0.000
#&gt; GSM11314     1  0.0000      0.988 1.000 0.000
#&gt; GSM11299     1  0.0000      0.988 1.000 0.000
#&gt; GSM28783     1  0.0000      0.988 1.000 0.000
#&gt; GSM11308     1  0.0000      0.988 1.000 0.000
#&gt; GSM28782     1  0.0000      0.988 1.000 0.000
#&gt; GSM28779     1  0.0000      0.988 1.000 0.000
#&gt; GSM11302     1  0.0000      0.988 1.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28789     1  0.1753      0.949 0.952 0.048 0.000
#&gt; GSM28790     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28786     1  0.0747      0.984 0.984 0.000 0.016
#&gt; GSM28800     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11304     1  0.0592      0.987 0.988 0.000 0.012
#&gt; GSM11303     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28780     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11297     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11314     1  0.0424      0.991 0.992 0.000 0.008
#&gt; GSM11299     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.997 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.0336      0.885 0.008 0.000 0.000 0.992
#&gt; GSM28789     4  0.3048      0.756 0.016 0.108 0.000 0.876
#&gt; GSM28790     1  0.4250      0.702 0.724 0.000 0.000 0.276
#&gt; GSM11300     3  0.0188      0.971 0.000 0.000 0.996 0.004
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.2345      0.696 0.900 0.000 0.000 0.100
#&gt; GSM28792     1  0.4624      0.690 0.660 0.000 0.000 0.340
#&gt; GSM11295     3  0.2300      0.924 0.064 0.000 0.920 0.016
#&gt; GSM28793     1  0.4679      0.683 0.648 0.000 0.000 0.352
#&gt; GSM11312     1  0.3726      0.697 0.788 0.000 0.000 0.212
#&gt; GSM28778     1  0.2216      0.641 0.908 0.000 0.000 0.092
#&gt; GSM28796     1  0.4679      0.683 0.648 0.000 0.000 0.352
#&gt; GSM11309     4  0.0336      0.886 0.008 0.000 0.000 0.992
#&gt; GSM11315     1  0.4713      0.675 0.640 0.000 0.000 0.360
#&gt; GSM11306     4  0.0707      0.877 0.020 0.000 0.000 0.980
#&gt; GSM28776     1  0.4948      0.541 0.560 0.000 0.000 0.440
#&gt; GSM28777     3  0.0188      0.971 0.000 0.000 0.996 0.004
#&gt; GSM11316     3  0.0188      0.971 0.000 0.000 0.996 0.004
#&gt; GSM11320     3  0.0000      0.970 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.0188      0.885 0.004 0.000 0.000 0.996
#&gt; GSM28786     4  0.0336      0.886 0.008 0.000 0.000 0.992
#&gt; GSM28800     1  0.4643      0.687 0.656 0.000 0.000 0.344
#&gt; GSM11310     4  0.4661      0.133 0.348 0.000 0.000 0.652
#&gt; GSM28787     3  0.4150      0.843 0.120 0.000 0.824 0.056
#&gt; GSM11304     1  0.4459      0.692 0.780 0.000 0.032 0.188
#&gt; GSM11303     3  0.0000      0.970 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0188      0.971 0.000 0.000 0.996 0.004
#&gt; GSM11311     4  0.1302      0.854 0.044 0.000 0.000 0.956
#&gt; GSM28799     1  0.4992      0.472 0.524 0.000 0.000 0.476
#&gt; GSM28791     1  0.1211      0.654 0.960 0.000 0.000 0.040
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28780     1  0.2149      0.629 0.912 0.000 0.000 0.088
#&gt; GSM28795     1  0.2530      0.610 0.888 0.000 0.000 0.112
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.5193      0.601 0.580 0.000 0.008 0.412
#&gt; GSM11298     1  0.4643      0.687 0.656 0.000 0.000 0.344
#&gt; GSM11314     1  0.2859      0.604 0.880 0.000 0.008 0.112
#&gt; GSM11299     3  0.0000      0.970 0.000 0.000 1.000 0.000
#&gt; GSM28783     1  0.2647      0.602 0.880 0.000 0.000 0.120
#&gt; GSM11308     1  0.2081      0.632 0.916 0.000 0.000 0.084
#&gt; GSM28782     1  0.1716      0.687 0.936 0.000 0.000 0.064
#&gt; GSM28779     1  0.4643      0.687 0.656 0.000 0.000 0.344
#&gt; GSM11302     1  0.4661      0.686 0.652 0.000 0.000 0.348
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM28788     4  0.0324     0.9860 0.004  0 0.000 0.992 0.004
#&gt; GSM28789     4  0.0324     0.9860 0.004  0 0.000 0.992 0.004
#&gt; GSM28790     1  0.0880     0.8821 0.968  0 0.000 0.000 0.032
#&gt; GSM11300     3  0.0290     0.9429 0.000  0 0.992 0.008 0.000
#&gt; GSM28798     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11318     1  0.0703     0.8844 0.976  0 0.000 0.000 0.024
#&gt; GSM28792     1  0.0404     0.8852 0.988  0 0.000 0.000 0.012
#&gt; GSM11295     3  0.4135     0.4924 0.000  0 0.656 0.004 0.340
#&gt; GSM28793     1  0.0000     0.8866 1.000  0 0.000 0.000 0.000
#&gt; GSM11312     5  0.4709     0.3196 0.364  0 0.000 0.024 0.612
#&gt; GSM28778     5  0.2561     0.7744 0.144  0 0.000 0.000 0.856
#&gt; GSM28796     1  0.0000     0.8866 1.000  0 0.000 0.000 0.000
#&gt; GSM11309     4  0.0162     0.9843 0.000  0 0.000 0.996 0.004
#&gt; GSM11315     1  0.0000     0.8866 1.000  0 0.000 0.000 0.000
#&gt; GSM11306     4  0.0451     0.9842 0.004  0 0.000 0.988 0.008
#&gt; GSM28776     1  0.2236     0.8623 0.908  0 0.000 0.068 0.024
#&gt; GSM28777     3  0.0162     0.9447 0.000  0 0.996 0.004 0.000
#&gt; GSM11316     3  0.0566     0.9387 0.000  0 0.984 0.004 0.012
#&gt; GSM11320     3  0.0000     0.9448 0.000  0 1.000 0.000 0.000
#&gt; GSM28797     4  0.0000     0.9852 0.000  0 0.000 1.000 0.000
#&gt; GSM28786     4  0.0000     0.9852 0.000  0 0.000 1.000 0.000
#&gt; GSM28800     1  0.0671     0.8859 0.980  0 0.000 0.004 0.016
#&gt; GSM11310     1  0.3452     0.7115 0.756  0 0.000 0.244 0.000
#&gt; GSM28787     5  0.4621     0.0648 0.004  0 0.412 0.008 0.576
#&gt; GSM11304     1  0.5418     0.6173 0.684  0 0.092 0.016 0.208
#&gt; GSM11303     3  0.0000     0.9448 0.000  0 1.000 0.000 0.000
#&gt; GSM11317     3  0.0162     0.9447 0.000  0 0.996 0.004 0.000
#&gt; GSM11311     4  0.1197     0.9423 0.048  0 0.000 0.952 0.000
#&gt; GSM28799     1  0.3086     0.7899 0.816  0 0.000 0.180 0.004
#&gt; GSM28791     5  0.1270     0.8437 0.052  0 0.000 0.000 0.948
#&gt; GSM28794     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28780     5  0.0880     0.8508 0.032  0 0.000 0.000 0.968
#&gt; GSM28795     5  0.0510     0.8486 0.016  0 0.000 0.000 0.984
#&gt; GSM11301     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11297     1  0.4301     0.7946 0.800  0 0.052 0.116 0.032
#&gt; GSM11298     1  0.0404     0.8862 0.988  0 0.000 0.000 0.012
#&gt; GSM11314     5  0.0510     0.8486 0.016  0 0.000 0.000 0.984
#&gt; GSM11299     3  0.0000     0.9448 0.000  0 1.000 0.000 0.000
#&gt; GSM28783     5  0.0794     0.8511 0.028  0 0.000 0.000 0.972
#&gt; GSM11308     5  0.0510     0.8486 0.016  0 0.000 0.000 0.984
#&gt; GSM28782     1  0.4306     0.0307 0.508  0 0.000 0.000 0.492
#&gt; GSM28779     1  0.1117     0.8859 0.964  0 0.000 0.016 0.020
#&gt; GSM11302     1  0.1117     0.8864 0.964  0 0.000 0.016 0.020
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.0551     0.9841 0.004 0.004 0.000 0.984 0.000 0.008
#&gt; GSM28789     4  0.0405     0.9842 0.000 0.004 0.000 0.988 0.000 0.008
#&gt; GSM28790     1  0.0972     0.8024 0.964 0.000 0.000 0.000 0.008 0.028
#&gt; GSM11300     3  0.2362     0.7044 0.000 0.000 0.860 0.004 0.000 0.136
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.1049     0.7907 0.960 0.000 0.000 0.000 0.008 0.032
#&gt; GSM28792     1  0.1765     0.7473 0.904 0.000 0.000 0.000 0.000 0.096
#&gt; GSM11295     3  0.6127     0.0867 0.036 0.000 0.432 0.004 0.100 0.428
#&gt; GSM28793     1  0.0547     0.7972 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM11312     5  0.4220     0.6782 0.104 0.000 0.000 0.032 0.776 0.088
#&gt; GSM28778     5  0.1010     0.8604 0.004 0.000 0.000 0.000 0.960 0.036
#&gt; GSM28796     1  0.0713     0.8022 0.972 0.000 0.000 0.000 0.000 0.028
#&gt; GSM11309     4  0.0508     0.9854 0.004 0.000 0.000 0.984 0.000 0.012
#&gt; GSM11315     1  0.0632     0.8018 0.976 0.000 0.000 0.000 0.000 0.024
#&gt; GSM11306     4  0.0603     0.9793 0.004 0.000 0.000 0.980 0.000 0.016
#&gt; GSM28776     1  0.3248     0.6682 0.804 0.000 0.000 0.032 0.000 0.164
#&gt; GSM28777     3  0.2100     0.7348 0.004 0.000 0.884 0.000 0.000 0.112
#&gt; GSM11316     3  0.1075     0.7667 0.000 0.000 0.952 0.000 0.000 0.048
#&gt; GSM11320     3  0.0458     0.7804 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28797     4  0.0146     0.9863 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM28786     4  0.0291     0.9865 0.004 0.000 0.000 0.992 0.000 0.004
#&gt; GSM28800     1  0.4543     0.2903 0.576 0.000 0.000 0.000 0.040 0.384
#&gt; GSM11310     1  0.5812     0.1631 0.488 0.000 0.000 0.172 0.004 0.336
#&gt; GSM28787     6  0.7255    -0.1724 0.068 0.000 0.256 0.016 0.232 0.428
#&gt; GSM11304     6  0.6936     0.4529 0.192 0.000 0.156 0.004 0.136 0.512
#&gt; GSM11303     3  0.0713     0.7777 0.000 0.000 0.972 0.000 0.000 0.028
#&gt; GSM11317     3  0.0146     0.7799 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11311     4  0.0820     0.9741 0.016 0.000 0.000 0.972 0.000 0.012
#&gt; GSM28799     1  0.5305     0.3261 0.564 0.000 0.000 0.108 0.004 0.324
#&gt; GSM28791     5  0.0508     0.8723 0.004 0.000 0.000 0.000 0.984 0.012
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28780     5  0.0363     0.8730 0.000 0.000 0.000 0.000 0.988 0.012
#&gt; GSM28795     5  0.0260     0.8715 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.6880     0.3866 0.248 0.000 0.168 0.024 0.052 0.508
#&gt; GSM11298     1  0.0713     0.8027 0.972 0.000 0.000 0.000 0.000 0.028
#&gt; GSM11314     5  0.1267     0.8373 0.000 0.000 0.000 0.000 0.940 0.060
#&gt; GSM11299     3  0.3847     0.3748 0.000 0.000 0.644 0.000 0.008 0.348
#&gt; GSM28783     5  0.0632     0.8719 0.000 0.000 0.000 0.000 0.976 0.024
#&gt; GSM11308     5  0.0547     0.8710 0.000 0.000 0.000 0.000 0.980 0.020
#&gt; GSM28782     5  0.5450     0.2063 0.164 0.000 0.000 0.000 0.560 0.276
#&gt; GSM28779     1  0.1843     0.7819 0.912 0.000 0.000 0.004 0.004 0.080
#&gt; GSM11302     1  0.1411     0.7906 0.936 0.000 0.000 0.000 0.004 0.060
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> SD:NMF 51     0.395 2
#> SD:NMF 52     0.372 3
#> SD:NMF 50     0.454 4
#> SD:NMF 48     0.425 5
#> SD:NMF 43     0.425 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.494           0.911       0.903         0.3513 0.638   0.638
#> 3 3 0.650           0.875       0.917         0.5853 0.826   0.727
#> 4 4 0.651           0.731       0.858         0.2695 0.810   0.590
#> 5 5 0.695           0.706       0.817         0.0794 0.907   0.674
#> 6 6 0.784           0.797       0.836         0.0674 0.954   0.781
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.6712      0.915 0.824 0.176
#&gt; GSM28789     1  0.6712      0.915 0.824 0.176
#&gt; GSM28790     1  0.6438      0.925 0.836 0.164
#&gt; GSM11300     1  0.0672      0.820 0.992 0.008
#&gt; GSM28798     2  0.3274      0.992 0.060 0.940
#&gt; GSM11296     2  0.3274      0.992 0.060 0.940
#&gt; GSM28801     2  0.3274      0.992 0.060 0.940
#&gt; GSM11319     2  0.3274      0.992 0.060 0.940
#&gt; GSM28781     2  0.3274      0.992 0.060 0.940
#&gt; GSM11305     2  0.3274      0.992 0.060 0.940
#&gt; GSM28784     2  0.3274      0.992 0.060 0.940
#&gt; GSM11307     2  0.3274      0.992 0.060 0.940
#&gt; GSM11313     2  0.3274      0.992 0.060 0.940
#&gt; GSM28785     2  0.3274      0.992 0.060 0.940
#&gt; GSM11318     1  0.6438      0.925 0.836 0.164
#&gt; GSM28792     1  0.6438      0.925 0.836 0.164
#&gt; GSM11295     1  0.3274      0.780 0.940 0.060
#&gt; GSM28793     1  0.6438      0.925 0.836 0.164
#&gt; GSM11312     1  0.6438      0.925 0.836 0.164
#&gt; GSM28778     1  0.6438      0.925 0.836 0.164
#&gt; GSM28796     1  0.6438      0.925 0.836 0.164
#&gt; GSM11309     1  0.6438      0.925 0.836 0.164
#&gt; GSM11315     1  0.6438      0.925 0.836 0.164
#&gt; GSM11306     1  0.6712      0.915 0.824 0.176
#&gt; GSM28776     1  0.6712      0.915 0.824 0.176
#&gt; GSM28777     1  0.3274      0.780 0.940 0.060
#&gt; GSM11316     1  0.3274      0.780 0.940 0.060
#&gt; GSM11320     1  0.3274      0.780 0.940 0.060
#&gt; GSM28797     1  0.6438      0.925 0.836 0.164
#&gt; GSM28786     1  0.6438      0.925 0.836 0.164
#&gt; GSM28800     1  0.6438      0.925 0.836 0.164
#&gt; GSM11310     1  0.6438      0.925 0.836 0.164
#&gt; GSM28787     1  0.3274      0.780 0.940 0.060
#&gt; GSM11304     1  0.0000      0.826 1.000 0.000
#&gt; GSM11303     1  0.3274      0.780 0.940 0.060
#&gt; GSM11317     1  0.3274      0.780 0.940 0.060
#&gt; GSM11311     1  0.6438      0.925 0.836 0.164
#&gt; GSM28799     1  0.6438      0.925 0.836 0.164
#&gt; GSM28791     1  0.6438      0.925 0.836 0.164
#&gt; GSM28794     2  0.5519      0.904 0.128 0.872
#&gt; GSM28780     1  0.6438      0.925 0.836 0.164
#&gt; GSM28795     1  0.6438      0.925 0.836 0.164
#&gt; GSM11301     2  0.3274      0.992 0.060 0.940
#&gt; GSM11297     1  0.0000      0.826 1.000 0.000
#&gt; GSM11298     1  0.6438      0.925 0.836 0.164
#&gt; GSM11314     1  0.6438      0.925 0.836 0.164
#&gt; GSM11299     1  0.0672      0.820 0.992 0.008
#&gt; GSM28783     1  0.6438      0.925 0.836 0.164
#&gt; GSM11308     1  0.6438      0.925 0.836 0.164
#&gt; GSM28782     1  0.6438      0.925 0.836 0.164
#&gt; GSM28779     1  0.6438      0.925 0.836 0.164
#&gt; GSM11302     1  0.6438      0.925 0.836 0.164
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0592      0.885 0.988 0.012 0.000
#&gt; GSM28789     1  0.0592      0.885 0.988 0.012 0.000
#&gt; GSM28790     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11300     1  0.5465      0.495 0.712 0.000 0.288
#&gt; GSM28798     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11318     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM28792     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11295     3  0.4452      0.995 0.192 0.000 0.808
#&gt; GSM28793     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11312     1  0.0237      0.888 0.996 0.000 0.004
#&gt; GSM28778     1  0.4291      0.784 0.820 0.000 0.180
#&gt; GSM28796     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11309     1  0.0000      0.888 1.000 0.000 0.000
#&gt; GSM11315     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11306     1  0.0592      0.885 0.988 0.012 0.000
#&gt; GSM28776     1  0.0592      0.885 0.988 0.012 0.000
#&gt; GSM28777     3  0.4399      0.998 0.188 0.000 0.812
#&gt; GSM11316     3  0.4399      0.998 0.188 0.000 0.812
#&gt; GSM11320     3  0.4399      0.998 0.188 0.000 0.812
#&gt; GSM28797     1  0.0000      0.888 1.000 0.000 0.000
#&gt; GSM28786     1  0.0000      0.888 1.000 0.000 0.000
#&gt; GSM28800     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11310     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM28787     3  0.4452      0.995 0.192 0.000 0.808
#&gt; GSM11304     1  0.5397      0.514 0.720 0.000 0.280
#&gt; GSM11303     3  0.4399      0.998 0.188 0.000 0.812
#&gt; GSM11317     3  0.4399      0.998 0.188 0.000 0.812
#&gt; GSM11311     1  0.0000      0.888 1.000 0.000 0.000
#&gt; GSM28799     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM28791     1  0.4399      0.778 0.812 0.000 0.188
#&gt; GSM28794     2  0.3412      0.801 0.124 0.876 0.000
#&gt; GSM28780     1  0.4399      0.778 0.812 0.000 0.188
#&gt; GSM28795     1  0.4399      0.778 0.812 0.000 0.188
#&gt; GSM11301     2  0.0000      0.984 0.000 1.000 0.000
#&gt; GSM11297     1  0.5397      0.514 0.720 0.000 0.280
#&gt; GSM11298     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11314     1  0.4291      0.784 0.820 0.000 0.180
#&gt; GSM11299     1  0.5465      0.495 0.712 0.000 0.288
#&gt; GSM28783     1  0.4399      0.778 0.812 0.000 0.188
#&gt; GSM11308     1  0.4399      0.778 0.812 0.000 0.188
#&gt; GSM28782     1  0.3941      0.804 0.844 0.000 0.156
#&gt; GSM28779     1  0.0592      0.889 0.988 0.000 0.012
#&gt; GSM11302     1  0.0592      0.889 0.988 0.000 0.012
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.5290    0.30628 0.324 0.012 0.008 0.656
#&gt; GSM28789     4  0.5290    0.30628 0.324 0.012 0.008 0.656
#&gt; GSM28790     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11300     4  0.6211    0.09369 0.052 0.000 0.460 0.488
#&gt; GSM28798     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM28792     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11295     3  0.0188    0.99472 0.004 0.000 0.996 0.000
#&gt; GSM28793     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11312     1  0.5179    0.73231 0.728 0.000 0.052 0.220
#&gt; GSM28778     1  0.1474    0.76379 0.948 0.000 0.000 0.052
#&gt; GSM28796     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11309     4  0.0188    0.52506 0.000 0.000 0.004 0.996
#&gt; GSM11315     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11306     4  0.5673    0.00336 0.444 0.012 0.008 0.536
#&gt; GSM28776     4  0.5697   -0.08114 0.468 0.012 0.008 0.512
#&gt; GSM28777     3  0.0000    0.99789 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000    0.99789 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000    0.99789 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.0188    0.52506 0.000 0.000 0.004 0.996
#&gt; GSM28786     4  0.0188    0.52506 0.000 0.000 0.004 0.996
#&gt; GSM28800     1  0.5185    0.77097 0.748 0.000 0.076 0.176
#&gt; GSM11310     1  0.5185    0.77097 0.748 0.000 0.076 0.176
#&gt; GSM28787     3  0.0188    0.99472 0.004 0.000 0.996 0.000
#&gt; GSM11304     4  0.6332    0.11069 0.060 0.000 0.452 0.488
#&gt; GSM11303     3  0.0000    0.99789 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000    0.99789 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.3626    0.51754 0.184 0.000 0.004 0.812
#&gt; GSM28799     1  0.5267    0.76449 0.740 0.000 0.076 0.184
#&gt; GSM28791     1  0.0469    0.77775 0.988 0.000 0.000 0.012
#&gt; GSM28794     2  0.3043    0.81163 0.112 0.876 0.004 0.008
#&gt; GSM28780     1  0.0469    0.77775 0.988 0.000 0.000 0.012
#&gt; GSM28795     1  0.1302    0.76405 0.956 0.000 0.000 0.044
#&gt; GSM11301     2  0.0000    0.98470 0.000 1.000 0.000 0.000
#&gt; GSM11297     4  0.6332    0.11069 0.060 0.000 0.452 0.488
#&gt; GSM11298     1  0.4267    0.80153 0.788 0.000 0.188 0.024
#&gt; GSM11314     1  0.1474    0.76379 0.948 0.000 0.000 0.052
#&gt; GSM11299     4  0.6211    0.09369 0.052 0.000 0.460 0.488
#&gt; GSM28783     1  0.0592    0.77986 0.984 0.000 0.000 0.016
#&gt; GSM11308     1  0.0336    0.77911 0.992 0.000 0.000 0.008
#&gt; GSM28782     1  0.1833    0.79918 0.944 0.000 0.032 0.024
#&gt; GSM28779     1  0.5410    0.75641 0.728 0.000 0.080 0.192
#&gt; GSM11302     1  0.5434    0.75929 0.728 0.000 0.084 0.188
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4   0.519      0.176 0.356 0.004 0.000 0.596 0.044
#&gt; GSM28789     4   0.519      0.176 0.356 0.004 0.000 0.596 0.044
#&gt; GSM28790     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11300     4   0.769      0.356 0.112 0.000 0.172 0.484 0.232
#&gt; GSM28798     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM28792     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11295     3   0.429      0.661 0.004 0.000 0.612 0.000 0.384
#&gt; GSM28793     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11312     1   0.313      0.699 0.820 0.000 0.000 0.172 0.008
#&gt; GSM28778     5   0.444      0.936 0.396 0.000 0.000 0.008 0.596
#&gt; GSM28796     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11309     4   0.000      0.552 0.000 0.000 0.000 1.000 0.000
#&gt; GSM11315     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11306     4   0.526     -0.181 0.476 0.004 0.000 0.484 0.036
#&gt; GSM28776     1   0.525      0.125 0.500 0.004 0.000 0.460 0.036
#&gt; GSM28777     3   0.000      0.883 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3   0.000      0.883 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3   0.000      0.883 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4   0.000      0.552 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28786     4   0.000      0.552 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28800     1   0.256      0.736 0.856 0.000 0.000 0.144 0.000
#&gt; GSM11310     1   0.256      0.736 0.856 0.000 0.000 0.144 0.000
#&gt; GSM28787     3   0.430      0.658 0.004 0.000 0.608 0.000 0.388
#&gt; GSM11304     4   0.771      0.364 0.120 0.000 0.164 0.484 0.232
#&gt; GSM11303     3   0.000      0.883 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3   0.000      0.883 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4   0.300      0.497 0.188 0.000 0.000 0.812 0.000
#&gt; GSM28799     1   0.265      0.730 0.848 0.000 0.000 0.152 0.000
#&gt; GSM28791     5   0.426      0.950 0.440 0.000 0.000 0.000 0.560
#&gt; GSM28794     2   0.233      0.809 0.124 0.876 0.000 0.000 0.000
#&gt; GSM28780     5   0.426      0.950 0.440 0.000 0.000 0.000 0.560
#&gt; GSM28795     5   0.417      0.938 0.396 0.000 0.000 0.000 0.604
#&gt; GSM11301     2   0.000      0.984 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     4   0.771      0.364 0.120 0.000 0.164 0.484 0.232
#&gt; GSM11298     1   0.051      0.749 0.984 0.000 0.000 0.000 0.016
#&gt; GSM11314     5   0.444      0.936 0.396 0.000 0.000 0.008 0.596
#&gt; GSM11299     4   0.769      0.356 0.112 0.000 0.172 0.484 0.232
#&gt; GSM28783     5   0.442      0.943 0.448 0.000 0.000 0.004 0.548
#&gt; GSM11308     5   0.427      0.946 0.444 0.000 0.000 0.000 0.556
#&gt; GSM28782     1   0.464     -0.746 0.532 0.000 0.000 0.012 0.456
#&gt; GSM28779     1   0.284      0.729 0.848 0.000 0.000 0.144 0.008
#&gt; GSM11302     1   0.280      0.732 0.852 0.000 0.000 0.140 0.008
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4   0.344      0.838 0.172 0.000 0.000 0.796 0.020 0.012
#&gt; GSM28789     4   0.344      0.838 0.172 0.000 0.000 0.796 0.020 0.012
#&gt; GSM28790     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     6   0.391      0.604 0.076 0.000 0.164 0.000 0.000 0.760
#&gt; GSM28798     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28792     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11295     3   0.616      0.517 0.000 0.000 0.500 0.200 0.020 0.280
#&gt; GSM28793     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     1   0.390      0.712 0.748 0.000 0.000 0.212 0.012 0.028
#&gt; GSM28778     5   0.261      0.860 0.020 0.000 0.000 0.044 0.888 0.048
#&gt; GSM28796     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     6   0.381      0.497 0.000 0.000 0.000 0.428 0.000 0.572
#&gt; GSM11315     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     4   0.388      0.831 0.292 0.000 0.000 0.688 0.020 0.000
#&gt; GSM28776     4   0.399      0.795 0.316 0.000 0.000 0.664 0.020 0.000
#&gt; GSM28777     3   0.000      0.844 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3   0.000      0.844 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3   0.000      0.844 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     6   0.381      0.497 0.000 0.000 0.000 0.428 0.000 0.572
#&gt; GSM28786     6   0.381      0.497 0.000 0.000 0.000 0.428 0.000 0.572
#&gt; GSM28800     1   0.359      0.776 0.788 0.000 0.000 0.152 0.000 0.060
#&gt; GSM11310     1   0.359      0.776 0.788 0.000 0.000 0.152 0.000 0.060
#&gt; GSM28787     3   0.618      0.513 0.000 0.000 0.496 0.204 0.020 0.280
#&gt; GSM11304     6   0.394      0.608 0.084 0.000 0.156 0.000 0.000 0.760
#&gt; GSM11303     3   0.000      0.844 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3   0.000      0.844 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     6   0.563      0.345 0.188 0.000 0.000 0.284 0.000 0.528
#&gt; GSM28799     1   0.372      0.769 0.780 0.000 0.000 0.148 0.000 0.072
#&gt; GSM28791     5   0.279      0.813 0.140 0.000 0.000 0.000 0.840 0.020
#&gt; GSM28794     2   0.252      0.824 0.100 0.876 0.000 0.012 0.012 0.000
#&gt; GSM28780     5   0.142      0.864 0.032 0.000 0.000 0.000 0.944 0.024
#&gt; GSM28795     5   0.247      0.861 0.020 0.000 0.000 0.036 0.896 0.048
#&gt; GSM11301     2   0.000      0.986 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6   0.394      0.608 0.084 0.000 0.156 0.000 0.000 0.760
#&gt; GSM11298     1   0.000      0.840 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11314     5   0.261      0.860 0.020 0.000 0.000 0.044 0.888 0.048
#&gt; GSM11299     6   0.391      0.604 0.076 0.000 0.164 0.000 0.000 0.760
#&gt; GSM28783     5   0.256      0.825 0.120 0.000 0.000 0.008 0.864 0.008
#&gt; GSM11308     5   0.150      0.864 0.032 0.000 0.000 0.000 0.940 0.028
#&gt; GSM28782     5   0.455      0.615 0.244 0.000 0.000 0.020 0.692 0.044
#&gt; GSM28779     1   0.403      0.742 0.756 0.000 0.000 0.184 0.012 0.048
#&gt; GSM11302     1   0.400      0.746 0.760 0.000 0.000 0.180 0.012 0.048
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:hclust 52     0.396 2
#> CV:hclust 50     0.370 3
#> CV:hclust 44     0.497 4
#> CV:hclust 42     0.499 5
#> CV:hclust 48     0.398 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.393           0.825       0.834         0.3547 0.638   0.638
#> 3 3 1.000           0.979       0.969         0.6077 0.790   0.670
#> 4 4 0.731           0.755       0.828         0.2373 0.873   0.704
#> 5 5 0.722           0.814       0.851         0.0990 0.910   0.702
#> 6 6 0.814           0.771       0.811         0.0555 1.000   1.000
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.961      0.827 0.616 0.384
#&gt; GSM28789     1   0.961      0.827 0.616 0.384
#&gt; GSM28790     1   0.961      0.827 0.616 0.384
#&gt; GSM11300     1   0.000      0.612 1.000 0.000
#&gt; GSM28798     2   0.000      1.000 0.000 1.000
#&gt; GSM11296     2   0.000      1.000 0.000 1.000
#&gt; GSM28801     2   0.000      1.000 0.000 1.000
#&gt; GSM11319     2   0.000      1.000 0.000 1.000
#&gt; GSM28781     2   0.000      1.000 0.000 1.000
#&gt; GSM11305     2   0.000      1.000 0.000 1.000
#&gt; GSM28784     2   0.000      1.000 0.000 1.000
#&gt; GSM11307     2   0.000      1.000 0.000 1.000
#&gt; GSM11313     2   0.000      1.000 0.000 1.000
#&gt; GSM28785     2   0.000      1.000 0.000 1.000
#&gt; GSM11318     1   0.961      0.827 0.616 0.384
#&gt; GSM28792     1   0.961      0.827 0.616 0.384
#&gt; GSM11295     1   0.000      0.612 1.000 0.000
#&gt; GSM28793     1   0.961      0.827 0.616 0.384
#&gt; GSM11312     1   0.961      0.827 0.616 0.384
#&gt; GSM28778     1   0.961      0.827 0.616 0.384
#&gt; GSM28796     1   0.961      0.827 0.616 0.384
#&gt; GSM11309     1   0.827      0.795 0.740 0.260
#&gt; GSM11315     1   0.961      0.827 0.616 0.384
#&gt; GSM11306     1   0.961      0.827 0.616 0.384
#&gt; GSM28776     1   0.961      0.827 0.616 0.384
#&gt; GSM28777     1   0.000      0.612 1.000 0.000
#&gt; GSM11316     1   0.000      0.612 1.000 0.000
#&gt; GSM11320     1   0.000      0.612 1.000 0.000
#&gt; GSM28797     1   0.844      0.801 0.728 0.272
#&gt; GSM28786     1   0.814      0.791 0.748 0.252
#&gt; GSM28800     1   0.961      0.827 0.616 0.384
#&gt; GSM11310     1   0.925      0.820 0.660 0.340
#&gt; GSM28787     1   0.000      0.612 1.000 0.000
#&gt; GSM11304     1   0.827      0.795 0.740 0.260
#&gt; GSM11303     1   0.000      0.612 1.000 0.000
#&gt; GSM11317     1   0.000      0.612 1.000 0.000
#&gt; GSM11311     1   0.844      0.801 0.728 0.272
#&gt; GSM28799     1   0.881      0.809 0.700 0.300
#&gt; GSM28791     1   0.961      0.827 0.616 0.384
#&gt; GSM28794     2   0.000      1.000 0.000 1.000
#&gt; GSM28780     1   0.958      0.827 0.620 0.380
#&gt; GSM28795     1   0.958      0.827 0.620 0.380
#&gt; GSM11301     2   0.000      1.000 0.000 1.000
#&gt; GSM11297     1   0.827      0.795 0.740 0.260
#&gt; GSM11298     1   0.961      0.827 0.616 0.384
#&gt; GSM11314     1   0.958      0.827 0.620 0.380
#&gt; GSM11299     1   0.000      0.612 1.000 0.000
#&gt; GSM28783     1   0.958      0.827 0.620 0.380
#&gt; GSM11308     1   0.844      0.801 0.728 0.272
#&gt; GSM28782     1   0.958      0.827 0.620 0.380
#&gt; GSM28779     1   0.961      0.827 0.616 0.384
#&gt; GSM11302     1   0.961      0.827 0.616 0.384
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.2496      0.961 0.928 0.004 0.068
#&gt; GSM28789     1  0.2496      0.961 0.928 0.004 0.068
#&gt; GSM28790     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11300     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM28798     2  0.0661      0.994 0.004 0.988 0.008
#&gt; GSM11296     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM28801     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11319     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM28781     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11305     2  0.0661      0.994 0.004 0.988 0.008
#&gt; GSM28784     2  0.0661      0.992 0.004 0.988 0.008
#&gt; GSM11307     2  0.0661      0.994 0.004 0.988 0.008
#&gt; GSM11313     2  0.0661      0.994 0.004 0.988 0.008
#&gt; GSM28785     2  0.0237      0.995 0.004 0.996 0.000
#&gt; GSM11318     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28792     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11295     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM28793     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11312     1  0.0424      0.971 0.992 0.000 0.008
#&gt; GSM28778     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28796     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11309     1  0.2496      0.961 0.928 0.004 0.068
#&gt; GSM11315     1  0.1163      0.975 0.972 0.000 0.028
#&gt; GSM11306     1  0.2096      0.966 0.944 0.004 0.052
#&gt; GSM28776     1  0.1289      0.975 0.968 0.000 0.032
#&gt; GSM28777     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM11316     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM11320     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM28797     1  0.2496      0.961 0.928 0.004 0.068
#&gt; GSM28786     1  0.2496      0.961 0.928 0.004 0.068
#&gt; GSM28800     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11310     1  0.1753      0.970 0.952 0.000 0.048
#&gt; GSM28787     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM11304     1  0.2261      0.950 0.932 0.000 0.068
#&gt; GSM11303     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM11317     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM11311     1  0.2096      0.966 0.944 0.004 0.052
#&gt; GSM28799     1  0.1529      0.969 0.960 0.000 0.040
#&gt; GSM28791     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28794     2  0.1267      0.986 0.004 0.972 0.024
#&gt; GSM28780     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11301     2  0.1129      0.987 0.004 0.976 0.020
#&gt; GSM11297     1  0.3038      0.912 0.896 0.000 0.104
#&gt; GSM11298     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11314     1  0.0237      0.972 0.996 0.000 0.004
#&gt; GSM11299     3  0.1964      1.000 0.056 0.000 0.944
#&gt; GSM28783     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM28779     1  0.1031      0.975 0.976 0.000 0.024
#&gt; GSM11302     1  0.1031      0.975 0.976 0.000 0.024
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.5163     0.8964 0.480 0.000 0.004 0.516
#&gt; GSM28789     4  0.5163     0.8964 0.480 0.000 0.004 0.516
#&gt; GSM28790     1  0.4008     0.6257 0.756 0.000 0.000 0.244
#&gt; GSM11300     3  0.0779     0.9809 0.004 0.000 0.980 0.016
#&gt; GSM28798     2  0.1004     0.9800 0.004 0.972 0.000 0.024
#&gt; GSM11296     2  0.0376     0.9814 0.004 0.992 0.000 0.004
#&gt; GSM28801     2  0.0376     0.9814 0.004 0.992 0.000 0.004
#&gt; GSM11319     2  0.0188     0.9816 0.004 0.996 0.000 0.000
#&gt; GSM28781     2  0.0376     0.9814 0.004 0.992 0.000 0.004
#&gt; GSM11305     2  0.1004     0.9800 0.004 0.972 0.000 0.024
#&gt; GSM28784     2  0.1209     0.9723 0.004 0.964 0.000 0.032
#&gt; GSM11307     2  0.1004     0.9800 0.004 0.972 0.000 0.024
#&gt; GSM11313     2  0.1004     0.9800 0.004 0.972 0.000 0.024
#&gt; GSM28785     2  0.0188     0.9816 0.004 0.996 0.000 0.000
#&gt; GSM11318     1  0.4008     0.6253 0.756 0.000 0.000 0.244
#&gt; GSM28792     1  0.0817     0.5983 0.976 0.000 0.000 0.024
#&gt; GSM11295     3  0.1576     0.9690 0.004 0.000 0.948 0.048
#&gt; GSM28793     1  0.1118     0.5872 0.964 0.000 0.000 0.036
#&gt; GSM11312     1  0.4164     0.6025 0.736 0.000 0.000 0.264
#&gt; GSM28778     1  0.4855     0.5801 0.600 0.000 0.000 0.400
#&gt; GSM28796     1  0.0921     0.5951 0.972 0.000 0.000 0.028
#&gt; GSM11309     4  0.5080     0.9199 0.420 0.000 0.004 0.576
#&gt; GSM11315     1  0.1118     0.5872 0.964 0.000 0.000 0.036
#&gt; GSM11306     4  0.4994     0.8929 0.480 0.000 0.000 0.520
#&gt; GSM28776     1  0.2530     0.4340 0.888 0.000 0.000 0.112
#&gt; GSM28777     3  0.0188     0.9853 0.004 0.000 0.996 0.000
#&gt; GSM11316     3  0.0564     0.9863 0.004 0.004 0.988 0.004
#&gt; GSM11320     3  0.0564     0.9863 0.004 0.004 0.988 0.004
#&gt; GSM28797     4  0.5080     0.9199 0.420 0.000 0.004 0.576
#&gt; GSM28786     4  0.5080     0.9199 0.420 0.000 0.004 0.576
#&gt; GSM28800     1  0.0469     0.5983 0.988 0.000 0.000 0.012
#&gt; GSM11310     1  0.3908     0.1179 0.784 0.000 0.004 0.212
#&gt; GSM28787     3  0.1576     0.9690 0.004 0.000 0.948 0.048
#&gt; GSM11304     1  0.5714     0.2097 0.716 0.000 0.128 0.156
#&gt; GSM11303     3  0.0564     0.9863 0.004 0.004 0.988 0.004
#&gt; GSM11317     3  0.0564     0.9863 0.004 0.004 0.988 0.004
#&gt; GSM11311     4  0.4925     0.8789 0.428 0.000 0.000 0.572
#&gt; GSM28799     1  0.4053     0.0973 0.768 0.000 0.004 0.228
#&gt; GSM28791     1  0.4679     0.6060 0.648 0.000 0.000 0.352
#&gt; GSM28794     2  0.2382     0.9540 0.004 0.912 0.004 0.080
#&gt; GSM28780     1  0.4830     0.5873 0.608 0.000 0.000 0.392
#&gt; GSM28795     1  0.4855     0.5801 0.600 0.000 0.000 0.400
#&gt; GSM11301     2  0.1930     0.9583 0.004 0.936 0.004 0.056
#&gt; GSM11297     1  0.5859     0.1813 0.704 0.000 0.140 0.156
#&gt; GSM11298     1  0.0921     0.6004 0.972 0.000 0.000 0.028
#&gt; GSM11314     1  0.4866     0.5773 0.596 0.000 0.000 0.404
#&gt; GSM11299     3  0.0779     0.9809 0.004 0.000 0.980 0.016
#&gt; GSM28783     1  0.4830     0.5873 0.608 0.000 0.000 0.392
#&gt; GSM11308     1  0.4830     0.5873 0.608 0.000 0.000 0.392
#&gt; GSM28782     1  0.4277     0.6247 0.720 0.000 0.000 0.280
#&gt; GSM28779     1  0.0188     0.6041 0.996 0.000 0.000 0.004
#&gt; GSM11302     1  0.0188     0.6041 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.5322      0.839 0.188 0.000 0.000 0.672 0.140
#&gt; GSM28789     4  0.5322      0.839 0.188 0.000 0.000 0.672 0.140
#&gt; GSM28790     1  0.2329      0.639 0.876 0.000 0.000 0.000 0.124
#&gt; GSM11300     3  0.1525      0.939 0.004 0.000 0.948 0.036 0.012
#&gt; GSM28798     2  0.1661      0.949 0.000 0.940 0.000 0.024 0.036
#&gt; GSM11296     2  0.0000      0.957 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.957 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.957 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.957 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.1661      0.949 0.000 0.940 0.000 0.024 0.036
#&gt; GSM28784     2  0.1992      0.930 0.000 0.924 0.000 0.044 0.032
#&gt; GSM11307     2  0.1661      0.949 0.000 0.940 0.000 0.024 0.036
#&gt; GSM11313     2  0.1661      0.949 0.000 0.940 0.000 0.024 0.036
#&gt; GSM28785     2  0.0000      0.957 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.2966      0.555 0.816 0.000 0.000 0.000 0.184
#&gt; GSM28792     1  0.1106      0.742 0.964 0.000 0.000 0.024 0.012
#&gt; GSM11295     3  0.2645      0.917 0.000 0.000 0.888 0.044 0.068
#&gt; GSM28793     1  0.1106      0.742 0.964 0.000 0.000 0.024 0.012
#&gt; GSM11312     1  0.5548     -0.208 0.492 0.000 0.000 0.068 0.440
#&gt; GSM28778     5  0.3942      0.977 0.232 0.000 0.000 0.020 0.748
#&gt; GSM28796     1  0.1106      0.742 0.964 0.000 0.000 0.024 0.012
#&gt; GSM11309     4  0.2824      0.893 0.116 0.000 0.000 0.864 0.020
#&gt; GSM11315     1  0.1106      0.742 0.964 0.000 0.000 0.024 0.012
#&gt; GSM11306     4  0.5264      0.835 0.196 0.000 0.000 0.676 0.128
#&gt; GSM28776     1  0.4017      0.681 0.788 0.000 0.000 0.148 0.064
#&gt; GSM28777     3  0.0290      0.955 0.000 0.000 0.992 0.008 0.000
#&gt; GSM11316     3  0.1211      0.956 0.000 0.000 0.960 0.016 0.024
#&gt; GSM11320     3  0.0865      0.957 0.000 0.000 0.972 0.004 0.024
#&gt; GSM28797     4  0.2824      0.893 0.116 0.000 0.000 0.864 0.020
#&gt; GSM28786     4  0.2824      0.893 0.116 0.000 0.000 0.864 0.020
#&gt; GSM28800     1  0.2740      0.726 0.876 0.000 0.000 0.028 0.096
#&gt; GSM11310     1  0.4823      0.567 0.700 0.000 0.000 0.228 0.072
#&gt; GSM28787     3  0.2708      0.916 0.000 0.000 0.884 0.044 0.072
#&gt; GSM11304     1  0.6859      0.527 0.604 0.000 0.128 0.152 0.116
#&gt; GSM11303     3  0.0865      0.957 0.000 0.000 0.972 0.004 0.024
#&gt; GSM11317     3  0.0865      0.957 0.000 0.000 0.972 0.004 0.024
#&gt; GSM11311     4  0.2723      0.885 0.124 0.000 0.000 0.864 0.012
#&gt; GSM28799     1  0.5043      0.572 0.692 0.000 0.000 0.208 0.100
#&gt; GSM28791     5  0.3689      0.963 0.256 0.000 0.000 0.004 0.740
#&gt; GSM28794     2  0.3701      0.882 0.004 0.824 0.000 0.060 0.112
#&gt; GSM28780     5  0.3835      0.974 0.244 0.000 0.000 0.012 0.744
#&gt; GSM28795     5  0.3942      0.977 0.232 0.000 0.000 0.020 0.748
#&gt; GSM11301     2  0.2859      0.903 0.000 0.876 0.000 0.056 0.068
#&gt; GSM11297     1  0.6897      0.521 0.600 0.000 0.132 0.152 0.116
#&gt; GSM11298     1  0.0992      0.743 0.968 0.000 0.000 0.024 0.008
#&gt; GSM11314     5  0.3970      0.970 0.224 0.000 0.000 0.024 0.752
#&gt; GSM11299     3  0.1525      0.939 0.004 0.000 0.948 0.036 0.012
#&gt; GSM28783     5  0.3756      0.962 0.248 0.000 0.000 0.008 0.744
#&gt; GSM11308     5  0.3728      0.971 0.244 0.000 0.000 0.008 0.748
#&gt; GSM28782     1  0.4383     -0.102 0.572 0.000 0.000 0.004 0.424
#&gt; GSM28779     1  0.1638      0.737 0.932 0.000 0.000 0.004 0.064
#&gt; GSM11302     1  0.1357      0.738 0.948 0.000 0.000 0.004 0.048
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28788     4  0.5859     0.7451 0.176 0.000 0.000 0.628 0.080 NA
#&gt; GSM28789     4  0.5859     0.7451 0.176 0.000 0.000 0.628 0.080 NA
#&gt; GSM28790     1  0.4361     0.6666 0.700 0.000 0.000 0.016 0.036 NA
#&gt; GSM11300     3  0.2265     0.8901 0.004 0.000 0.896 0.024 0.000 NA
#&gt; GSM28798     2  0.1701     0.9058 0.000 0.920 0.000 0.000 0.008 NA
#&gt; GSM11296     2  0.0146     0.9150 0.000 0.996 0.000 0.000 0.000 NA
#&gt; GSM28801     2  0.0291     0.9147 0.000 0.992 0.000 0.000 0.004 NA
#&gt; GSM11319     2  0.0000     0.9153 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28781     2  0.0291     0.9147 0.000 0.992 0.000 0.000 0.004 NA
#&gt; GSM11305     2  0.1701     0.9058 0.000 0.920 0.000 0.000 0.008 NA
#&gt; GSM28784     2  0.2595     0.8426 0.000 0.836 0.000 0.000 0.004 NA
#&gt; GSM11307     2  0.1701     0.9058 0.000 0.920 0.000 0.000 0.008 NA
#&gt; GSM11313     2  0.1701     0.9058 0.000 0.920 0.000 0.000 0.008 NA
#&gt; GSM28785     2  0.0000     0.9153 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11318     1  0.4874     0.6418 0.664 0.000 0.000 0.016 0.072 NA
#&gt; GSM28792     1  0.4207     0.6716 0.712 0.000 0.000 0.020 0.024 NA
#&gt; GSM11295     3  0.3349     0.8496 0.000 0.000 0.804 0.008 0.024 NA
#&gt; GSM28793     1  0.4050     0.6759 0.728 0.000 0.000 0.016 0.024 NA
#&gt; GSM11312     1  0.5456     0.0805 0.500 0.000 0.000 0.044 0.416 NA
#&gt; GSM28778     5  0.1934     0.9621 0.044 0.000 0.000 0.000 0.916 NA
#&gt; GSM28796     1  0.4050     0.6759 0.728 0.000 0.000 0.016 0.024 NA
#&gt; GSM11309     4  0.0520     0.8343 0.008 0.000 0.000 0.984 0.008 NA
#&gt; GSM11315     1  0.4050     0.6759 0.728 0.000 0.000 0.016 0.024 NA
#&gt; GSM11306     4  0.5718     0.7485 0.172 0.000 0.000 0.644 0.084 NA
#&gt; GSM28776     1  0.3053     0.6245 0.856 0.000 0.000 0.080 0.048 NA
#&gt; GSM28777     3  0.0405     0.9301 0.000 0.000 0.988 0.000 0.004 NA
#&gt; GSM11316     3  0.0632     0.9268 0.000 0.000 0.976 0.000 0.000 NA
#&gt; GSM11320     3  0.0146     0.9309 0.000 0.000 0.996 0.000 0.000 NA
#&gt; GSM28797     4  0.0520     0.8343 0.008 0.000 0.000 0.984 0.008 NA
#&gt; GSM28786     4  0.0520     0.8343 0.008 0.000 0.000 0.984 0.008 NA
#&gt; GSM28800     1  0.2823     0.6415 0.872 0.000 0.000 0.016 0.068 NA
#&gt; GSM11310     1  0.4684     0.4981 0.716 0.000 0.000 0.192 0.048 NA
#&gt; GSM28787     3  0.3536     0.8388 0.000 0.000 0.784 0.012 0.020 NA
#&gt; GSM11304     1  0.7136     0.4060 0.564 0.000 0.080 0.140 0.092 NA
#&gt; GSM11303     3  0.0146     0.9309 0.000 0.000 0.996 0.000 0.000 NA
#&gt; GSM11317     3  0.0146     0.9309 0.000 0.000 0.996 0.000 0.000 NA
#&gt; GSM11311     4  0.0653     0.8298 0.012 0.000 0.000 0.980 0.004 NA
#&gt; GSM28799     1  0.5295     0.4792 0.676 0.000 0.000 0.180 0.056 NA
#&gt; GSM28791     5  0.1528     0.9628 0.048 0.000 0.000 0.000 0.936 NA
#&gt; GSM28794     2  0.4859     0.6527 0.056 0.604 0.000 0.000 0.008 NA
#&gt; GSM28780     5  0.1398     0.9621 0.052 0.000 0.000 0.000 0.940 NA
#&gt; GSM28795     5  0.1934     0.9621 0.044 0.000 0.000 0.000 0.916 NA
#&gt; GSM11301     2  0.2996     0.7994 0.000 0.772 0.000 0.000 0.000 NA
#&gt; GSM11297     1  0.7136     0.4060 0.564 0.000 0.080 0.140 0.092 NA
#&gt; GSM11298     1  0.3831     0.6768 0.744 0.000 0.000 0.012 0.020 NA
#&gt; GSM11314     5  0.2070     0.9594 0.048 0.000 0.000 0.000 0.908 NA
#&gt; GSM11299     3  0.2510     0.8839 0.008 0.000 0.884 0.028 0.000 NA
#&gt; GSM28783     5  0.2163     0.9158 0.092 0.000 0.000 0.000 0.892 NA
#&gt; GSM11308     5  0.1542     0.9605 0.052 0.000 0.000 0.004 0.936 NA
#&gt; GSM28782     1  0.4527     0.0765 0.516 0.000 0.000 0.004 0.456 NA
#&gt; GSM28779     1  0.1672     0.6623 0.932 0.000 0.000 0.004 0.048 NA
#&gt; GSM11302     1  0.2279     0.6710 0.900 0.000 0.000 0.004 0.048 NA
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:kmeans 52     0.396 2
#> CV:kmeans 52     0.372 3
#> CV:kmeans 47     0.450 4
#> CV:kmeans 50     0.431 5
#> CV:kmeans 46     0.431 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.964           0.970       0.985         0.4096 0.599   0.599
#> 3 3 0.967           0.909       0.964         0.5951 0.687   0.501
#> 4 4 0.753           0.643       0.846         0.1527 0.834   0.557
#> 5 5 0.940           0.906       0.955         0.0755 0.904   0.634
#> 6 6 0.905           0.799       0.894         0.0357 0.943   0.714
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.000      0.982 1.000 0.000
#&gt; GSM28789     2   0.204      0.961 0.032 0.968
#&gt; GSM28790     1   0.000      0.982 1.000 0.000
#&gt; GSM11300     1   0.000      0.982 1.000 0.000
#&gt; GSM28798     2   0.000      0.988 0.000 1.000
#&gt; GSM11296     2   0.000      0.988 0.000 1.000
#&gt; GSM28801     2   0.000      0.988 0.000 1.000
#&gt; GSM11319     2   0.000      0.988 0.000 1.000
#&gt; GSM28781     2   0.000      0.988 0.000 1.000
#&gt; GSM11305     2   0.000      0.988 0.000 1.000
#&gt; GSM28784     2   0.000      0.988 0.000 1.000
#&gt; GSM11307     2   0.000      0.988 0.000 1.000
#&gt; GSM11313     2   0.000      0.988 0.000 1.000
#&gt; GSM28785     2   0.000      0.988 0.000 1.000
#&gt; GSM11318     1   0.000      0.982 1.000 0.000
#&gt; GSM28792     1   0.000      0.982 1.000 0.000
#&gt; GSM11295     1   0.204      0.961 0.968 0.032
#&gt; GSM28793     1   0.000      0.982 1.000 0.000
#&gt; GSM11312     1   0.000      0.982 1.000 0.000
#&gt; GSM28778     2   0.541      0.864 0.124 0.876
#&gt; GSM28796     1   0.000      0.982 1.000 0.000
#&gt; GSM11309     1   0.000      0.982 1.000 0.000
#&gt; GSM11315     1   0.000      0.982 1.000 0.000
#&gt; GSM11306     1   0.000      0.982 1.000 0.000
#&gt; GSM28776     1   0.000      0.982 1.000 0.000
#&gt; GSM28777     1   0.204      0.961 0.968 0.032
#&gt; GSM11316     1   0.671      0.806 0.824 0.176
#&gt; GSM11320     1   0.204      0.961 0.968 0.032
#&gt; GSM28797     1   0.000      0.982 1.000 0.000
#&gt; GSM28786     1   0.000      0.982 1.000 0.000
#&gt; GSM28800     1   0.000      0.982 1.000 0.000
#&gt; GSM11310     1   0.000      0.982 1.000 0.000
#&gt; GSM28787     1   0.662      0.811 0.828 0.172
#&gt; GSM11304     1   0.000      0.982 1.000 0.000
#&gt; GSM11303     1   0.204      0.961 0.968 0.032
#&gt; GSM11317     1   0.204      0.961 0.968 0.032
#&gt; GSM11311     1   0.000      0.982 1.000 0.000
#&gt; GSM28799     1   0.000      0.982 1.000 0.000
#&gt; GSM28791     1   0.000      0.982 1.000 0.000
#&gt; GSM28794     2   0.000      0.988 0.000 1.000
#&gt; GSM28780     1   0.000      0.982 1.000 0.000
#&gt; GSM28795     1   0.000      0.982 1.000 0.000
#&gt; GSM11301     2   0.000      0.988 0.000 1.000
#&gt; GSM11297     1   0.000      0.982 1.000 0.000
#&gt; GSM11298     1   0.000      0.982 1.000 0.000
#&gt; GSM11314     1   0.584      0.841 0.860 0.140
#&gt; GSM11299     1   0.000      0.982 1.000 0.000
#&gt; GSM28783     1   0.000      0.982 1.000 0.000
#&gt; GSM11308     1   0.000      0.982 1.000 0.000
#&gt; GSM28782     1   0.000      0.982 1.000 0.000
#&gt; GSM28779     1   0.000      0.982 1.000 0.000
#&gt; GSM11302     1   0.000      0.982 1.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     3  0.6309     0.0601 0.496 0.000 0.504
#&gt; GSM28789     3  0.8275     0.0788 0.076 0.452 0.472
#&gt; GSM28790     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11309     3  0.0424     0.8946 0.008 0.000 0.992
#&gt; GSM11315     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11306     1  0.0592     0.9741 0.988 0.000 0.012
#&gt; GSM28776     1  0.0592     0.9741 0.988 0.000 0.012
#&gt; GSM28777     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM28797     3  0.1643     0.8675 0.044 0.000 0.956
#&gt; GSM28786     3  0.0424     0.8946 0.008 0.000 0.992
#&gt; GSM28800     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11310     1  0.4235     0.7711 0.824 0.000 0.176
#&gt; GSM28787     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11304     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11303     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11311     1  0.2625     0.9032 0.916 0.000 0.084
#&gt; GSM28799     3  0.6095     0.3721 0.392 0.000 0.608
#&gt; GSM28791     1  0.0237     0.9797 0.996 0.000 0.004
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28780     1  0.0237     0.9797 0.996 0.000 0.004
#&gt; GSM28795     1  0.0237     0.9797 0.996 0.000 0.004
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11297     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM11298     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11314     1  0.1315     0.9637 0.972 0.008 0.020
#&gt; GSM11299     3  0.0000     0.8983 0.000 0.000 1.000
#&gt; GSM28783     1  0.0237     0.9797 0.996 0.000 0.004
#&gt; GSM11308     1  0.1860     0.9383 0.948 0.000 0.052
#&gt; GSM28782     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000     0.9812 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.9812 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.3852     0.5238 0.180 0.000 0.012 0.808
#&gt; GSM28789     4  0.4410     0.5203 0.144 0.032 0.012 0.812
#&gt; GSM28790     1  0.3266     0.6085 0.832 0.000 0.000 0.168
#&gt; GSM11300     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.3219     0.6116 0.836 0.000 0.000 0.164
#&gt; GSM28792     1  0.1474     0.6677 0.948 0.000 0.000 0.052
#&gt; GSM11295     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.0000     0.6801 1.000 0.000 0.000 0.000
#&gt; GSM11312     4  0.4999    -0.1411 0.492 0.000 0.000 0.508
#&gt; GSM28778     4  0.4996    -0.2266 0.484 0.000 0.000 0.516
#&gt; GSM28796     1  0.0000     0.6801 1.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.5897     0.5104 0.164 0.000 0.136 0.700
#&gt; GSM11315     1  0.0000     0.6801 1.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.3649     0.5167 0.204 0.000 0.000 0.796
#&gt; GSM28776     1  0.4730    -0.0342 0.636 0.000 0.000 0.364
#&gt; GSM28777     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.5332     0.5234 0.184 0.000 0.080 0.736
#&gt; GSM28786     4  0.6469     0.4790 0.164 0.000 0.192 0.644
#&gt; GSM28800     1  0.0707     0.6778 0.980 0.000 0.000 0.020
#&gt; GSM11310     1  0.4977    -0.2222 0.540 0.000 0.000 0.460
#&gt; GSM28787     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM11304     3  0.0657     0.9846 0.004 0.000 0.984 0.012
#&gt; GSM11303     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.5060     0.3283 0.412 0.000 0.004 0.584
#&gt; GSM28799     4  0.7300     0.3078 0.372 0.000 0.156 0.472
#&gt; GSM28791     1  0.4933     0.2851 0.568 0.000 0.000 0.432
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28780     4  0.5000    -0.2580 0.500 0.000 0.000 0.500
#&gt; GSM28795     4  0.4996    -0.2266 0.484 0.000 0.000 0.516
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11297     3  0.0921     0.9723 0.000 0.000 0.972 0.028
#&gt; GSM11298     1  0.0000     0.6801 1.000 0.000 0.000 0.000
#&gt; GSM11314     4  0.5035     0.1305 0.284 0.004 0.016 0.696
#&gt; GSM11299     3  0.0000     0.9956 0.000 0.000 1.000 0.000
#&gt; GSM28783     1  0.5000     0.1528 0.504 0.000 0.000 0.496
#&gt; GSM11308     1  0.6011     0.1501 0.484 0.000 0.040 0.476
#&gt; GSM28782     1  0.4040     0.5425 0.752 0.000 0.000 0.248
#&gt; GSM28779     1  0.0000     0.6801 1.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.6801 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM28788     4  0.0613     0.8729 0.008  0 0.004 0.984 0.004
#&gt; GSM28789     4  0.0613     0.8729 0.008  0 0.004 0.984 0.004
#&gt; GSM28790     1  0.1571     0.9013 0.936  0 0.000 0.004 0.060
#&gt; GSM11300     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM28798     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11318     1  0.2439     0.8510 0.876  0 0.000 0.004 0.120
#&gt; GSM28792     1  0.0290     0.9284 0.992  0 0.000 0.008 0.000
#&gt; GSM11295     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM28793     1  0.0290     0.9284 0.992  0 0.000 0.008 0.000
#&gt; GSM11312     5  0.4300     0.7498 0.132  0 0.000 0.096 0.772
#&gt; GSM28778     5  0.0693     0.9368 0.012  0 0.000 0.008 0.980
#&gt; GSM28796     1  0.0290     0.9284 0.992  0 0.000 0.008 0.000
#&gt; GSM11309     4  0.0290     0.8738 0.000  0 0.008 0.992 0.000
#&gt; GSM11315     1  0.0290     0.9284 0.992  0 0.000 0.008 0.000
#&gt; GSM11306     4  0.0898     0.8645 0.008  0 0.000 0.972 0.020
#&gt; GSM28776     1  0.3934     0.5782 0.716  0 0.000 0.276 0.008
#&gt; GSM28777     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM28797     4  0.0404     0.8735 0.000  0 0.012 0.988 0.000
#&gt; GSM28786     4  0.0404     0.8735 0.000  0 0.012 0.988 0.000
#&gt; GSM28800     1  0.2464     0.8609 0.888  0 0.000 0.016 0.096
#&gt; GSM11310     4  0.4656     0.0752 0.480  0 0.000 0.508 0.012
#&gt; GSM28787     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM11304     3  0.2483     0.9143 0.028  0 0.908 0.048 0.016
#&gt; GSM11303     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM11311     4  0.1121     0.8600 0.044  0 0.000 0.956 0.000
#&gt; GSM28799     4  0.5463     0.5175 0.292  0 0.052 0.636 0.020
#&gt; GSM28791     5  0.0000     0.9402 0.000  0 0.000 0.000 1.000
#&gt; GSM28794     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28780     5  0.0162     0.9405 0.004  0 0.000 0.000 0.996
#&gt; GSM28795     5  0.0451     0.9393 0.004  0 0.000 0.008 0.988
#&gt; GSM11301     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11297     3  0.3110     0.8857 0.044  0 0.876 0.060 0.020
#&gt; GSM11298     1  0.0451     0.9274 0.988  0 0.000 0.008 0.004
#&gt; GSM11314     5  0.0451     0.9388 0.000  0 0.004 0.008 0.988
#&gt; GSM11299     3  0.0000     0.9801 0.000  0 1.000 0.000 0.000
#&gt; GSM28783     5  0.0162     0.9395 0.000  0 0.000 0.004 0.996
#&gt; GSM11308     5  0.0162     0.9398 0.000  0 0.004 0.000 0.996
#&gt; GSM28782     5  0.3086     0.7800 0.180  0 0.000 0.004 0.816
#&gt; GSM28779     1  0.0912     0.9225 0.972  0 0.000 0.016 0.012
#&gt; GSM11302     1  0.0807     0.9240 0.976  0 0.000 0.012 0.012
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.2482    0.85990 0.000 0.000 0.000 0.848 0.004 0.148
#&gt; GSM28789     4  0.2442    0.86099 0.000 0.000 0.000 0.852 0.004 0.144
#&gt; GSM28790     1  0.1967    0.79934 0.904 0.000 0.000 0.000 0.084 0.012
#&gt; GSM11300     3  0.1204    0.94138 0.000 0.000 0.944 0.000 0.000 0.056
#&gt; GSM28798     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.1700    0.80130 0.916 0.000 0.000 0.000 0.080 0.004
#&gt; GSM28792     1  0.0405    0.84326 0.988 0.000 0.000 0.000 0.004 0.008
#&gt; GSM11295     3  0.0146    0.97907 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM28793     1  0.0000    0.84640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     6  0.6164   -0.14887 0.080 0.000 0.000 0.064 0.424 0.432
#&gt; GSM28778     5  0.1152    0.87434 0.004 0.000 0.000 0.000 0.952 0.044
#&gt; GSM28796     1  0.0000    0.84640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0363    0.88959 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; GSM11315     1  0.0000    0.84640 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.2838    0.80145 0.000 0.000 0.000 0.808 0.004 0.188
#&gt; GSM28776     6  0.5880    0.00902 0.384 0.000 0.000 0.172 0.004 0.440
#&gt; GSM28777     3  0.0000    0.98101 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000    0.98101 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000    0.98101 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0291    0.89086 0.000 0.000 0.004 0.992 0.000 0.004
#&gt; GSM28786     4  0.0291    0.89086 0.000 0.000 0.004 0.992 0.000 0.004
#&gt; GSM28800     6  0.3622    0.41606 0.212 0.000 0.000 0.004 0.024 0.760
#&gt; GSM11310     6  0.4144    0.48686 0.072 0.000 0.000 0.200 0.000 0.728
#&gt; GSM28787     3  0.0260    0.97712 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11304     6  0.6207    0.26648 0.020 0.000 0.360 0.092 0.028 0.500
#&gt; GSM11303     3  0.0000    0.98101 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000    0.98101 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.2147    0.82597 0.084 0.000 0.000 0.896 0.000 0.020
#&gt; GSM28799     6  0.5703    0.45749 0.108 0.000 0.032 0.188 0.020 0.652
#&gt; GSM28791     5  0.1152    0.87330 0.004 0.000 0.000 0.000 0.952 0.044
#&gt; GSM28794     2  0.0146    0.99570 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28780     5  0.0363    0.88175 0.000 0.000 0.000 0.000 0.988 0.012
#&gt; GSM28795     5  0.0458    0.88003 0.000 0.000 0.000 0.000 0.984 0.016
#&gt; GSM11301     2  0.0000    0.99961 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.6227    0.33208 0.020 0.000 0.324 0.096 0.032 0.528
#&gt; GSM11298     1  0.1863    0.80078 0.896 0.000 0.000 0.000 0.000 0.104
#&gt; GSM11314     5  0.1493    0.86475 0.000 0.000 0.004 0.004 0.936 0.056
#&gt; GSM11299     3  0.1531    0.92673 0.000 0.000 0.928 0.004 0.000 0.068
#&gt; GSM28783     5  0.1866    0.85432 0.000 0.000 0.000 0.008 0.908 0.084
#&gt; GSM11308     5  0.1219    0.87352 0.000 0.000 0.000 0.004 0.948 0.048
#&gt; GSM28782     5  0.4852    0.21600 0.048 0.000 0.000 0.004 0.528 0.420
#&gt; GSM28779     1  0.4211    0.26508 0.532 0.000 0.000 0.008 0.004 0.456
#&gt; GSM11302     1  0.3448    0.62245 0.716 0.000 0.000 0.004 0.000 0.280
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> CV:skmeans 52     0.396 2
#> CV:skmeans 49     0.446 3
#> CV:skmeans 39     0.473 4
#> CV:skmeans 51     0.437 5
#> CV:skmeans 43     0.436 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.966       0.988         0.3754 0.638   0.638
#> 3 3 0.881           0.941       0.976         0.5426 0.790   0.670
#> 4 4 0.659           0.575       0.781         0.2331 0.834   0.612
#> 5 5 0.729           0.614       0.807         0.0847 0.842   0.514
#> 6 6 0.821           0.768       0.890         0.0600 0.894   0.589
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28789     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28790     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11300     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28798     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11296     2  0.0000     0.9996 0.000 1.000
#&gt; GSM28801     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11319     2  0.0000     0.9996 0.000 1.000
#&gt; GSM28781     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11305     2  0.0000     0.9996 0.000 1.000
#&gt; GSM28784     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11307     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11313     2  0.0000     0.9996 0.000 1.000
#&gt; GSM28785     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11318     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28792     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11295     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28793     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11312     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28778     1  0.5629     0.8392 0.868 0.132
#&gt; GSM28796     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11309     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11315     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11306     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28776     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28777     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11316     1  1.0000     0.0153 0.504 0.496
#&gt; GSM11320     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28797     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28786     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28800     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11310     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28787     1  0.0376     0.9800 0.996 0.004
#&gt; GSM11304     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11303     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11317     1  0.0376     0.9800 0.996 0.004
#&gt; GSM11311     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28799     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28791     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28794     2  0.0376     0.9959 0.004 0.996
#&gt; GSM28780     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28795     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11301     2  0.0000     0.9996 0.000 1.000
#&gt; GSM11297     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11298     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11314     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11299     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28783     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11308     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28782     1  0.0000     0.9835 1.000 0.000
#&gt; GSM28779     1  0.0000     0.9835 1.000 0.000
#&gt; GSM11302     1  0.0000     0.9835 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1  p2    p3
#&gt; GSM28788     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28789     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28790     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11300     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM28798     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11296     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM28801     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11319     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM28781     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11305     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM28784     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11307     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11313     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM28785     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11318     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28792     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11295     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM28793     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11312     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28778     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28796     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11309     1   0.455      0.758 0.800 0.0 0.200
#&gt; GSM11315     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11306     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28776     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28777     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM11316     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM11320     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM28797     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28786     1   0.571      0.549 0.680 0.0 0.320
#&gt; GSM28800     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11310     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28787     3   0.525      0.612 0.264 0.0 0.736
#&gt; GSM11304     1   0.455      0.758 0.800 0.0 0.200
#&gt; GSM11303     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM11317     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM11311     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28799     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28791     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28794     2   0.296      0.847 0.100 0.9 0.000
#&gt; GSM28780     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28795     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11301     2   0.000      0.987 0.000 1.0 0.000
#&gt; GSM11297     1   0.418      0.794 0.828 0.0 0.172
#&gt; GSM11298     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11314     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11299     3   0.000      0.954 0.000 0.0 1.000
#&gt; GSM28783     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11308     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28782     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM28779     1   0.000      0.968 1.000 0.0 0.000
#&gt; GSM11302     1   0.000      0.968 1.000 0.0 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.4382    0.49304 0.296 0.000 0.000 0.704
#&gt; GSM28789     4  0.4277    0.50983 0.280 0.000 0.000 0.720
#&gt; GSM28790     1  0.2149    0.43547 0.912 0.000 0.000 0.088
#&gt; GSM11300     3  0.3764    0.79241 0.000 0.000 0.784 0.216
#&gt; GSM28798     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0000    0.43527 1.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000    0.43527 1.000 0.000 0.000 0.000
#&gt; GSM11295     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.0188    0.43466 0.996 0.000 0.000 0.004
#&gt; GSM11312     1  0.4877    0.32547 0.592 0.000 0.000 0.408
#&gt; GSM28778     1  0.4679    0.38983 0.648 0.000 0.000 0.352
#&gt; GSM28796     1  0.0000    0.43527 1.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0524    0.45011 0.008 0.000 0.004 0.988
#&gt; GSM11315     1  0.0000    0.43527 1.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.4830    0.34839 0.392 0.000 0.000 0.608
#&gt; GSM28776     1  0.3610    0.40447 0.800 0.000 0.000 0.200
#&gt; GSM28777     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.4193    0.49800 0.268 0.000 0.000 0.732
#&gt; GSM28786     4  0.3172    0.54191 0.160 0.000 0.000 0.840
#&gt; GSM28800     1  0.4992    0.00338 0.524 0.000 0.000 0.476
#&gt; GSM11310     4  0.3528    0.51962 0.192 0.000 0.000 0.808
#&gt; GSM28787     3  0.5650    0.66040 0.104 0.000 0.716 0.180
#&gt; GSM11304     1  0.7299    0.01242 0.520 0.000 0.184 0.296
#&gt; GSM11303     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000    0.90624 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.4998    0.23547 0.488 0.000 0.000 0.512
#&gt; GSM28799     4  0.4605    0.18233 0.336 0.000 0.000 0.664
#&gt; GSM28791     1  0.4804    0.36404 0.616 0.000 0.000 0.384
#&gt; GSM28794     2  0.2888    0.81525 0.004 0.872 0.000 0.124
#&gt; GSM28780     1  0.4804    0.36298 0.616 0.000 0.000 0.384
#&gt; GSM28795     4  0.4830    0.08400 0.392 0.000 0.000 0.608
#&gt; GSM11301     2  0.0000    0.98435 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.7344   -0.00576 0.504 0.000 0.180 0.316
#&gt; GSM11298     1  0.4679    0.38983 0.648 0.000 0.000 0.352
#&gt; GSM11314     1  0.4730    0.37219 0.636 0.000 0.000 0.364
#&gt; GSM11299     3  0.4331    0.73317 0.000 0.000 0.712 0.288
#&gt; GSM28783     1  0.4898    0.30275 0.584 0.000 0.000 0.416
#&gt; GSM11308     4  0.4790    0.12584 0.380 0.000 0.000 0.620
#&gt; GSM28782     1  0.4877    0.32020 0.592 0.000 0.000 0.408
#&gt; GSM28779     1  0.4679    0.38983 0.648 0.000 0.000 0.352
#&gt; GSM11302     1  0.4679    0.38983 0.648 0.000 0.000 0.352
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     5  0.3662     0.4775 0.252 0.000 0.000 0.004 0.744
#&gt; GSM28789     5  0.4698     0.4898 0.172 0.000 0.000 0.096 0.732
#&gt; GSM28790     5  0.4430    -0.2287 0.456 0.000 0.000 0.004 0.540
#&gt; GSM11300     3  0.4211     0.5883 0.360 0.000 0.636 0.004 0.000
#&gt; GSM28798     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4383     0.3741 0.572 0.000 0.000 0.004 0.424
#&gt; GSM28792     1  0.4383     0.3741 0.572 0.000 0.000 0.004 0.424
#&gt; GSM11295     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     5  0.4446    -0.3858 0.476 0.000 0.000 0.004 0.520
#&gt; GSM11312     5  0.0404     0.6121 0.012 0.000 0.000 0.000 0.988
#&gt; GSM28778     5  0.1608     0.5928 0.072 0.000 0.000 0.000 0.928
#&gt; GSM28796     1  0.4383     0.3741 0.572 0.000 0.000 0.004 0.424
#&gt; GSM11309     4  0.0162     0.9928 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11315     1  0.4430     0.3430 0.540 0.000 0.000 0.004 0.456
#&gt; GSM11306     5  0.3109     0.5196 0.000 0.000 0.000 0.200 0.800
#&gt; GSM28776     5  0.3790     0.1293 0.272 0.000 0.000 0.004 0.724
#&gt; GSM28777     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.0290     0.9891 0.000 0.000 0.000 0.992 0.008
#&gt; GSM28786     4  0.0162     0.9928 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28800     1  0.4101     0.0713 0.664 0.000 0.000 0.004 0.332
#&gt; GSM11310     5  0.6303     0.2850 0.268 0.000 0.000 0.204 0.528
#&gt; GSM28787     3  0.4816     0.6289 0.096 0.000 0.732 0.004 0.168
#&gt; GSM11304     1  0.2813     0.3442 0.880 0.000 0.084 0.004 0.032
#&gt; GSM11303     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.8470 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.0162     0.9892 0.004 0.000 0.000 0.996 0.000
#&gt; GSM28799     5  0.4403     0.2370 0.436 0.000 0.000 0.004 0.560
#&gt; GSM28791     5  0.1608     0.5958 0.072 0.000 0.000 0.000 0.928
#&gt; GSM28794     2  0.2690     0.7607 0.000 0.844 0.000 0.000 0.156
#&gt; GSM28780     5  0.2179     0.5803 0.100 0.000 0.000 0.004 0.896
#&gt; GSM28795     5  0.3661     0.4570 0.276 0.000 0.000 0.000 0.724
#&gt; GSM11301     2  0.0000     0.9801 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     1  0.2888     0.3728 0.880 0.000 0.060 0.004 0.056
#&gt; GSM11298     5  0.0000     0.6118 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11314     5  0.2648     0.5340 0.152 0.000 0.000 0.000 0.848
#&gt; GSM11299     3  0.4383     0.5189 0.424 0.000 0.572 0.004 0.000
#&gt; GSM28783     5  0.3949     0.4810 0.332 0.000 0.000 0.000 0.668
#&gt; GSM11308     1  0.4450    -0.3496 0.508 0.000 0.000 0.004 0.488
#&gt; GSM28782     5  0.3689     0.4796 0.256 0.000 0.000 0.004 0.740
#&gt; GSM28779     5  0.0000     0.6118 0.000 0.000 0.000 0.000 1.000
#&gt; GSM11302     5  0.0000     0.6118 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     5  0.2378     0.7188 0.000 0.000 0.000 0.000 0.848 0.152
#&gt; GSM28789     5  0.1341     0.7885 0.000 0.000 0.000 0.024 0.948 0.028
#&gt; GSM28790     1  0.3244     0.5831 0.732 0.000 0.000 0.000 0.268 0.000
#&gt; GSM11300     6  0.3050     0.5708 0.000 0.000 0.236 0.000 0.000 0.764
#&gt; GSM28798     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.1075     0.8855 0.952 0.000 0.000 0.000 0.048 0.000
#&gt; GSM28792     1  0.0937     0.8905 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; GSM11295     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28793     1  0.0790     0.8779 0.968 0.000 0.000 0.000 0.032 0.000
#&gt; GSM11312     5  0.0000     0.7983 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28778     5  0.1196     0.7909 0.040 0.000 0.000 0.000 0.952 0.008
#&gt; GSM28796     1  0.0146     0.8960 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11309     4  0.0000     0.9984 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11315     1  0.0146     0.8960 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11306     5  0.1421     0.7903 0.028 0.000 0.000 0.028 0.944 0.000
#&gt; GSM28776     5  0.3867     0.0539 0.488 0.000 0.000 0.000 0.512 0.000
#&gt; GSM28777     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0000     0.9984 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28786     4  0.0000     0.9984 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28800     6  0.5799     0.2616 0.192 0.000 0.000 0.000 0.340 0.468
#&gt; GSM11310     6  0.6708     0.2667 0.048 0.000 0.000 0.204 0.336 0.412
#&gt; GSM28787     3  0.5575     0.1171 0.000 0.000 0.528 0.000 0.168 0.304
#&gt; GSM11304     6  0.2320     0.7061 0.080 0.000 0.024 0.000 0.004 0.892
#&gt; GSM11303     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9122 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.0146     0.9952 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; GSM28799     6  0.3175     0.5861 0.000 0.000 0.000 0.000 0.256 0.744
#&gt; GSM28791     5  0.2350     0.7628 0.036 0.000 0.000 0.000 0.888 0.076
#&gt; GSM28794     2  0.3288     0.5512 0.000 0.724 0.000 0.000 0.276 0.000
#&gt; GSM28780     6  0.4127     0.4359 0.036 0.000 0.000 0.000 0.284 0.680
#&gt; GSM28795     5  0.4463     0.4144 0.036 0.000 0.000 0.000 0.588 0.376
#&gt; GSM11301     2  0.0000     0.9666 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.2113     0.7003 0.092 0.000 0.008 0.000 0.004 0.896
#&gt; GSM11298     5  0.1327     0.7899 0.064 0.000 0.000 0.000 0.936 0.000
#&gt; GSM11314     5  0.4442     0.6438 0.120 0.000 0.000 0.000 0.712 0.168
#&gt; GSM11299     6  0.1910     0.6771 0.000 0.000 0.108 0.000 0.000 0.892
#&gt; GSM28783     5  0.3756     0.4357 0.000 0.000 0.000 0.000 0.600 0.400
#&gt; GSM11308     6  0.0000     0.6951 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28782     5  0.2823     0.6467 0.000 0.000 0.000 0.000 0.796 0.204
#&gt; GSM28779     5  0.0260     0.7986 0.008 0.000 0.000 0.000 0.992 0.000
#&gt; GSM11302     5  0.0632     0.7991 0.024 0.000 0.000 0.000 0.976 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:pam 51     0.395 2
#> CV:pam 52     0.372 3
#> CV:pam 24     0.392 4
#> CV:pam 34     0.398 5
#> CV:pam 45     0.475 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.699           0.743       0.879         0.4260 0.509   0.509
#> 3 3 0.891           0.882       0.949         0.4216 0.829   0.686
#> 4 4 0.740           0.781       0.855         0.2018 0.854   0.661
#> 5 5 0.730           0.651       0.821         0.0827 0.878   0.601
#> 6 6 0.698           0.525       0.711         0.0280 0.950   0.749
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   1.000      0.963 0.504 0.496
#&gt; GSM28789     1   1.000      0.958 0.500 0.500
#&gt; GSM28790     1   1.000      0.963 0.504 0.496
#&gt; GSM11300     2   0.000      0.401 0.000 1.000
#&gt; GSM28798     2   1.000      0.695 0.496 0.504
#&gt; GSM11296     2   1.000      0.695 0.496 0.504
#&gt; GSM28801     2   1.000      0.695 0.496 0.504
#&gt; GSM11319     2   1.000      0.695 0.496 0.504
#&gt; GSM28781     2   1.000      0.695 0.496 0.504
#&gt; GSM11305     2   1.000      0.695 0.496 0.504
#&gt; GSM28784     2   1.000      0.695 0.496 0.504
#&gt; GSM11307     2   1.000      0.695 0.496 0.504
#&gt; GSM11313     2   1.000      0.695 0.496 0.504
#&gt; GSM28785     2   1.000      0.695 0.496 0.504
#&gt; GSM11318     1   1.000      0.963 0.504 0.496
#&gt; GSM28792     1   1.000      0.963 0.504 0.496
#&gt; GSM11295     2   0.000      0.401 0.000 1.000
#&gt; GSM28793     1   1.000      0.963 0.504 0.496
#&gt; GSM11312     1   1.000      0.963 0.504 0.496
#&gt; GSM28778     1   1.000      0.963 0.504 0.496
#&gt; GSM28796     1   1.000      0.963 0.504 0.496
#&gt; GSM11309     1   1.000      0.963 0.504 0.496
#&gt; GSM11315     1   1.000      0.963 0.504 0.496
#&gt; GSM11306     1   1.000      0.963 0.504 0.496
#&gt; GSM28776     1   1.000      0.963 0.504 0.496
#&gt; GSM28777     2   0.000      0.401 0.000 1.000
#&gt; GSM11316     2   0.000      0.401 0.000 1.000
#&gt; GSM11320     2   0.000      0.401 0.000 1.000
#&gt; GSM28797     1   1.000      0.963 0.504 0.496
#&gt; GSM28786     1   1.000      0.963 0.504 0.496
#&gt; GSM28800     1   1.000      0.963 0.504 0.496
#&gt; GSM11310     1   1.000      0.963 0.504 0.496
#&gt; GSM28787     2   0.000      0.401 0.000 1.000
#&gt; GSM11304     1   1.000      0.963 0.504 0.496
#&gt; GSM11303     2   0.000      0.401 0.000 1.000
#&gt; GSM11317     2   0.000      0.401 0.000 1.000
#&gt; GSM11311     1   1.000      0.963 0.504 0.496
#&gt; GSM28799     1   1.000      0.963 0.504 0.496
#&gt; GSM28791     1   1.000      0.963 0.504 0.496
#&gt; GSM28794     1   0.917     -0.594 0.668 0.332
#&gt; GSM28780     1   1.000      0.963 0.504 0.496
#&gt; GSM28795     1   1.000      0.963 0.504 0.496
#&gt; GSM11301     2   1.000      0.694 0.492 0.508
#&gt; GSM11297     1   1.000      0.963 0.504 0.496
#&gt; GSM11298     1   1.000      0.963 0.504 0.496
#&gt; GSM11314     2   1.000     -0.950 0.492 0.508
#&gt; GSM11299     2   0.000      0.401 0.000 1.000
#&gt; GSM28783     1   1.000      0.963 0.504 0.496
#&gt; GSM11308     1   1.000      0.963 0.504 0.496
#&gt; GSM28782     1   1.000      0.963 0.504 0.496
#&gt; GSM28779     1   1.000      0.963 0.504 0.496
#&gt; GSM11302     1   1.000      0.963 0.504 0.496
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.2448     0.8913 0.924 0.000 0.076
#&gt; GSM28789     1  0.4642     0.8387 0.856 0.060 0.084
#&gt; GSM28790     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11318     1  0.0237     0.9179 0.996 0.000 0.004
#&gt; GSM28792     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM28778     1  0.0661     0.9155 0.988 0.008 0.004
#&gt; GSM28796     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11309     1  0.2448     0.8913 0.924 0.000 0.076
#&gt; GSM11315     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11306     1  0.2448     0.8913 0.924 0.000 0.076
#&gt; GSM28776     1  0.0892     0.9146 0.980 0.000 0.020
#&gt; GSM28777     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM28797     1  0.2448     0.8913 0.924 0.000 0.076
#&gt; GSM28786     1  0.2448     0.8913 0.924 0.000 0.076
#&gt; GSM28800     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11310     1  0.0892     0.9146 0.980 0.000 0.020
#&gt; GSM28787     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM11304     1  0.6154     0.3620 0.592 0.000 0.408
#&gt; GSM11303     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM11311     1  0.2356     0.8953 0.928 0.000 0.072
#&gt; GSM28799     1  0.4796     0.7311 0.780 0.000 0.220
#&gt; GSM28791     1  0.0424     0.9173 0.992 0.000 0.008
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM28780     1  0.1289     0.9080 0.968 0.000 0.032
#&gt; GSM28795     1  0.4121     0.7849 0.832 0.000 0.168
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM11297     3  0.6299    -0.0716 0.476 0.000 0.524
#&gt; GSM11298     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11314     1  0.6154     0.3596 0.592 0.000 0.408
#&gt; GSM11299     3  0.0000     0.9314 0.000 0.000 1.000
#&gt; GSM28783     1  0.0747     0.9150 0.984 0.000 0.016
#&gt; GSM11308     1  0.5363     0.6277 0.724 0.000 0.276
#&gt; GSM28782     1  0.0424     0.9173 0.992 0.000 0.008
#&gt; GSM28779     1  0.0000     0.9182 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.9182 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.2197      0.850 0.080 0.004 0.000 0.916
#&gt; GSM28789     4  0.2300      0.842 0.028 0.048 0.000 0.924
#&gt; GSM28790     1  0.0336      0.718 0.992 0.000 0.000 0.008
#&gt; GSM11300     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28798     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0592      0.720 0.984 0.000 0.000 0.016
#&gt; GSM28792     1  0.3377      0.728 0.848 0.000 0.012 0.140
#&gt; GSM11295     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.4431      0.654 0.696 0.000 0.000 0.304
#&gt; GSM11312     1  0.3400      0.721 0.820 0.000 0.000 0.180
#&gt; GSM28778     1  0.4730      0.178 0.636 0.000 0.000 0.364
#&gt; GSM28796     1  0.3942      0.704 0.764 0.000 0.000 0.236
#&gt; GSM11309     4  0.0000      0.879 0.000 0.000 0.000 1.000
#&gt; GSM11315     1  0.4222      0.683 0.728 0.000 0.000 0.272
#&gt; GSM11306     4  0.3982      0.663 0.220 0.004 0.000 0.776
#&gt; GSM28776     1  0.4905      0.575 0.632 0.004 0.000 0.364
#&gt; GSM28777     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.0000      0.879 0.000 0.000 0.000 1.000
#&gt; GSM28786     4  0.0000      0.879 0.000 0.000 0.000 1.000
#&gt; GSM28800     1  0.3975      0.702 0.760 0.000 0.000 0.240
#&gt; GSM11310     1  0.4999      0.359 0.508 0.000 0.000 0.492
#&gt; GSM28787     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11304     1  0.7762      0.300 0.428 0.000 0.316 0.256
#&gt; GSM11303     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.3024      0.778 0.148 0.000 0.000 0.852
#&gt; GSM28799     1  0.7740      0.292 0.428 0.000 0.244 0.328
#&gt; GSM28791     1  0.0000      0.715 1.000 0.000 0.000 0.000
#&gt; GSM28794     2  0.4277      0.611 0.000 0.720 0.000 0.280
#&gt; GSM28780     1  0.0000      0.715 1.000 0.000 0.000 0.000
#&gt; GSM28795     1  0.0921      0.710 0.972 0.000 0.000 0.028
#&gt; GSM11301     2  0.0000      0.973 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.7762      0.299 0.428 0.000 0.316 0.256
#&gt; GSM11298     1  0.3907      0.706 0.768 0.000 0.000 0.232
#&gt; GSM11314     1  0.4890      0.584 0.776 0.000 0.080 0.144
#&gt; GSM11299     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM28783     1  0.0000      0.715 1.000 0.000 0.000 0.000
#&gt; GSM11308     1  0.3024      0.634 0.852 0.000 0.000 0.148
#&gt; GSM28782     1  0.0921      0.722 0.972 0.000 0.000 0.028
#&gt; GSM28779     1  0.4193      0.686 0.732 0.000 0.000 0.268
#&gt; GSM11302     1  0.3907      0.706 0.768 0.000 0.000 0.232
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.3861    0.65738 0.284 0.000 0.000 0.712 0.004
#&gt; GSM28789     4  0.4687    0.69080 0.200 0.052 0.000 0.736 0.012
#&gt; GSM28790     5  0.4450    0.17112 0.488 0.000 0.000 0.004 0.508
#&gt; GSM11300     3  0.0404    0.96880 0.000 0.000 0.988 0.000 0.012
#&gt; GSM28798     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.95661 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4367   -0.10995 0.580 0.000 0.000 0.004 0.416
#&gt; GSM28792     1  0.3906    0.36480 0.744 0.000 0.000 0.016 0.240
#&gt; GSM11295     3  0.0703    0.96449 0.000 0.000 0.976 0.000 0.024
#&gt; GSM28793     1  0.2419    0.64665 0.904 0.000 0.004 0.064 0.028
#&gt; GSM11312     1  0.3421    0.62112 0.788 0.000 0.000 0.008 0.204
#&gt; GSM28778     5  0.5341    0.27950 0.356 0.000 0.000 0.064 0.580
#&gt; GSM28796     1  0.0324    0.66627 0.992 0.000 0.000 0.004 0.004
#&gt; GSM11309     4  0.0566    0.71664 0.012 0.000 0.000 0.984 0.004
#&gt; GSM11315     1  0.1768    0.65457 0.924 0.000 0.000 0.072 0.004
#&gt; GSM11306     4  0.4196    0.56527 0.356 0.000 0.000 0.640 0.004
#&gt; GSM28776     1  0.5740    0.40748 0.616 0.000 0.000 0.152 0.232
#&gt; GSM28777     3  0.0000    0.97173 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000    0.97173 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000    0.97173 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.1608    0.72520 0.072 0.000 0.000 0.928 0.000
#&gt; GSM28786     4  0.0451    0.71467 0.008 0.000 0.000 0.988 0.004
#&gt; GSM28800     1  0.0451    0.66825 0.988 0.000 0.000 0.004 0.008
#&gt; GSM11310     4  0.6224    0.26910 0.152 0.000 0.000 0.496 0.352
#&gt; GSM28787     3  0.2929    0.81151 0.000 0.000 0.820 0.000 0.180
#&gt; GSM11304     5  0.7640    0.03170 0.228 0.000 0.080 0.224 0.468
#&gt; GSM11303     3  0.0000    0.97173 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000    0.97173 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.5769    0.58543 0.144 0.000 0.004 0.628 0.224
#&gt; GSM28799     5  0.7307    0.03498 0.264 0.000 0.036 0.248 0.452
#&gt; GSM28791     5  0.4074    0.37217 0.364 0.000 0.000 0.000 0.636
#&gt; GSM28794     2  0.4763    0.44286 0.000 0.632 0.000 0.336 0.032
#&gt; GSM28780     5  0.3521    0.51063 0.232 0.000 0.000 0.004 0.764
#&gt; GSM28795     5  0.3048    0.52424 0.176 0.000 0.000 0.004 0.820
#&gt; GSM11301     2  0.2193    0.87601 0.000 0.912 0.000 0.060 0.028
#&gt; GSM11297     5  0.8029    0.00739 0.192 0.000 0.144 0.224 0.440
#&gt; GSM11298     1  0.2970    0.64934 0.828 0.000 0.000 0.004 0.168
#&gt; GSM11314     5  0.3804    0.49673 0.160 0.000 0.000 0.044 0.796
#&gt; GSM11299     3  0.0880    0.96194 0.000 0.000 0.968 0.000 0.032
#&gt; GSM28783     5  0.3814    0.49160 0.276 0.000 0.000 0.004 0.720
#&gt; GSM11308     5  0.3264    0.52414 0.164 0.000 0.000 0.016 0.820
#&gt; GSM28782     5  0.4443    0.19120 0.472 0.000 0.000 0.004 0.524
#&gt; GSM28779     1  0.3209    0.64787 0.812 0.000 0.000 0.008 0.180
#&gt; GSM11302     1  0.3010    0.64923 0.824 0.000 0.000 0.004 0.172
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4   0.377     0.5067 0.204 0.000 0.000 0.752 0.000 0.044
#&gt; GSM28789     4   0.389     0.5101 0.192 0.000 0.000 0.756 0.004 0.048
#&gt; GSM28790     5   0.553     0.4563 0.336 0.000 0.000 0.000 0.516 0.148
#&gt; GSM11300     3   0.347     0.9034 0.000 0.000 0.804 0.000 0.068 0.128
#&gt; GSM28798     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.238     0.8702 0.000 0.884 0.000 0.000 0.084 0.032
#&gt; GSM11319     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.026     0.9164 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11305     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.238     0.8702 0.000 0.884 0.000 0.000 0.084 0.032
#&gt; GSM11307     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000     0.9186 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     5   0.386     0.2602 0.480 0.000 0.000 0.000 0.520 0.000
#&gt; GSM28792     1   0.408    -0.0760 0.648 0.000 0.004 0.008 0.336 0.004
#&gt; GSM11295     3   0.330     0.9088 0.000 0.000 0.820 0.000 0.068 0.112
#&gt; GSM28793     1   0.407     0.4588 0.756 0.000 0.004 0.160 0.000 0.080
#&gt; GSM11312     1   0.442     0.3366 0.604 0.000 0.000 0.000 0.036 0.360
#&gt; GSM28778     5   0.633     0.3498 0.160 0.000 0.000 0.036 0.468 0.336
#&gt; GSM28796     1   0.079     0.4882 0.968 0.000 0.000 0.000 0.032 0.000
#&gt; GSM11309     4   0.514     0.5589 0.000 0.000 0.000 0.612 0.248 0.140
#&gt; GSM11315     1   0.310     0.4290 0.800 0.000 0.000 0.184 0.000 0.016
#&gt; GSM11306     4   0.367     0.4488 0.268 0.000 0.000 0.716 0.000 0.016
#&gt; GSM28776     6   0.617     0.0426 0.368 0.000 0.000 0.252 0.004 0.376
#&gt; GSM28777     3   0.186     0.9193 0.000 0.000 0.920 0.000 0.032 0.048
#&gt; GSM11316     3   0.000     0.9172 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3   0.000     0.9172 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4   0.543     0.5663 0.024 0.000 0.000 0.620 0.248 0.108
#&gt; GSM28786     4   0.514     0.5589 0.000 0.000 0.000 0.612 0.248 0.140
#&gt; GSM28800     1   0.026     0.5100 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11310     4   0.530    -0.1253 0.076 0.000 0.000 0.540 0.012 0.372
#&gt; GSM28787     3   0.334     0.9075 0.000 0.000 0.816 0.000 0.068 0.116
#&gt; GSM11304     6   0.615     0.2740 0.208 0.000 0.024 0.248 0.000 0.520
#&gt; GSM11303     3   0.000     0.9172 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3   0.000     0.9172 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4   0.449     0.3489 0.072 0.000 0.000 0.692 0.004 0.232
#&gt; GSM28799     6   0.594     0.2587 0.220 0.000 0.008 0.260 0.000 0.512
#&gt; GSM28791     6   0.533    -0.3168 0.104 0.000 0.000 0.000 0.444 0.452
#&gt; GSM28794     2   0.625     0.3437 0.004 0.512 0.000 0.332 0.092 0.060
#&gt; GSM28780     6   0.502    -0.2005 0.076 0.000 0.000 0.000 0.396 0.528
#&gt; GSM28795     6   0.450    -0.1526 0.036 0.000 0.000 0.000 0.392 0.572
#&gt; GSM11301     2   0.448     0.7452 0.000 0.756 0.000 0.120 0.084 0.040
#&gt; GSM11297     6   0.651     0.2815 0.168 0.000 0.068 0.244 0.000 0.520
#&gt; GSM11298     1   0.365     0.3746 0.640 0.000 0.000 0.000 0.000 0.360
#&gt; GSM11314     5   0.594     0.3127 0.084 0.000 0.000 0.044 0.484 0.388
#&gt; GSM11299     3   0.363     0.8957 0.000 0.000 0.788 0.000 0.068 0.144
#&gt; GSM28783     5   0.511     0.3678 0.088 0.000 0.000 0.000 0.536 0.376
#&gt; GSM11308     6   0.457    -0.1342 0.028 0.000 0.000 0.008 0.376 0.588
#&gt; GSM28782     5   0.568     0.4593 0.200 0.000 0.000 0.000 0.520 0.280
#&gt; GSM28779     1   0.401     0.3711 0.632 0.000 0.000 0.004 0.008 0.356
#&gt; GSM11302     1   0.365     0.3746 0.640 0.000 0.000 0.000 0.000 0.360
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n tissue(p) k
#> CV:mclust 41     0.383 2
#> CV:mclust 49     0.368 3
#> CV:mclust 47     0.450 4
#> CV:mclust 38     0.417 5
#> CV:mclust 26     0.384 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.951       0.983         0.3819 0.618   0.618
#> 3 3 0.937           0.931       0.974         0.5529 0.750   0.610
#> 4 4 0.755           0.762       0.867         0.2333 0.854   0.652
#> 5 5 0.923           0.875       0.943         0.1004 0.896   0.639
#> 6 6 0.906           0.854       0.915         0.0402 0.952   0.759
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 5
```

There is also optional best $k$ = 2 3 5 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.988 1.000 0.000
#&gt; GSM28789     1  0.9775      0.254 0.588 0.412
#&gt; GSM28790     1  0.0000      0.988 1.000 0.000
#&gt; GSM11300     1  0.0000      0.988 1.000 0.000
#&gt; GSM28798     2  0.0000      0.961 0.000 1.000
#&gt; GSM11296     2  0.0000      0.961 0.000 1.000
#&gt; GSM28801     2  0.0000      0.961 0.000 1.000
#&gt; GSM11319     2  0.0000      0.961 0.000 1.000
#&gt; GSM28781     2  0.0000      0.961 0.000 1.000
#&gt; GSM11305     2  0.0000      0.961 0.000 1.000
#&gt; GSM28784     2  0.0000      0.961 0.000 1.000
#&gt; GSM11307     2  0.0000      0.961 0.000 1.000
#&gt; GSM11313     2  0.0000      0.961 0.000 1.000
#&gt; GSM28785     2  0.0000      0.961 0.000 1.000
#&gt; GSM11318     1  0.0000      0.988 1.000 0.000
#&gt; GSM28792     1  0.0000      0.988 1.000 0.000
#&gt; GSM11295     1  0.0000      0.988 1.000 0.000
#&gt; GSM28793     1  0.0000      0.988 1.000 0.000
#&gt; GSM11312     1  0.0000      0.988 1.000 0.000
#&gt; GSM28778     2  0.9933      0.152 0.452 0.548
#&gt; GSM28796     1  0.0000      0.988 1.000 0.000
#&gt; GSM11309     1  0.0000      0.988 1.000 0.000
#&gt; GSM11315     1  0.0000      0.988 1.000 0.000
#&gt; GSM11306     1  0.0000      0.988 1.000 0.000
#&gt; GSM28776     1  0.0000      0.988 1.000 0.000
#&gt; GSM28777     1  0.0000      0.988 1.000 0.000
#&gt; GSM11316     1  0.1184      0.972 0.984 0.016
#&gt; GSM11320     1  0.0000      0.988 1.000 0.000
#&gt; GSM28797     1  0.0000      0.988 1.000 0.000
#&gt; GSM28786     1  0.0000      0.988 1.000 0.000
#&gt; GSM28800     1  0.0000      0.988 1.000 0.000
#&gt; GSM11310     1  0.0000      0.988 1.000 0.000
#&gt; GSM28787     1  0.0376      0.984 0.996 0.004
#&gt; GSM11304     1  0.0000      0.988 1.000 0.000
#&gt; GSM11303     1  0.0000      0.988 1.000 0.000
#&gt; GSM11317     1  0.0000      0.988 1.000 0.000
#&gt; GSM11311     1  0.0000      0.988 1.000 0.000
#&gt; GSM28799     1  0.0000      0.988 1.000 0.000
#&gt; GSM28791     1  0.0000      0.988 1.000 0.000
#&gt; GSM28794     2  0.0000      0.961 0.000 1.000
#&gt; GSM28780     1  0.0000      0.988 1.000 0.000
#&gt; GSM28795     1  0.0000      0.988 1.000 0.000
#&gt; GSM11301     2  0.0000      0.961 0.000 1.000
#&gt; GSM11297     1  0.0000      0.988 1.000 0.000
#&gt; GSM11298     1  0.0000      0.988 1.000 0.000
#&gt; GSM11314     1  0.0000      0.988 1.000 0.000
#&gt; GSM11299     1  0.0000      0.988 1.000 0.000
#&gt; GSM28783     1  0.0000      0.988 1.000 0.000
#&gt; GSM11308     1  0.0000      0.988 1.000 0.000
#&gt; GSM28782     1  0.0000      0.988 1.000 0.000
#&gt; GSM28779     1  0.0000      0.988 1.000 0.000
#&gt; GSM11302     1  0.0000      0.988 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28789     1  0.3267      0.853 0.884 0.116 0.000
#&gt; GSM28790     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11309     1  0.5621      0.556 0.692 0.000 0.308
#&gt; GSM11315     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28786     3  0.6267      0.105 0.452 0.000 0.548
#&gt; GSM28800     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM11304     1  0.3619      0.829 0.864 0.000 0.136
#&gt; GSM11303     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM11311     1  0.0237      0.964 0.996 0.000 0.004
#&gt; GSM28799     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28780     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11297     1  0.5706      0.531 0.680 0.000 0.320
#&gt; GSM11298     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11299     3  0.0000      0.932 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.967 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.967 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.0000     0.7964 0.000 0.000 0.000 1.000
#&gt; GSM28789     4  0.2635     0.7415 0.016 0.072 0.004 0.908
#&gt; GSM28790     1  0.2281     0.6764 0.904 0.000 0.000 0.096
#&gt; GSM11300     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.2149     0.6762 0.912 0.000 0.000 0.088
#&gt; GSM28792     1  0.4164     0.6467 0.736 0.000 0.000 0.264
#&gt; GSM11295     3  0.0592     0.9832 0.016 0.000 0.984 0.000
#&gt; GSM28793     1  0.4543     0.6133 0.676 0.000 0.000 0.324
#&gt; GSM11312     1  0.4454     0.5867 0.692 0.000 0.000 0.308
#&gt; GSM28778     1  0.3311     0.6030 0.828 0.000 0.000 0.172
#&gt; GSM28796     1  0.4500     0.6202 0.684 0.000 0.000 0.316
#&gt; GSM11309     4  0.2216     0.7525 0.000 0.000 0.092 0.908
#&gt; GSM11315     1  0.4624     0.5935 0.660 0.000 0.000 0.340
#&gt; GSM11306     4  0.0592     0.7934 0.016 0.000 0.000 0.984
#&gt; GSM28776     1  0.4761     0.5421 0.628 0.000 0.000 0.372
#&gt; GSM28777     3  0.0188     0.9903 0.000 0.000 0.996 0.004
#&gt; GSM11316     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.0000     0.7964 0.000 0.000 0.000 1.000
#&gt; GSM28786     4  0.2216     0.7571 0.000 0.000 0.092 0.908
#&gt; GSM28800     1  0.4564     0.6089 0.672 0.000 0.000 0.328
#&gt; GSM11310     4  0.4730     0.2203 0.364 0.000 0.000 0.636
#&gt; GSM28787     3  0.1109     0.9699 0.028 0.000 0.968 0.004
#&gt; GSM11304     1  0.7084     0.4560 0.560 0.000 0.264 0.176
#&gt; GSM11303     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.1557     0.7644 0.056 0.000 0.000 0.944
#&gt; GSM28799     4  0.5244     0.0428 0.388 0.000 0.012 0.600
#&gt; GSM28791     1  0.2011     0.6451 0.920 0.000 0.000 0.080
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28780     1  0.3306     0.6037 0.840 0.000 0.004 0.156
#&gt; GSM28795     1  0.3448     0.5922 0.828 0.000 0.004 0.168
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.7398     0.2181 0.424 0.000 0.412 0.164
#&gt; GSM11298     1  0.4454     0.6262 0.692 0.000 0.000 0.308
#&gt; GSM11314     1  0.3545     0.5940 0.828 0.000 0.008 0.164
#&gt; GSM11299     3  0.0000     0.9934 0.000 0.000 1.000 0.000
#&gt; GSM28783     1  0.3157     0.6125 0.852 0.000 0.004 0.144
#&gt; GSM11308     1  0.2988     0.6286 0.876 0.000 0.012 0.112
#&gt; GSM28782     1  0.1118     0.6649 0.964 0.000 0.000 0.036
#&gt; GSM28779     1  0.4543     0.6133 0.676 0.000 0.000 0.324
#&gt; GSM11302     1  0.4454     0.6313 0.692 0.000 0.000 0.308
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM28788     4  0.0000     0.9145 0.000  0 0.000 1.000 0.000
#&gt; GSM28789     4  0.0000     0.9145 0.000  0 0.000 1.000 0.000
#&gt; GSM28790     1  0.1908     0.8361 0.908  0 0.000 0.000 0.092
#&gt; GSM11300     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM28798     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11318     1  0.1608     0.8454 0.928  0 0.000 0.000 0.072
#&gt; GSM28792     1  0.0404     0.8725 0.988  0 0.000 0.000 0.012
#&gt; GSM11295     3  0.0794     0.9642 0.000  0 0.972 0.000 0.028
#&gt; GSM28793     1  0.0162     0.8717 0.996  0 0.000 0.004 0.000
#&gt; GSM11312     5  0.5006     0.6655 0.116  0 0.000 0.180 0.704
#&gt; GSM28778     5  0.0609     0.9193 0.020  0 0.000 0.000 0.980
#&gt; GSM28796     1  0.0000     0.8718 1.000  0 0.000 0.000 0.000
#&gt; GSM11309     4  0.0000     0.9145 0.000  0 0.000 1.000 0.000
#&gt; GSM11315     1  0.0000     0.8718 1.000  0 0.000 0.000 0.000
#&gt; GSM11306     4  0.0290     0.9109 0.000  0 0.000 0.992 0.008
#&gt; GSM28776     1  0.1774     0.8514 0.932  0 0.000 0.052 0.016
#&gt; GSM28777     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM11316     3  0.0290     0.9758 0.000  0 0.992 0.000 0.008
#&gt; GSM11320     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM28797     4  0.0000     0.9145 0.000  0 0.000 1.000 0.000
#&gt; GSM28786     4  0.0000     0.9145 0.000  0 0.000 1.000 0.000
#&gt; GSM28800     1  0.0798     0.8725 0.976  0 0.000 0.008 0.016
#&gt; GSM11310     1  0.4166     0.4212 0.648  0 0.000 0.348 0.004
#&gt; GSM28787     3  0.3053     0.8549 0.012  0 0.852 0.008 0.128
#&gt; GSM11304     1  0.4848     0.5303 0.644  0 0.320 0.004 0.032
#&gt; GSM11303     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM11311     4  0.1197     0.8815 0.048  0 0.000 0.952 0.000
#&gt; GSM28799     4  0.4684     0.0849 0.452  0 0.004 0.536 0.008
#&gt; GSM28791     5  0.0609     0.9196 0.020  0 0.000 0.000 0.980
#&gt; GSM28794     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28780     5  0.0404     0.9217 0.012  0 0.000 0.000 0.988
#&gt; GSM28795     5  0.0290     0.9206 0.008  0 0.000 0.000 0.992
#&gt; GSM11301     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11297     1  0.5042     0.1954 0.512  0 0.460 0.004 0.024
#&gt; GSM11298     1  0.0798     0.8732 0.976  0 0.000 0.008 0.016
#&gt; GSM11314     5  0.0162     0.9175 0.004  0 0.000 0.000 0.996
#&gt; GSM11299     3  0.0000     0.9792 0.000  0 1.000 0.000 0.000
#&gt; GSM28783     5  0.0404     0.9217 0.012  0 0.000 0.000 0.988
#&gt; GSM11308     5  0.0290     0.9206 0.008  0 0.000 0.000 0.992
#&gt; GSM28782     5  0.3730     0.6079 0.288  0 0.000 0.000 0.712
#&gt; GSM28779     1  0.1331     0.8691 0.952  0 0.000 0.008 0.040
#&gt; GSM11302     1  0.1469     0.8691 0.948  0 0.000 0.016 0.036
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.0717      0.980 0.008 0.000 0.000 0.976 0.000 0.016
#&gt; GSM28789     4  0.0291      0.981 0.004 0.000 0.000 0.992 0.000 0.004
#&gt; GSM28790     1  0.1713      0.843 0.928 0.000 0.000 0.000 0.044 0.028
#&gt; GSM11300     3  0.2003      0.857 0.000 0.000 0.884 0.000 0.000 0.116
#&gt; GSM28798     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.995 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0837      0.866 0.972 0.000 0.004 0.000 0.004 0.020
#&gt; GSM28792     1  0.0922      0.864 0.968 0.000 0.004 0.000 0.004 0.024
#&gt; GSM11295     3  0.2981      0.826 0.020 0.000 0.848 0.000 0.016 0.116
#&gt; GSM28793     1  0.0146      0.876 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11312     5  0.4636      0.528 0.048 0.000 0.000 0.016 0.672 0.264
#&gt; GSM28778     5  0.0547      0.922 0.000 0.000 0.000 0.000 0.980 0.020
#&gt; GSM28796     1  0.0363      0.876 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM11309     4  0.0363      0.980 0.000 0.000 0.000 0.988 0.000 0.012
#&gt; GSM11315     1  0.0260      0.876 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM11306     4  0.0603      0.976 0.004 0.000 0.000 0.980 0.000 0.016
#&gt; GSM28776     1  0.3017      0.784 0.848 0.000 0.000 0.052 0.004 0.096
#&gt; GSM28777     3  0.0551      0.897 0.004 0.000 0.984 0.004 0.000 0.008
#&gt; GSM11316     3  0.0405      0.897 0.000 0.000 0.988 0.004 0.000 0.008
#&gt; GSM11320     3  0.0713      0.898 0.000 0.000 0.972 0.000 0.000 0.028
#&gt; GSM28797     4  0.0405      0.983 0.004 0.000 0.000 0.988 0.000 0.008
#&gt; GSM28786     4  0.0777      0.979 0.004 0.000 0.000 0.972 0.000 0.024
#&gt; GSM28800     6  0.4027      0.598 0.308 0.000 0.000 0.008 0.012 0.672
#&gt; GSM11310     6  0.4386      0.690 0.200 0.000 0.000 0.092 0.000 0.708
#&gt; GSM28787     3  0.5953      0.645 0.056 0.000 0.648 0.020 0.128 0.148
#&gt; GSM11304     6  0.3832      0.718 0.072 0.000 0.108 0.000 0.020 0.800
#&gt; GSM11303     3  0.0865      0.899 0.000 0.000 0.964 0.000 0.000 0.036
#&gt; GSM11317     3  0.0363      0.899 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11311     4  0.1176      0.970 0.020 0.000 0.000 0.956 0.000 0.024
#&gt; GSM28799     6  0.5460      0.603 0.256 0.000 0.016 0.124 0.000 0.604
#&gt; GSM28791     5  0.0146      0.920 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; GSM28794     2  0.1204      0.942 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; GSM28780     5  0.0632      0.921 0.000 0.000 0.000 0.000 0.976 0.024
#&gt; GSM28795     5  0.0146      0.920 0.000 0.000 0.000 0.000 0.996 0.004
#&gt; GSM11301     2  0.0146      0.991 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11297     6  0.4029      0.715 0.064 0.000 0.112 0.012 0.016 0.796
#&gt; GSM11298     1  0.0777      0.872 0.972 0.000 0.000 0.000 0.004 0.024
#&gt; GSM11314     5  0.0260      0.916 0.000 0.000 0.000 0.000 0.992 0.008
#&gt; GSM11299     3  0.2597      0.812 0.000 0.000 0.824 0.000 0.000 0.176
#&gt; GSM28783     5  0.1806      0.872 0.004 0.000 0.000 0.000 0.908 0.088
#&gt; GSM11308     5  0.1075      0.913 0.000 0.000 0.000 0.000 0.952 0.048
#&gt; GSM28782     6  0.4598      0.313 0.048 0.000 0.000 0.000 0.360 0.592
#&gt; GSM28779     1  0.4122     -0.165 0.520 0.000 0.000 0.004 0.004 0.472
#&gt; GSM11302     1  0.2373      0.806 0.880 0.000 0.000 0.008 0.008 0.104
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n tissue(p) k
#> CV:NMF 50     0.394 2
#> CV:NMF 51     0.371 3
#> CV:NMF 48     0.451 4
#> CV:NMF 49     0.436 5
#> CV:NMF 50     0.422 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.591           0.945       0.949         0.3466 0.660   0.660
#> 3 3 0.887           0.952       0.979         0.5536 0.821   0.728
#> 4 4 0.777           0.892       0.924         0.2761 0.857   0.703
#> 5 5 0.717           0.611       0.821         0.0898 0.893   0.691
#> 6 6 0.764           0.654       0.825         0.0655 0.902   0.637
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.000      0.959 1.000 0.000
#&gt; GSM28789     1   0.000      0.959 1.000 0.000
#&gt; GSM28790     1   0.000      0.959 1.000 0.000
#&gt; GSM11300     1   0.443      0.911 0.908 0.092
#&gt; GSM28798     2   0.494      1.000 0.108 0.892
#&gt; GSM11296     2   0.494      1.000 0.108 0.892
#&gt; GSM28801     2   0.494      1.000 0.108 0.892
#&gt; GSM11319     2   0.494      1.000 0.108 0.892
#&gt; GSM28781     2   0.494      1.000 0.108 0.892
#&gt; GSM11305     2   0.494      1.000 0.108 0.892
#&gt; GSM28784     2   0.494      1.000 0.108 0.892
#&gt; GSM11307     2   0.494      1.000 0.108 0.892
#&gt; GSM11313     2   0.494      1.000 0.108 0.892
#&gt; GSM28785     2   0.494      1.000 0.108 0.892
#&gt; GSM11318     1   0.000      0.959 1.000 0.000
#&gt; GSM28792     1   0.000      0.959 1.000 0.000
#&gt; GSM11295     1   0.494      0.900 0.892 0.108
#&gt; GSM28793     1   0.000      0.959 1.000 0.000
#&gt; GSM11312     1   0.000      0.959 1.000 0.000
#&gt; GSM28778     1   0.000      0.959 1.000 0.000
#&gt; GSM28796     1   0.000      0.959 1.000 0.000
#&gt; GSM11309     1   0.000      0.959 1.000 0.000
#&gt; GSM11315     1   0.000      0.959 1.000 0.000
#&gt; GSM11306     1   0.000      0.959 1.000 0.000
#&gt; GSM28776     1   0.000      0.959 1.000 0.000
#&gt; GSM28777     1   0.494      0.900 0.892 0.108
#&gt; GSM11316     1   0.494      0.900 0.892 0.108
#&gt; GSM11320     1   0.494      0.900 0.892 0.108
#&gt; GSM28797     1   0.000      0.959 1.000 0.000
#&gt; GSM28786     1   0.000      0.959 1.000 0.000
#&gt; GSM28800     1   0.000      0.959 1.000 0.000
#&gt; GSM11310     1   0.000      0.959 1.000 0.000
#&gt; GSM28787     1   0.494      0.900 0.892 0.108
#&gt; GSM11304     1   0.443      0.911 0.908 0.092
#&gt; GSM11303     1   0.494      0.900 0.892 0.108
#&gt; GSM11317     1   0.494      0.900 0.892 0.108
#&gt; GSM11311     1   0.000      0.959 1.000 0.000
#&gt; GSM28799     1   0.000      0.959 1.000 0.000
#&gt; GSM28791     1   0.000      0.959 1.000 0.000
#&gt; GSM28794     1   0.925      0.383 0.660 0.340
#&gt; GSM28780     1   0.000      0.959 1.000 0.000
#&gt; GSM28795     1   0.000      0.959 1.000 0.000
#&gt; GSM11301     2   0.494      1.000 0.108 0.892
#&gt; GSM11297     1   0.443      0.911 0.908 0.092
#&gt; GSM11298     1   0.000      0.959 1.000 0.000
#&gt; GSM11314     1   0.000      0.959 1.000 0.000
#&gt; GSM11299     1   0.443      0.911 0.908 0.092
#&gt; GSM28783     1   0.000      0.959 1.000 0.000
#&gt; GSM11308     1   0.000      0.959 1.000 0.000
#&gt; GSM28782     1   0.000      0.959 1.000 0.000
#&gt; GSM28779     1   0.000      0.959 1.000 0.000
#&gt; GSM11302     1   0.000      0.959 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3
#&gt; GSM28788     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28789     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28790     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11300     1   0.418      0.806 0.828 0.00 0.172
#&gt; GSM28798     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11296     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM28801     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11319     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM28781     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11305     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM28784     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11307     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11313     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM28785     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11318     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28792     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11295     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM28793     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11312     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28778     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28796     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11309     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11315     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11306     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28776     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28777     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM11316     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM11320     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM28797     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28786     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28800     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11310     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28787     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM11304     1   0.418      0.806 0.828 0.00 0.172
#&gt; GSM11303     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM11317     3   0.000      1.000 0.000 0.00 1.000
#&gt; GSM11311     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28799     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28791     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28794     1   0.619      0.311 0.580 0.42 0.000
#&gt; GSM28780     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28795     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11301     2   0.000      1.000 0.000 1.00 0.000
#&gt; GSM11297     1   0.418      0.806 0.828 0.00 0.172
#&gt; GSM11298     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11314     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11299     1   0.418      0.806 0.828 0.00 0.172
#&gt; GSM28783     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11308     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28782     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM28779     1   0.000      0.965 1.000 0.00 0.000
#&gt; GSM11302     1   0.000      0.965 1.000 0.00 0.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4
#&gt; GSM28788     1  0.3123      0.813 0.844 0.00 0.000 0.156
#&gt; GSM28789     1  0.3123      0.813 0.844 0.00 0.000 0.156
#&gt; GSM28790     1  0.1940      0.876 0.924 0.00 0.000 0.076
#&gt; GSM11300     4  0.3402      0.877 0.004 0.00 0.164 0.832
#&gt; GSM28798     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11318     1  0.1940      0.876 0.924 0.00 0.000 0.076
#&gt; GSM28792     1  0.1940      0.876 0.924 0.00 0.000 0.076
#&gt; GSM11295     3  0.0921      0.980 0.000 0.00 0.972 0.028
#&gt; GSM28793     1  0.2149      0.874 0.912 0.00 0.000 0.088
#&gt; GSM11312     1  0.0707      0.882 0.980 0.00 0.000 0.020
#&gt; GSM28778     1  0.1557      0.870 0.944 0.00 0.000 0.056
#&gt; GSM28796     1  0.2149      0.874 0.912 0.00 0.000 0.088
#&gt; GSM11309     4  0.2081      0.854 0.084 0.00 0.000 0.916
#&gt; GSM11315     1  0.2149      0.874 0.912 0.00 0.000 0.088
#&gt; GSM11306     1  0.2760      0.835 0.872 0.00 0.000 0.128
#&gt; GSM28776     1  0.2760      0.835 0.872 0.00 0.000 0.128
#&gt; GSM28777     3  0.0000      0.992 0.000 0.00 1.000 0.000
#&gt; GSM11316     3  0.0000      0.992 0.000 0.00 1.000 0.000
#&gt; GSM11320     3  0.0000      0.992 0.000 0.00 1.000 0.000
#&gt; GSM28797     4  0.2081      0.854 0.084 0.00 0.000 0.916
#&gt; GSM28786     4  0.2081      0.854 0.084 0.00 0.000 0.916
#&gt; GSM28800     1  0.3444      0.814 0.816 0.00 0.000 0.184
#&gt; GSM11310     1  0.3444      0.814 0.816 0.00 0.000 0.184
#&gt; GSM28787     3  0.0921      0.980 0.000 0.00 0.972 0.028
#&gt; GSM11304     4  0.3402      0.877 0.004 0.00 0.164 0.832
#&gt; GSM11303     3  0.0000      0.992 0.000 0.00 1.000 0.000
#&gt; GSM11317     3  0.0000      0.992 0.000 0.00 1.000 0.000
#&gt; GSM11311     1  0.4522      0.590 0.680 0.00 0.000 0.320
#&gt; GSM28799     1  0.3907      0.761 0.768 0.00 0.000 0.232
#&gt; GSM28791     1  0.1389      0.876 0.952 0.00 0.000 0.048
#&gt; GSM28794     1  0.4907      0.334 0.580 0.42 0.000 0.000
#&gt; GSM28780     1  0.1637      0.876 0.940 0.00 0.000 0.060
#&gt; GSM28795     1  0.1637      0.871 0.940 0.00 0.000 0.060
#&gt; GSM11301     2  0.0000      1.000 0.000 1.00 0.000 0.000
#&gt; GSM11297     4  0.3402      0.877 0.004 0.00 0.164 0.832
#&gt; GSM11298     1  0.2081      0.875 0.916 0.00 0.000 0.084
#&gt; GSM11314     1  0.1716      0.872 0.936 0.00 0.000 0.064
#&gt; GSM11299     4  0.3402      0.877 0.004 0.00 0.164 0.832
#&gt; GSM28783     1  0.1637      0.876 0.940 0.00 0.000 0.060
#&gt; GSM11308     1  0.1637      0.876 0.940 0.00 0.000 0.060
#&gt; GSM28782     1  0.1389      0.879 0.952 0.00 0.000 0.048
#&gt; GSM28779     1  0.0707      0.882 0.980 0.00 0.000 0.020
#&gt; GSM11302     1  0.0707      0.882 0.980 0.00 0.000 0.020
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5
#&gt; GSM28788     5  0.5473      0.650 0.416 0.00 0.000 0.064 0.520
#&gt; GSM28789     5  0.5473      0.650 0.416 0.00 0.000 0.064 0.520
#&gt; GSM28790     1  0.1522      0.628 0.944 0.00 0.000 0.044 0.012
#&gt; GSM11300     4  0.1041      0.871 0.004 0.00 0.032 0.964 0.000
#&gt; GSM28798     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11318     1  0.1522      0.628 0.944 0.00 0.000 0.044 0.012
#&gt; GSM28792     1  0.1522      0.628 0.944 0.00 0.000 0.044 0.012
#&gt; GSM11295     3  0.0404      0.662 0.000 0.00 0.988 0.012 0.000
#&gt; GSM28793     1  0.1197      0.629 0.952 0.00 0.000 0.048 0.000
#&gt; GSM11312     1  0.4251     -0.170 0.672 0.00 0.000 0.012 0.316
#&gt; GSM28778     5  0.4256      0.737 0.436 0.00 0.000 0.000 0.564
#&gt; GSM28796     1  0.1197      0.629 0.952 0.00 0.000 0.048 0.000
#&gt; GSM11309     4  0.3454      0.841 0.064 0.00 0.000 0.836 0.100
#&gt; GSM11315     1  0.1197      0.629 0.952 0.00 0.000 0.048 0.000
#&gt; GSM11306     1  0.5286     -0.587 0.504 0.00 0.000 0.048 0.448
#&gt; GSM28776     1  0.5286     -0.587 0.504 0.00 0.000 0.048 0.448
#&gt; GSM28777     3  0.5825      0.872 0.000 0.00 0.564 0.116 0.320
#&gt; GSM11316     3  0.5825      0.872 0.000 0.00 0.564 0.116 0.320
#&gt; GSM11320     3  0.5825      0.872 0.000 0.00 0.564 0.116 0.320
#&gt; GSM28797     4  0.3454      0.841 0.064 0.00 0.000 0.836 0.100
#&gt; GSM28786     4  0.3454      0.841 0.064 0.00 0.000 0.836 0.100
#&gt; GSM28800     1  0.2674      0.577 0.856 0.00 0.000 0.140 0.004
#&gt; GSM11310     1  0.2674      0.577 0.856 0.00 0.000 0.140 0.004
#&gt; GSM28787     3  0.0404      0.662 0.000 0.00 0.988 0.012 0.000
#&gt; GSM11304     4  0.1041      0.871 0.004 0.00 0.032 0.964 0.000
#&gt; GSM11303     3  0.5825      0.872 0.000 0.00 0.564 0.116 0.320
#&gt; GSM11317     3  0.5825      0.872 0.000 0.00 0.564 0.116 0.320
#&gt; GSM11311     1  0.4644      0.346 0.680 0.00 0.000 0.280 0.040
#&gt; GSM28799     1  0.3621      0.513 0.788 0.00 0.000 0.192 0.020
#&gt; GSM28791     1  0.3857      0.105 0.688 0.00 0.000 0.000 0.312
#&gt; GSM28794     2  0.6671     -0.385 0.340 0.42 0.000 0.000 0.240
#&gt; GSM28780     1  0.3177      0.423 0.792 0.00 0.000 0.000 0.208
#&gt; GSM28795     5  0.4242      0.731 0.428 0.00 0.000 0.000 0.572
#&gt; GSM11301     2  0.0000      0.933 0.000 1.00 0.000 0.000 0.000
#&gt; GSM11297     4  0.1041      0.871 0.004 0.00 0.032 0.964 0.000
#&gt; GSM11298     1  0.1597      0.626 0.940 0.00 0.000 0.048 0.012
#&gt; GSM11314     5  0.4403      0.740 0.436 0.00 0.000 0.004 0.560
#&gt; GSM11299     4  0.1041      0.871 0.004 0.00 0.032 0.964 0.000
#&gt; GSM28783     1  0.3143      0.423 0.796 0.00 0.000 0.000 0.204
#&gt; GSM11308     1  0.3177      0.423 0.792 0.00 0.000 0.000 0.208
#&gt; GSM28782     1  0.2813      0.477 0.832 0.00 0.000 0.000 0.168
#&gt; GSM28779     1  0.4313     -0.343 0.636 0.00 0.000 0.008 0.356
#&gt; GSM11302     1  0.3783      0.115 0.740 0.00 0.000 0.008 0.252
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1   p2    p3    p4    p5    p6
#&gt; GSM28788     5  0.6301      0.236 0.304 0.00 0.000 0.096 0.520 0.080
#&gt; GSM28789     5  0.6301      0.236 0.304 0.00 0.000 0.096 0.520 0.080
#&gt; GSM28790     1  0.0508      0.731 0.984 0.00 0.000 0.000 0.004 0.012
#&gt; GSM11300     4  0.1967      0.869 0.000 0.00 0.084 0.904 0.000 0.012
#&gt; GSM28798     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0508      0.731 0.984 0.00 0.000 0.000 0.004 0.012
#&gt; GSM28792     1  0.0508      0.731 0.984 0.00 0.000 0.000 0.004 0.012
#&gt; GSM11295     6  0.3409      1.000 0.000 0.00 0.300 0.000 0.000 0.700
#&gt; GSM28793     1  0.0000      0.735 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11312     1  0.4141      0.209 0.596 0.00 0.000 0.000 0.388 0.016
#&gt; GSM28778     5  0.3364      0.397 0.024 0.00 0.000 0.000 0.780 0.196
#&gt; GSM28796     1  0.0000      0.735 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.2816      0.827 0.028 0.00 0.000 0.876 0.060 0.036
#&gt; GSM11315     1  0.0000      0.735 1.000 0.00 0.000 0.000 0.000 0.000
#&gt; GSM11306     5  0.5989      0.128 0.404 0.00 0.000 0.068 0.468 0.060
#&gt; GSM28776     5  0.5989      0.128 0.404 0.00 0.000 0.068 0.468 0.060
#&gt; GSM28777     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.2816      0.827 0.028 0.00 0.000 0.876 0.060 0.036
#&gt; GSM28786     4  0.2816      0.827 0.028 0.00 0.000 0.876 0.060 0.036
#&gt; GSM28800     1  0.2162      0.692 0.896 0.00 0.000 0.088 0.012 0.004
#&gt; GSM11310     1  0.2162      0.692 0.896 0.00 0.000 0.088 0.012 0.004
#&gt; GSM28787     6  0.3409      1.000 0.000 0.00 0.300 0.000 0.000 0.700
#&gt; GSM11304     4  0.1967      0.869 0.000 0.00 0.084 0.904 0.000 0.012
#&gt; GSM11303     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000      1.000 0.000 0.00 1.000 0.000 0.000 0.000
#&gt; GSM11311     1  0.4433      0.384 0.668 0.00 0.000 0.288 0.016 0.028
#&gt; GSM28799     1  0.3147      0.632 0.828 0.00 0.000 0.140 0.016 0.016
#&gt; GSM28791     5  0.4167      0.334 0.344 0.00 0.000 0.000 0.632 0.024
#&gt; GSM28794     2  0.6243     -0.152 0.276 0.42 0.000 0.000 0.296 0.008
#&gt; GSM28780     5  0.4325      0.233 0.456 0.00 0.000 0.000 0.524 0.020
#&gt; GSM28795     5  0.3424      0.394 0.024 0.00 0.000 0.000 0.772 0.204
#&gt; GSM11301     2  0.0000      0.934 0.000 1.00 0.000 0.000 0.000 0.000
#&gt; GSM11297     4  0.1967      0.869 0.000 0.00 0.084 0.904 0.000 0.012
#&gt; GSM11298     1  0.0458      0.731 0.984 0.00 0.000 0.000 0.016 0.000
#&gt; GSM11314     5  0.3333      0.404 0.024 0.00 0.000 0.000 0.784 0.192
#&gt; GSM11299     4  0.1967      0.869 0.000 0.00 0.084 0.904 0.000 0.012
#&gt; GSM28783     5  0.4335      0.209 0.472 0.00 0.000 0.000 0.508 0.020
#&gt; GSM11308     5  0.4325      0.233 0.456 0.00 0.000 0.000 0.524 0.020
#&gt; GSM28782     1  0.4333     -0.236 0.512 0.00 0.000 0.000 0.468 0.020
#&gt; GSM28779     1  0.4269      0.146 0.568 0.00 0.000 0.000 0.412 0.020
#&gt; GSM11302     1  0.3619      0.361 0.680 0.00 0.000 0.000 0.316 0.004
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:hclust 51     0.395 2
#> MAD:hclust 51     0.371 3
#> MAD:hclust 51     0.453 4
#> MAD:hclust 40     0.414 5
#> MAD:hclust 35     0.394 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.369           0.788       0.812         0.3639 0.638   0.638
#> 3 3 1.000           0.971       0.975         0.5249 0.790   0.670
#> 4 4 0.696           0.561       0.802         0.2567 0.902   0.771
#> 5 5 0.669           0.747       0.808         0.1037 0.830   0.514
#> 6 6 0.746           0.669       0.812         0.0557 0.992   0.958
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.980      0.783 0.584 0.416
#&gt; GSM28789     1   0.980      0.783 0.584 0.416
#&gt; GSM28790     1   0.980      0.783 0.584 0.416
#&gt; GSM11300     1   0.000      0.584 1.000 0.000
#&gt; GSM28798     2   0.000      0.997 0.000 1.000
#&gt; GSM11296     2   0.000      0.997 0.000 1.000
#&gt; GSM28801     2   0.000      0.997 0.000 1.000
#&gt; GSM11319     2   0.000      0.997 0.000 1.000
#&gt; GSM28781     2   0.000      0.997 0.000 1.000
#&gt; GSM11305     2   0.000      0.997 0.000 1.000
#&gt; GSM28784     2   0.000      0.997 0.000 1.000
#&gt; GSM11307     2   0.000      0.997 0.000 1.000
#&gt; GSM11313     2   0.000      0.997 0.000 1.000
#&gt; GSM28785     2   0.000      0.997 0.000 1.000
#&gt; GSM11318     1   0.980      0.783 0.584 0.416
#&gt; GSM28792     1   0.980      0.783 0.584 0.416
#&gt; GSM11295     1   0.141      0.572 0.980 0.020
#&gt; GSM28793     1   0.980      0.783 0.584 0.416
#&gt; GSM11312     1   0.980      0.783 0.584 0.416
#&gt; GSM28778     1   0.980      0.783 0.584 0.416
#&gt; GSM28796     1   0.980      0.783 0.584 0.416
#&gt; GSM11309     1   0.802      0.738 0.756 0.244
#&gt; GSM11315     1   0.980      0.783 0.584 0.416
#&gt; GSM11306     1   0.980      0.783 0.584 0.416
#&gt; GSM28776     1   0.980      0.783 0.584 0.416
#&gt; GSM28777     1   0.141      0.572 0.980 0.020
#&gt; GSM11316     1   0.141      0.572 0.980 0.020
#&gt; GSM11320     1   0.141      0.572 0.980 0.020
#&gt; GSM28797     1   0.802      0.738 0.756 0.244
#&gt; GSM28786     1   0.671      0.699 0.824 0.176
#&gt; GSM28800     1   0.978      0.784 0.588 0.412
#&gt; GSM11310     1   0.975      0.783 0.592 0.408
#&gt; GSM28787     1   0.141      0.572 0.980 0.020
#&gt; GSM11304     1   0.753      0.724 0.784 0.216
#&gt; GSM11303     1   0.141      0.572 0.980 0.020
#&gt; GSM11317     1   0.141      0.572 0.980 0.020
#&gt; GSM11311     1   0.866      0.753 0.712 0.288
#&gt; GSM28799     1   0.871      0.754 0.708 0.292
#&gt; GSM28791     1   0.980      0.783 0.584 0.416
#&gt; GSM28794     2   0.141      0.966 0.020 0.980
#&gt; GSM28780     1   0.978      0.784 0.588 0.412
#&gt; GSM28795     1   0.978      0.784 0.588 0.412
#&gt; GSM11301     2   0.000      0.997 0.000 1.000
#&gt; GSM11297     1   0.753      0.724 0.784 0.216
#&gt; GSM11298     1   0.980      0.783 0.584 0.416
#&gt; GSM11314     1   0.978      0.784 0.588 0.412
#&gt; GSM11299     1   0.000      0.584 1.000 0.000
#&gt; GSM28783     1   0.978      0.784 0.588 0.412
#&gt; GSM11308     1   0.827      0.744 0.740 0.260
#&gt; GSM28782     1   0.978      0.784 0.588 0.412
#&gt; GSM28779     1   0.980      0.783 0.584 0.416
#&gt; GSM11302     1   0.980      0.783 0.584 0.416
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM28789     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM28790     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11300     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM28798     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM11296     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM28801     2  0.1711      0.943 0.032 0.960 0.008
#&gt; GSM11319     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM28781     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM11305     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM28784     2  0.2176      0.937 0.032 0.948 0.020
#&gt; GSM11307     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM11313     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM28785     2  0.1289      0.946 0.032 0.968 0.000
#&gt; GSM11318     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11295     3  0.2569      0.981 0.032 0.032 0.936
#&gt; GSM28793     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11309     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM11315     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11306     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM28776     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28777     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM11316     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM11320     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM28797     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM28786     1  0.1289      0.974 0.968 0.000 0.032
#&gt; GSM28800     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28787     3  0.2569      0.981 0.032 0.032 0.936
#&gt; GSM11304     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11303     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM11317     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM11311     1  0.0592      0.989 0.988 0.000 0.012
#&gt; GSM28799     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28794     2  0.6962      0.340 0.412 0.568 0.020
#&gt; GSM28780     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11301     2  0.2176      0.937 0.032 0.948 0.020
#&gt; GSM11297     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11299     3  0.1289      0.995 0.032 0.000 0.968
#&gt; GSM28783     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.997 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.4925    -0.2172 0.572 0.000 0.000 0.428
#&gt; GSM28789     1  0.4916    -0.2114 0.576 0.000 0.000 0.424
#&gt; GSM28790     1  0.4522     0.4723 0.680 0.000 0.000 0.320
#&gt; GSM11300     3  0.1940     0.9468 0.000 0.000 0.924 0.076
#&gt; GSM28798     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0336     0.9372 0.000 0.992 0.000 0.008
#&gt; GSM11319     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.1557     0.9137 0.000 0.944 0.000 0.056
#&gt; GSM11307     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9402 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.4679     0.4556 0.648 0.000 0.000 0.352
#&gt; GSM28792     1  0.4790     0.4417 0.620 0.000 0.000 0.380
#&gt; GSM11295     3  0.2011     0.9526 0.000 0.000 0.920 0.080
#&gt; GSM28793     1  0.4804     0.4389 0.616 0.000 0.000 0.384
#&gt; GSM11312     1  0.2281     0.4253 0.904 0.000 0.000 0.096
#&gt; GSM28778     1  0.1867     0.4050 0.928 0.000 0.000 0.072
#&gt; GSM28796     1  0.4804     0.4389 0.616 0.000 0.000 0.384
#&gt; GSM11309     4  0.4605     0.6352 0.336 0.000 0.000 0.664
#&gt; GSM11315     1  0.4804     0.4389 0.616 0.000 0.000 0.384
#&gt; GSM11306     1  0.4776    -0.0673 0.624 0.000 0.000 0.376
#&gt; GSM28776     1  0.4543     0.3532 0.676 0.000 0.000 0.324
#&gt; GSM28777     3  0.0592     0.9698 0.000 0.000 0.984 0.016
#&gt; GSM11316     3  0.0000     0.9717 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9717 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.4605     0.6352 0.336 0.000 0.000 0.664
#&gt; GSM28786     4  0.4677     0.6204 0.316 0.000 0.004 0.680
#&gt; GSM28800     1  0.4730     0.4337 0.636 0.000 0.000 0.364
#&gt; GSM11310     4  0.5000    -0.2802 0.500 0.000 0.000 0.500
#&gt; GSM28787     3  0.2011     0.9526 0.000 0.000 0.920 0.080
#&gt; GSM11304     1  0.4999    -0.0516 0.508 0.000 0.000 0.492
#&gt; GSM11303     3  0.0000     0.9717 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9717 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.4164     0.4018 0.264 0.000 0.000 0.736
#&gt; GSM28799     1  0.4955     0.0153 0.556 0.000 0.000 0.444
#&gt; GSM28791     1  0.0188     0.4530 0.996 0.000 0.000 0.004
#&gt; GSM28794     2  0.6988     0.2223 0.380 0.500 0.000 0.120
#&gt; GSM28780     1  0.1302     0.4305 0.956 0.000 0.000 0.044
#&gt; GSM28795     1  0.1940     0.4030 0.924 0.000 0.000 0.076
#&gt; GSM11301     2  0.2345     0.8871 0.000 0.900 0.000 0.100
#&gt; GSM11297     1  0.5000    -0.0627 0.504 0.000 0.000 0.496
#&gt; GSM11298     1  0.4804     0.4389 0.616 0.000 0.000 0.384
#&gt; GSM11314     1  0.2647     0.3532 0.880 0.000 0.000 0.120
#&gt; GSM11299     3  0.1940     0.9468 0.000 0.000 0.924 0.076
#&gt; GSM28783     1  0.1792     0.4154 0.932 0.000 0.000 0.068
#&gt; GSM11308     1  0.1867     0.4422 0.928 0.000 0.000 0.072
#&gt; GSM28782     1  0.3356     0.4721 0.824 0.000 0.000 0.176
#&gt; GSM28779     1  0.4679     0.4640 0.648 0.000 0.000 0.352
#&gt; GSM11302     1  0.4679     0.4640 0.648 0.000 0.000 0.352
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.5928      0.547 0.108 0.000 0.000 0.500 0.392
#&gt; GSM28789     4  0.5928      0.547 0.108 0.000 0.000 0.500 0.392
#&gt; GSM28790     1  0.3561      0.759 0.740 0.000 0.000 0.000 0.260
#&gt; GSM11300     3  0.3085      0.851 0.032 0.000 0.852 0.116 0.000
#&gt; GSM28798     2  0.0290      0.930 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11296     2  0.0000      0.931 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0404      0.928 0.000 0.988 0.000 0.012 0.000
#&gt; GSM11319     2  0.0000      0.931 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0290      0.929 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11305     2  0.0290      0.930 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28784     2  0.2569      0.877 0.040 0.892 0.000 0.068 0.000
#&gt; GSM11307     2  0.0290      0.930 0.000 0.992 0.000 0.008 0.000
#&gt; GSM11313     2  0.0290      0.930 0.000 0.992 0.000 0.008 0.000
#&gt; GSM28785     2  0.0000      0.931 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.3491      0.779 0.768 0.000 0.000 0.004 0.228
#&gt; GSM28792     1  0.3461      0.781 0.772 0.000 0.000 0.004 0.224
#&gt; GSM11295     3  0.4089      0.850 0.100 0.000 0.804 0.088 0.008
#&gt; GSM28793     1  0.3461      0.781 0.772 0.000 0.000 0.004 0.224
#&gt; GSM11312     5  0.4648      0.602 0.156 0.000 0.000 0.104 0.740
#&gt; GSM28778     5  0.0703      0.848 0.024 0.000 0.000 0.000 0.976
#&gt; GSM28796     1  0.3461      0.781 0.772 0.000 0.000 0.004 0.224
#&gt; GSM11309     4  0.4262      0.708 0.100 0.000 0.000 0.776 0.124
#&gt; GSM11315     1  0.3461      0.781 0.772 0.000 0.000 0.004 0.224
#&gt; GSM11306     4  0.5844      0.494 0.096 0.000 0.000 0.484 0.420
#&gt; GSM28776     1  0.6492      0.470 0.456 0.000 0.000 0.196 0.348
#&gt; GSM28777     3  0.0290      0.924 0.008 0.000 0.992 0.000 0.000
#&gt; GSM11316     3  0.0451      0.923 0.008 0.000 0.988 0.004 0.000
#&gt; GSM11320     3  0.0000      0.925 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.4262      0.708 0.100 0.000 0.000 0.776 0.124
#&gt; GSM28786     4  0.4325      0.704 0.100 0.000 0.004 0.780 0.116
#&gt; GSM28800     1  0.4794      0.682 0.624 0.000 0.000 0.032 0.344
#&gt; GSM11310     1  0.5983      0.644 0.580 0.000 0.000 0.168 0.252
#&gt; GSM28787     3  0.4089      0.850 0.100 0.000 0.804 0.088 0.008
#&gt; GSM11304     1  0.6961      0.306 0.424 0.000 0.012 0.228 0.336
#&gt; GSM11303     3  0.0000      0.925 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.925 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.5193      0.369 0.364 0.000 0.000 0.584 0.052
#&gt; GSM28799     1  0.6621      0.438 0.448 0.000 0.000 0.240 0.312
#&gt; GSM28791     5  0.1544      0.828 0.068 0.000 0.000 0.000 0.932
#&gt; GSM28794     2  0.7646      0.271 0.160 0.484 0.000 0.108 0.248
#&gt; GSM28780     5  0.0865      0.848 0.024 0.000 0.000 0.004 0.972
#&gt; GSM28795     5  0.0671      0.845 0.016 0.000 0.000 0.004 0.980
#&gt; GSM11301     2  0.3506      0.838 0.064 0.832 0.000 0.104 0.000
#&gt; GSM11297     1  0.6961      0.306 0.424 0.000 0.012 0.228 0.336
#&gt; GSM11298     1  0.3305      0.780 0.776 0.000 0.000 0.000 0.224
#&gt; GSM11314     5  0.1331      0.805 0.008 0.000 0.000 0.040 0.952
#&gt; GSM11299     3  0.3085      0.851 0.032 0.000 0.852 0.116 0.000
#&gt; GSM28783     5  0.0771      0.844 0.020 0.000 0.000 0.004 0.976
#&gt; GSM11308     5  0.1965      0.796 0.052 0.000 0.000 0.024 0.924
#&gt; GSM28782     5  0.3949      0.197 0.332 0.000 0.000 0.000 0.668
#&gt; GSM28779     1  0.3790      0.754 0.724 0.000 0.000 0.004 0.272
#&gt; GSM11302     1  0.3814      0.753 0.720 0.000 0.000 0.004 0.276
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.6630      0.402 0.064 0.000 0.000 0.472 0.300 0.164
#&gt; GSM28789     4  0.6630      0.402 0.064 0.000 0.000 0.472 0.300 0.164
#&gt; GSM28790     1  0.0632      0.704 0.976 0.000 0.000 0.000 0.024 0.000
#&gt; GSM11300     3  0.4623      0.749 0.000 0.000 0.736 0.104 0.028 0.132
#&gt; GSM28798     2  0.0551      0.931 0.000 0.984 0.000 0.004 0.004 0.008
#&gt; GSM11296     2  0.0000      0.933 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0405      0.928 0.000 0.988 0.000 0.008 0.000 0.004
#&gt; GSM11319     2  0.0000      0.933 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0260      0.930 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; GSM11305     2  0.0551      0.931 0.000 0.984 0.000 0.004 0.004 0.008
#&gt; GSM28784     2  0.2219      0.782 0.000 0.864 0.000 0.000 0.000 0.136
#&gt; GSM11307     2  0.0551      0.931 0.000 0.984 0.000 0.004 0.004 0.008
#&gt; GSM11313     2  0.0551      0.931 0.000 0.984 0.000 0.004 0.004 0.008
#&gt; GSM28785     2  0.0000      0.933 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0508      0.708 0.984 0.000 0.000 0.004 0.012 0.000
#&gt; GSM28792     1  0.0405      0.709 0.988 0.000 0.000 0.004 0.008 0.000
#&gt; GSM11295     3  0.3837      0.789 0.000 0.000 0.752 0.016 0.020 0.212
#&gt; GSM28793     1  0.0520      0.710 0.984 0.000 0.000 0.008 0.008 0.000
#&gt; GSM11312     5  0.6242      0.418 0.208 0.000 0.000 0.072 0.572 0.148
#&gt; GSM28778     5  0.2900      0.767 0.088 0.000 0.000 0.008 0.860 0.044
#&gt; GSM28796     1  0.0520      0.710 0.984 0.000 0.000 0.008 0.008 0.000
#&gt; GSM11309     4  0.1320      0.603 0.036 0.000 0.000 0.948 0.016 0.000
#&gt; GSM11315     1  0.0520      0.710 0.984 0.000 0.000 0.008 0.008 0.000
#&gt; GSM11306     4  0.6778      0.299 0.084 0.000 0.000 0.440 0.332 0.144
#&gt; GSM28776     1  0.7026      0.284 0.484 0.000 0.000 0.180 0.176 0.160
#&gt; GSM28777     3  0.1478      0.871 0.000 0.000 0.944 0.004 0.032 0.020
#&gt; GSM11316     3  0.0146      0.877 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11320     3  0.0000      0.878 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.1320      0.603 0.036 0.000 0.000 0.948 0.016 0.000
#&gt; GSM28786     4  0.1010      0.596 0.036 0.000 0.000 0.960 0.004 0.000
#&gt; GSM28800     1  0.5145      0.568 0.660 0.000 0.000 0.024 0.096 0.220
#&gt; GSM11310     1  0.6115      0.518 0.588 0.000 0.000 0.160 0.064 0.188
#&gt; GSM28787     3  0.3837      0.789 0.000 0.000 0.752 0.016 0.020 0.212
#&gt; GSM11304     1  0.7830      0.192 0.324 0.000 0.020 0.212 0.132 0.312
#&gt; GSM11303     3  0.0000      0.878 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.878 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.3151      0.430 0.252 0.000 0.000 0.748 0.000 0.000
#&gt; GSM28799     1  0.6873      0.406 0.484 0.000 0.000 0.204 0.096 0.216
#&gt; GSM28791     5  0.1910      0.777 0.108 0.000 0.000 0.000 0.892 0.000
#&gt; GSM28794     6  0.7459      0.000 0.140 0.304 0.000 0.016 0.132 0.408
#&gt; GSM28780     5  0.2860      0.770 0.100 0.000 0.000 0.000 0.852 0.048
#&gt; GSM28795     5  0.2711      0.769 0.084 0.000 0.000 0.008 0.872 0.036
#&gt; GSM11301     2  0.3448      0.503 0.000 0.716 0.000 0.004 0.000 0.280
#&gt; GSM11297     1  0.7830      0.192 0.324 0.000 0.020 0.212 0.132 0.312
#&gt; GSM11298     1  0.0291      0.707 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; GSM11314     5  0.3300      0.747 0.076 0.000 0.000 0.016 0.840 0.068
#&gt; GSM11299     3  0.4873      0.724 0.000 0.000 0.708 0.104 0.028 0.160
#&gt; GSM28783     5  0.3424      0.751 0.096 0.000 0.000 0.000 0.812 0.092
#&gt; GSM11308     5  0.3891      0.669 0.072 0.000 0.000 0.008 0.780 0.140
#&gt; GSM28782     5  0.5623      0.263 0.372 0.000 0.000 0.000 0.476 0.152
#&gt; GSM28779     1  0.3207      0.660 0.844 0.000 0.000 0.016 0.048 0.092
#&gt; GSM11302     1  0.2945      0.664 0.864 0.000 0.000 0.016 0.048 0.072
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:kmeans 52     0.396 2
#> MAD:kmeans 51     0.371 3
#> MAD:kmeans 23     0.389 4
#> MAD:kmeans 44     0.451 5
#> MAD:kmeans 41     0.501 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.720           0.890       0.950         0.4335 0.581   0.581
#> 3 3 0.969           0.912       0.966         0.5080 0.703   0.512
#> 4 4 0.783           0.785       0.901         0.1587 0.845   0.576
#> 5 5 0.922           0.860       0.938         0.0653 0.916   0.674
#> 6 6 0.859           0.767       0.866         0.0358 0.953   0.765
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 3
```

There is also optional best $k$ = 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1   0.000      0.941 1.000 0.000
#&gt; GSM28789     2   0.827      0.662 0.260 0.740
#&gt; GSM28790     1   0.000      0.941 1.000 0.000
#&gt; GSM11300     1   0.000      0.941 1.000 0.000
#&gt; GSM28798     2   0.000      0.942 0.000 1.000
#&gt; GSM11296     2   0.000      0.942 0.000 1.000
#&gt; GSM28801     2   0.000      0.942 0.000 1.000
#&gt; GSM11319     2   0.000      0.942 0.000 1.000
#&gt; GSM28781     2   0.000      0.942 0.000 1.000
#&gt; GSM11305     2   0.000      0.942 0.000 1.000
#&gt; GSM28784     2   0.000      0.942 0.000 1.000
#&gt; GSM11307     2   0.000      0.942 0.000 1.000
#&gt; GSM11313     2   0.000      0.942 0.000 1.000
#&gt; GSM28785     2   0.000      0.942 0.000 1.000
#&gt; GSM11318     1   0.000      0.941 1.000 0.000
#&gt; GSM28792     1   0.000      0.941 1.000 0.000
#&gt; GSM11295     1   0.827      0.687 0.740 0.260
#&gt; GSM28793     1   0.000      0.941 1.000 0.000
#&gt; GSM11312     1   0.000      0.941 1.000 0.000
#&gt; GSM28778     2   0.827      0.662 0.260 0.740
#&gt; GSM28796     1   0.000      0.941 1.000 0.000
#&gt; GSM11309     1   0.000      0.941 1.000 0.000
#&gt; GSM11315     1   0.000      0.941 1.000 0.000
#&gt; GSM11306     1   0.000      0.941 1.000 0.000
#&gt; GSM28776     1   0.000      0.941 1.000 0.000
#&gt; GSM28777     1   0.827      0.687 0.740 0.260
#&gt; GSM11316     1   0.876      0.634 0.704 0.296
#&gt; GSM11320     1   0.827      0.687 0.740 0.260
#&gt; GSM28797     1   0.000      0.941 1.000 0.000
#&gt; GSM28786     1   0.000      0.941 1.000 0.000
#&gt; GSM28800     1   0.000      0.941 1.000 0.000
#&gt; GSM11310     1   0.000      0.941 1.000 0.000
#&gt; GSM28787     1   0.886      0.621 0.696 0.304
#&gt; GSM11304     1   0.000      0.941 1.000 0.000
#&gt; GSM11303     1   0.827      0.687 0.740 0.260
#&gt; GSM11317     1   0.827      0.687 0.740 0.260
#&gt; GSM11311     1   0.000      0.941 1.000 0.000
#&gt; GSM28799     1   0.000      0.941 1.000 0.000
#&gt; GSM28791     1   0.000      0.941 1.000 0.000
#&gt; GSM28794     2   0.000      0.942 0.000 1.000
#&gt; GSM28780     1   0.000      0.941 1.000 0.000
#&gt; GSM28795     1   0.000      0.941 1.000 0.000
#&gt; GSM11301     2   0.000      0.942 0.000 1.000
#&gt; GSM11297     1   0.000      0.941 1.000 0.000
#&gt; GSM11298     1   0.000      0.941 1.000 0.000
#&gt; GSM11314     2   0.722      0.713 0.200 0.800
#&gt; GSM11299     1   0.000      0.941 1.000 0.000
#&gt; GSM28783     1   0.000      0.941 1.000 0.000
#&gt; GSM11308     1   0.000      0.941 1.000 0.000
#&gt; GSM28782     1   0.000      0.941 1.000 0.000
#&gt; GSM28779     1   0.000      0.941 1.000 0.000
#&gt; GSM11302     1   0.000      0.941 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.2356     0.9145 0.928 0.000 0.072
#&gt; GSM28789     2  0.7203     0.2429 0.416 0.556 0.028
#&gt; GSM28790     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28778     1  0.0424     0.9797 0.992 0.008 0.000
#&gt; GSM28796     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11309     3  0.0424     0.9194 0.008 0.000 0.992
#&gt; GSM11315     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM28797     3  0.0424     0.9194 0.008 0.000 0.992
#&gt; GSM28786     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM28800     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11304     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11303     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28799     1  0.4504     0.7474 0.804 0.000 0.196
#&gt; GSM28791     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28794     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM28780     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000     0.9573 0.000 1.000 0.000
#&gt; GSM11297     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM11298     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11314     3  0.9955     0.0901 0.348 0.288 0.364
#&gt; GSM11299     3  0.0000     0.9253 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11308     3  0.6180     0.3086 0.416 0.000 0.584
#&gt; GSM28782     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000     0.9867 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.9867 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.1209      0.690 0.032 0.000 0.004 0.964
#&gt; GSM28789     4  0.0712      0.693 0.004 0.008 0.004 0.984
#&gt; GSM28790     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM11300     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM28792     1  0.0188      0.873 0.996 0.000 0.000 0.004
#&gt; GSM11295     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11312     4  0.4697      0.527 0.356 0.000 0.000 0.644
#&gt; GSM28778     4  0.4250      0.649 0.276 0.000 0.000 0.724
#&gt; GSM28796     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.4456      0.411 0.004 0.000 0.280 0.716
#&gt; GSM11315     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.0707      0.695 0.020 0.000 0.000 0.980
#&gt; GSM28776     1  0.4585      0.527 0.668 0.000 0.000 0.332
#&gt; GSM28777     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.4456      0.411 0.004 0.000 0.280 0.716
#&gt; GSM28786     3  0.5097      0.291 0.004 0.000 0.568 0.428
#&gt; GSM28800     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.3801      0.676 0.780 0.000 0.000 0.220
#&gt; GSM28787     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM11304     3  0.0188      0.905 0.000 0.000 0.996 0.004
#&gt; GSM11303     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.4277      0.594 0.720 0.000 0.000 0.280
#&gt; GSM28799     1  0.5736      0.466 0.628 0.000 0.044 0.328
#&gt; GSM28791     4  0.4961      0.417 0.448 0.000 0.000 0.552
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM28780     4  0.4804      0.538 0.384 0.000 0.000 0.616
#&gt; GSM28795     4  0.4164      0.657 0.264 0.000 0.000 0.736
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000
#&gt; GSM11297     3  0.0336      0.902 0.000 0.000 0.992 0.008
#&gt; GSM11298     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11314     4  0.3937      0.682 0.024 0.024 0.100 0.852
#&gt; GSM11299     3  0.0000      0.908 0.000 0.000 1.000 0.000
#&gt; GSM28783     4  0.4304      0.644 0.284 0.000 0.000 0.716
#&gt; GSM11308     3  0.7603     -0.168 0.204 0.000 0.436 0.360
#&gt; GSM28782     1  0.2814      0.722 0.868 0.000 0.000 0.132
#&gt; GSM28779     1  0.0000      0.875 1.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.875 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.1270     0.8678 0.000 0.000 0.000 0.948 0.052
#&gt; GSM28789     4  0.1270     0.8678 0.000 0.000 0.000 0.948 0.052
#&gt; GSM28790     1  0.0290     0.8834 0.992 0.000 0.000 0.000 0.008
#&gt; GSM11300     3  0.0324     0.9713 0.000 0.000 0.992 0.004 0.004
#&gt; GSM28798     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0290     0.8836 0.992 0.000 0.000 0.000 0.008
#&gt; GSM28792     1  0.0162     0.8850 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11295     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     1  0.0000     0.8859 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     5  0.5577     0.5515 0.184 0.000 0.000 0.172 0.644
#&gt; GSM28778     5  0.1211     0.8618 0.016 0.000 0.000 0.024 0.960
#&gt; GSM28796     1  0.0000     0.8859 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0451     0.8756 0.000 0.000 0.008 0.988 0.004
#&gt; GSM11315     1  0.0000     0.8859 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.2563     0.7941 0.008 0.000 0.000 0.872 0.120
#&gt; GSM28776     1  0.5195     0.3098 0.564 0.000 0.000 0.388 0.048
#&gt; GSM28777     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.0451     0.8756 0.000 0.000 0.008 0.988 0.004
#&gt; GSM28786     4  0.0404     0.8737 0.000 0.000 0.012 0.988 0.000
#&gt; GSM28800     1  0.1597     0.8563 0.940 0.000 0.000 0.012 0.048
#&gt; GSM11310     1  0.3961     0.6850 0.760 0.000 0.000 0.212 0.028
#&gt; GSM28787     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11304     3  0.2938     0.8883 0.008 0.000 0.876 0.084 0.032
#&gt; GSM11303     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9741 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4  0.4171     0.2621 0.396 0.000 0.000 0.604 0.000
#&gt; GSM28799     1  0.5696     0.0746 0.472 0.000 0.020 0.468 0.040
#&gt; GSM28791     5  0.0963     0.8611 0.036 0.000 0.000 0.000 0.964
#&gt; GSM28794     2  0.0162     0.9961 0.000 0.996 0.000 0.004 0.000
#&gt; GSM28780     5  0.0162     0.8646 0.004 0.000 0.000 0.000 0.996
#&gt; GSM28795     5  0.0671     0.8635 0.004 0.000 0.000 0.016 0.980
#&gt; GSM11301     2  0.0000     0.9996 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     3  0.3218     0.8738 0.012 0.000 0.860 0.096 0.032
#&gt; GSM11298     1  0.0000     0.8859 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11314     5  0.1195     0.8564 0.000 0.000 0.012 0.028 0.960
#&gt; GSM11299     3  0.0451     0.9695 0.000 0.000 0.988 0.008 0.004
#&gt; GSM28783     5  0.0566     0.8645 0.012 0.000 0.000 0.004 0.984
#&gt; GSM11308     5  0.1557     0.8346 0.000 0.000 0.052 0.008 0.940
#&gt; GSM28782     5  0.4359     0.3244 0.412 0.000 0.000 0.004 0.584
#&gt; GSM28779     1  0.0404     0.8833 0.988 0.000 0.000 0.012 0.000
#&gt; GSM11302     1  0.0290     0.8841 0.992 0.000 0.000 0.008 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.4024     0.6807 0.000 0.000 0.000 0.700 0.036 0.264
#&gt; GSM28789     4  0.4151     0.6755 0.000 0.000 0.000 0.692 0.044 0.264
#&gt; GSM28790     1  0.0622     0.9116 0.980 0.000 0.000 0.000 0.008 0.012
#&gt; GSM11300     3  0.2630     0.8576 0.000 0.000 0.872 0.032 0.004 0.092
#&gt; GSM28798     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0291     0.9144 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; GSM28792     1  0.0146     0.9166 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM11295     3  0.0547     0.9489 0.000 0.000 0.980 0.000 0.000 0.020
#&gt; GSM28793     1  0.0000     0.9177 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     5  0.6085     0.2914 0.092 0.000 0.000 0.048 0.448 0.412
#&gt; GSM28778     5  0.2325     0.7620 0.008 0.000 0.000 0.008 0.884 0.100
#&gt; GSM28796     1  0.0000     0.9177 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.0146     0.7328 0.000 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11315     1  0.0000     0.9177 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     4  0.4565     0.5755 0.000 0.000 0.000 0.664 0.076 0.260
#&gt; GSM28776     6  0.6938     0.1482 0.260 0.000 0.000 0.264 0.064 0.412
#&gt; GSM28777     3  0.0260     0.9541 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11316     3  0.0260     0.9541 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11320     3  0.0000     0.9546 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0000     0.7339 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28786     4  0.0405     0.7292 0.000 0.000 0.004 0.988 0.000 0.008
#&gt; GSM28800     6  0.5279     0.2258 0.376 0.000 0.000 0.040 0.036 0.548
#&gt; GSM11310     6  0.5964     0.4067 0.216 0.000 0.000 0.248 0.012 0.524
#&gt; GSM28787     3  0.0632     0.9464 0.000 0.000 0.976 0.000 0.000 0.024
#&gt; GSM11304     6  0.6343     0.0817 0.012 0.000 0.404 0.140 0.020 0.424
#&gt; GSM11303     3  0.0000     0.9546 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9546 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.3861     0.3165 0.352 0.000 0.000 0.640 0.000 0.008
#&gt; GSM28799     6  0.6038     0.3714 0.176 0.000 0.008 0.264 0.012 0.540
#&gt; GSM28791     5  0.1908     0.7766 0.028 0.000 0.000 0.000 0.916 0.056
#&gt; GSM28794     2  0.0458     0.9833 0.000 0.984 0.000 0.000 0.000 0.016
#&gt; GSM28780     5  0.1010     0.7757 0.004 0.000 0.000 0.000 0.960 0.036
#&gt; GSM28795     5  0.1285     0.7749 0.004 0.000 0.000 0.000 0.944 0.052
#&gt; GSM11301     2  0.0000     0.9985 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.6477     0.1462 0.016 0.000 0.376 0.152 0.020 0.436
#&gt; GSM11298     1  0.0790     0.9033 0.968 0.000 0.000 0.000 0.000 0.032
#&gt; GSM11314     5  0.3407     0.7264 0.000 0.000 0.016 0.016 0.800 0.168
#&gt; GSM11299     3  0.2747     0.8456 0.000 0.000 0.860 0.028 0.004 0.108
#&gt; GSM28783     5  0.3056     0.7319 0.004 0.000 0.000 0.008 0.804 0.184
#&gt; GSM11308     5  0.2573     0.7354 0.000 0.000 0.004 0.008 0.856 0.132
#&gt; GSM28782     5  0.5655     0.3823 0.164 0.000 0.000 0.004 0.536 0.296
#&gt; GSM28779     1  0.3518     0.6309 0.732 0.000 0.000 0.000 0.012 0.256
#&gt; GSM11302     1  0.3014     0.7419 0.804 0.000 0.000 0.000 0.012 0.184
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> MAD:skmeans 52     0.396 2
#> MAD:skmeans 49     0.446 3
#> MAD:skmeans 46     0.412 4
#> MAD:skmeans 48     0.449 5
#> MAD:skmeans 43     0.441 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.995       0.998         0.3850 0.618   0.618
#> 3 3 1.000           0.965       0.986         0.4838 0.798   0.676
#> 4 4 0.744           0.803       0.892         0.2448 0.839   0.633
#> 5 5 0.725           0.627       0.817         0.0846 0.913   0.714
#> 6 6 0.787           0.789       0.828         0.0606 0.854   0.497
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.997 1.000 0.000
#&gt; GSM28789     1  0.0000      0.997 1.000 0.000
#&gt; GSM28790     1  0.0000      0.997 1.000 0.000
#&gt; GSM11300     1  0.0000      0.997 1.000 0.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000
#&gt; GSM11318     1  0.0000      0.997 1.000 0.000
#&gt; GSM28792     1  0.0000      0.997 1.000 0.000
#&gt; GSM11295     1  0.0000      0.997 1.000 0.000
#&gt; GSM28793     1  0.0000      0.997 1.000 0.000
#&gt; GSM11312     1  0.0000      0.997 1.000 0.000
#&gt; GSM28778     1  0.0000      0.997 1.000 0.000
#&gt; GSM28796     1  0.0000      0.997 1.000 0.000
#&gt; GSM11309     1  0.0000      0.997 1.000 0.000
#&gt; GSM11315     1  0.0000      0.997 1.000 0.000
#&gt; GSM11306     1  0.0000      0.997 1.000 0.000
#&gt; GSM28776     1  0.0000      0.997 1.000 0.000
#&gt; GSM28777     1  0.0000      0.997 1.000 0.000
#&gt; GSM11316     2  0.0000      1.000 0.000 1.000
#&gt; GSM11320     1  0.0000      0.997 1.000 0.000
#&gt; GSM28797     1  0.0000      0.997 1.000 0.000
#&gt; GSM28786     1  0.0000      0.997 1.000 0.000
#&gt; GSM28800     1  0.0000      0.997 1.000 0.000
#&gt; GSM11310     1  0.0000      0.997 1.000 0.000
#&gt; GSM28787     1  0.4690      0.889 0.900 0.100
#&gt; GSM11304     1  0.0000      0.997 1.000 0.000
#&gt; GSM11303     1  0.0000      0.997 1.000 0.000
#&gt; GSM11317     1  0.0938      0.986 0.988 0.012
#&gt; GSM11311     1  0.0000      0.997 1.000 0.000
#&gt; GSM28799     1  0.0000      0.997 1.000 0.000
#&gt; GSM28791     1  0.0000      0.997 1.000 0.000
#&gt; GSM28794     2  0.0376      0.996 0.004 0.996
#&gt; GSM28780     1  0.0000      0.997 1.000 0.000
#&gt; GSM28795     1  0.0000      0.997 1.000 0.000
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000
#&gt; GSM11297     1  0.0000      0.997 1.000 0.000
#&gt; GSM11298     1  0.0000      0.997 1.000 0.000
#&gt; GSM11314     1  0.0000      0.997 1.000 0.000
#&gt; GSM11299     1  0.0000      0.997 1.000 0.000
#&gt; GSM28783     1  0.0000      0.997 1.000 0.000
#&gt; GSM11308     1  0.0000      0.997 1.000 0.000
#&gt; GSM28782     1  0.0000      0.997 1.000 0.000
#&gt; GSM28779     1  0.0000      0.997 1.000 0.000
#&gt; GSM11302     1  0.0000      0.997 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11309     1  0.2261      0.933 0.932 0.000 0.068
#&gt; GSM11315     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28797     1  0.1411      0.960 0.964 0.000 0.036
#&gt; GSM28786     1  0.3038      0.893 0.896 0.000 0.104
#&gt; GSM28800     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28787     3  0.5905      0.434 0.352 0.000 0.648
#&gt; GSM11304     1  0.2261      0.933 0.932 0.000 0.068
#&gt; GSM11303     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28794     2  0.0747      0.976 0.016 0.984 0.000
#&gt; GSM28780     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      0.998 0.000 1.000 0.000
#&gt; GSM11297     1  0.2261      0.933 0.932 0.000 0.068
#&gt; GSM11298     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11299     3  0.0000      0.938 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11308     1  0.0892      0.973 0.980 0.000 0.020
#&gt; GSM28782     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.987 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.987 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.3688     0.7794 0.792 0.000 0.000 0.208
#&gt; GSM28789     1  0.3942     0.7577 0.764 0.000 0.000 0.236
#&gt; GSM28790     1  0.2216     0.8234 0.908 0.000 0.000 0.092
#&gt; GSM11300     4  0.4522     0.4550 0.000 0.000 0.320 0.680
#&gt; GSM28798     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.2216     0.8234 0.908 0.000 0.000 0.092
#&gt; GSM28792     1  0.2216     0.8234 0.908 0.000 0.000 0.092
#&gt; GSM11295     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.1940     0.8301 0.924 0.000 0.000 0.076
#&gt; GSM11312     1  0.3649     0.7811 0.796 0.000 0.000 0.204
#&gt; GSM28778     1  0.0336     0.8419 0.992 0.000 0.000 0.008
#&gt; GSM28796     1  0.2216     0.8234 0.908 0.000 0.000 0.092
#&gt; GSM11309     4  0.1302     0.6680 0.044 0.000 0.000 0.956
#&gt; GSM11315     1  0.2216     0.8234 0.908 0.000 0.000 0.092
#&gt; GSM11306     1  0.3649     0.7811 0.796 0.000 0.000 0.204
#&gt; GSM28776     1  0.3764     0.7819 0.784 0.000 0.000 0.216
#&gt; GSM28777     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM28797     1  0.3942     0.7676 0.764 0.000 0.000 0.236
#&gt; GSM28786     4  0.4761    -0.0045 0.372 0.000 0.000 0.628
#&gt; GSM28800     4  0.4193     0.6810 0.268 0.000 0.000 0.732
#&gt; GSM11310     1  0.4072     0.7780 0.748 0.000 0.000 0.252
#&gt; GSM28787     3  0.6171     0.2579 0.348 0.000 0.588 0.064
#&gt; GSM11304     4  0.3873     0.6896 0.228 0.000 0.000 0.772
#&gt; GSM11303     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.8958 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.2589     0.8114 0.884 0.000 0.000 0.116
#&gt; GSM28799     4  0.3172     0.6534 0.160 0.000 0.000 0.840
#&gt; GSM28791     1  0.0592     0.8441 0.984 0.000 0.000 0.016
#&gt; GSM28794     2  0.1389     0.9384 0.048 0.952 0.000 0.000
#&gt; GSM28780     1  0.0707     0.8434 0.980 0.000 0.000 0.020
#&gt; GSM28795     4  0.4356     0.7057 0.292 0.000 0.000 0.708
#&gt; GSM11301     2  0.0000     0.9946 0.000 1.000 0.000 0.000
#&gt; GSM11297     4  0.1389     0.6891 0.048 0.000 0.000 0.952
#&gt; GSM11298     1  0.0000     0.8435 1.000 0.000 0.000 0.000
#&gt; GSM11314     1  0.4072     0.7767 0.748 0.000 0.000 0.252
#&gt; GSM11299     4  0.4522     0.4550 0.000 0.000 0.320 0.680
#&gt; GSM28783     1  0.4103     0.6595 0.744 0.000 0.000 0.256
#&gt; GSM11308     4  0.4500     0.7041 0.316 0.000 0.000 0.684
#&gt; GSM28782     1  0.0188     0.8430 0.996 0.000 0.000 0.004
#&gt; GSM28779     1  0.0592     0.8441 0.984 0.000 0.000 0.016
#&gt; GSM11302     1  0.0000     0.8435 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1   0.642      0.604 0.416 0.000 0.000 0.172 0.412
#&gt; GSM28789     5   0.646     -0.655 0.408 0.000 0.000 0.180 0.412
#&gt; GSM28790     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     5   0.595      0.269 0.000 0.000 0.312 0.132 0.556
#&gt; GSM28798     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28792     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11295     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11312     1   0.642      0.604 0.416 0.000 0.000 0.172 0.412
#&gt; GSM28778     1   0.426      0.661 0.564 0.000 0.000 0.000 0.436
#&gt; GSM28796     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4   0.207      0.674 0.000 0.000 0.000 0.896 0.104
#&gt; GSM11315     1   0.000      0.503 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     1   0.652      0.601 0.416 0.000 0.000 0.192 0.392
#&gt; GSM28776     1   0.639      0.615 0.472 0.000 0.000 0.176 0.352
#&gt; GSM28777     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4   0.283      0.704 0.044 0.000 0.000 0.876 0.080
#&gt; GSM28786     4   0.000      0.733 0.000 0.000 0.000 1.000 0.000
#&gt; GSM28800     5   0.516      0.286 0.440 0.000 0.000 0.040 0.520
#&gt; GSM11310     1   0.596      0.601 0.588 0.000 0.000 0.176 0.236
#&gt; GSM28787     3   0.495      0.424 0.256 0.000 0.676 0.068 0.000
#&gt; GSM11304     5   0.595      0.349 0.312 0.000 0.000 0.132 0.556
#&gt; GSM11303     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3   0.000      0.918 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     4   0.397      0.575 0.336 0.000 0.000 0.664 0.000
#&gt; GSM28799     5   0.489      0.287 0.052 0.000 0.000 0.288 0.660
#&gt; GSM28791     1   0.522      0.655 0.512 0.000 0.000 0.044 0.444
#&gt; GSM28794     2   0.385      0.599 0.016 0.752 0.000 0.000 0.232
#&gt; GSM28780     1   0.456      0.638 0.512 0.000 0.000 0.008 0.480
#&gt; GSM28795     5   0.412      0.369 0.100 0.000 0.000 0.112 0.788
#&gt; GSM11301     2   0.000      0.969 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     5   0.595      0.349 0.312 0.000 0.000 0.132 0.556
#&gt; GSM11298     1   0.421      0.666 0.588 0.000 0.000 0.000 0.412
#&gt; GSM11314     1   0.633      0.577 0.588 0.016 0.000 0.188 0.208
#&gt; GSM11299     5   0.595      0.269 0.000 0.000 0.312 0.132 0.556
#&gt; GSM28783     5   0.558     -0.380 0.352 0.000 0.000 0.084 0.564
#&gt; GSM11308     5   0.282      0.393 0.012 0.000 0.000 0.132 0.856
#&gt; GSM28782     1   0.431      0.638 0.508 0.000 0.000 0.000 0.492
#&gt; GSM28779     1   0.562      0.657 0.512 0.000 0.000 0.076 0.412
#&gt; GSM11302     1   0.421      0.666 0.588 0.000 0.000 0.000 0.412
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     5  0.2519      0.692 0.048 0.000 0.000 0.056 0.888 0.008
#&gt; GSM28789     5  0.3142      0.681 0.044 0.000 0.000 0.108 0.840 0.008
#&gt; GSM28790     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     6  0.2092      0.858 0.000 0.000 0.124 0.000 0.000 0.876
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11295     3  0.0972      0.919 0.000 0.000 0.964 0.000 0.028 0.008
#&gt; GSM28793     1  0.0260      0.923 0.992 0.000 0.000 0.000 0.008 0.000
#&gt; GSM11312     5  0.4129      0.682 0.092 0.000 0.000 0.020 0.776 0.112
#&gt; GSM28778     5  0.2006      0.695 0.104 0.000 0.000 0.000 0.892 0.004
#&gt; GSM28796     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11309     4  0.2092      0.765 0.000 0.000 0.000 0.876 0.000 0.124
#&gt; GSM11315     1  0.0000      0.933 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     5  0.6159      0.595 0.092 0.000 0.000 0.208 0.588 0.112
#&gt; GSM28776     5  0.6129      0.614 0.136 0.000 0.000 0.144 0.608 0.112
#&gt; GSM28777     3  0.0000      0.942 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.942 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000      0.942 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.0000      0.827 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28786     4  0.0458      0.832 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM28800     1  0.3330      0.491 0.716 0.000 0.000 0.000 0.000 0.284
#&gt; GSM11310     5  0.6643      0.562 0.208 0.000 0.000 0.148 0.532 0.112
#&gt; GSM28787     3  0.4954      0.651 0.000 0.000 0.724 0.084 0.076 0.116
#&gt; GSM11304     6  0.2092      0.857 0.124 0.000 0.000 0.000 0.000 0.876
#&gt; GSM11303     3  0.0000      0.942 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.942 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     4  0.3309      0.613 0.280 0.000 0.000 0.720 0.000 0.000
#&gt; GSM28799     5  0.5990      0.333 0.016 0.000 0.000 0.144 0.436 0.404
#&gt; GSM28791     5  0.2805      0.708 0.184 0.000 0.000 0.000 0.812 0.004
#&gt; GSM28794     5  0.3997      0.165 0.004 0.488 0.000 0.000 0.508 0.000
#&gt; GSM28780     5  0.5039      0.592 0.184 0.000 0.000 0.000 0.640 0.176
#&gt; GSM28795     5  0.4824      0.277 0.056 0.000 0.000 0.000 0.524 0.420
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6  0.2092      0.857 0.124 0.000 0.000 0.000 0.000 0.876
#&gt; GSM11298     5  0.2697      0.708 0.188 0.000 0.000 0.000 0.812 0.000
#&gt; GSM11314     5  0.7833      0.466 0.224 0.056 0.000 0.144 0.444 0.132
#&gt; GSM11299     6  0.2092      0.858 0.000 0.000 0.124 0.000 0.000 0.876
#&gt; GSM28783     5  0.5393      0.504 0.068 0.000 0.000 0.028 0.576 0.328
#&gt; GSM11308     6  0.2146      0.803 0.004 0.000 0.000 0.000 0.116 0.880
#&gt; GSM28782     5  0.2882      0.709 0.180 0.000 0.000 0.000 0.812 0.008
#&gt; GSM28779     5  0.2664      0.709 0.184 0.000 0.000 0.000 0.816 0.000
#&gt; GSM11302     5  0.2697      0.708 0.188 0.000 0.000 0.000 0.812 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:pam 52     0.396 2
#> MAD:pam 51     0.371 3
#> MAD:pam 48     0.448 4
#> MAD:pam 41     0.488 5
#> MAD:pam 47     0.484 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.656           0.758       0.897         0.4224 0.638   0.638
#> 3 3 0.747           0.857       0.935         0.4427 0.759   0.623
#> 4 4 0.771           0.806       0.894         0.1628 0.848   0.647
#> 5 5 0.685           0.741       0.822         0.0964 0.865   0.583
#> 6 6 0.725           0.642       0.740         0.0430 0.934   0.720
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.2948     0.8294 0.948 0.052
#&gt; GSM28789     1  0.2948     0.8294 0.948 0.052
#&gt; GSM28790     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11300     1  0.9954     0.3150 0.540 0.460
#&gt; GSM28798     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11296     2  0.0000     0.9543 0.000 1.000
#&gt; GSM28801     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11319     2  0.0000     0.9543 0.000 1.000
#&gt; GSM28781     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11305     2  0.0000     0.9543 0.000 1.000
#&gt; GSM28784     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11307     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11313     2  0.0000     0.9543 0.000 1.000
#&gt; GSM28785     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11318     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28792     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11295     1  0.9954     0.3150 0.540 0.460
#&gt; GSM28793     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11312     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28778     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28796     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11309     1  0.2778     0.8321 0.952 0.048
#&gt; GSM11315     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11306     1  0.2603     0.8344 0.956 0.044
#&gt; GSM28776     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28777     1  0.9963     0.3057 0.536 0.464
#&gt; GSM11316     1  0.9963     0.3057 0.536 0.464
#&gt; GSM11320     1  0.9963     0.3057 0.536 0.464
#&gt; GSM28797     1  0.2778     0.8321 0.952 0.048
#&gt; GSM28786     1  0.2778     0.8321 0.952 0.048
#&gt; GSM28800     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11310     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28787     1  0.9954     0.3150 0.540 0.460
#&gt; GSM11304     1  0.4161     0.8108 0.916 0.084
#&gt; GSM11303     1  0.9963     0.3057 0.536 0.464
#&gt; GSM11317     1  0.9963     0.3057 0.536 0.464
#&gt; GSM11311     1  0.1184     0.8483 0.984 0.016
#&gt; GSM28799     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28791     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28794     2  0.9850     0.0594 0.428 0.572
#&gt; GSM28780     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28795     1  0.0376     0.8535 0.996 0.004
#&gt; GSM11301     2  0.0000     0.9543 0.000 1.000
#&gt; GSM11297     1  0.4298     0.8079 0.912 0.088
#&gt; GSM11298     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11314     1  0.7453     0.6923 0.788 0.212
#&gt; GSM11299     1  0.9954     0.3150 0.540 0.460
#&gt; GSM28783     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11308     1  0.3274     0.8264 0.940 0.060
#&gt; GSM28782     1  0.0000     0.8547 1.000 0.000
#&gt; GSM28779     1  0.0000     0.8547 1.000 0.000
#&gt; GSM11302     1  0.0000     0.8547 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.3375     0.8798 0.892 0.008 0.100
#&gt; GSM28789     1  0.4139     0.8574 0.860 0.016 0.124
#&gt; GSM28790     1  0.0000     0.9132 1.000 0.000 0.000
#&gt; GSM11300     3  0.0424     0.8931 0.008 0.000 0.992
#&gt; GSM28798     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000     0.9708 0.000 1.000 0.000
#&gt; GSM11318     1  0.0237     0.9139 0.996 0.000 0.004
#&gt; GSM28792     1  0.0424     0.9137 0.992 0.000 0.008
#&gt; GSM11295     3  0.0237     0.8940 0.004 0.000 0.996
#&gt; GSM28793     1  0.0592     0.9122 0.988 0.012 0.000
#&gt; GSM11312     1  0.0747     0.9109 0.984 0.016 0.000
#&gt; GSM28778     1  0.7525     0.6397 0.684 0.208 0.108
#&gt; GSM28796     1  0.0237     0.9135 0.996 0.000 0.004
#&gt; GSM11309     1  0.3192     0.8757 0.888 0.000 0.112
#&gt; GSM11315     1  0.0661     0.9130 0.988 0.008 0.004
#&gt; GSM11306     1  0.2959     0.8825 0.900 0.000 0.100
#&gt; GSM28776     1  0.1643     0.9077 0.956 0.000 0.044
#&gt; GSM28777     3  0.0000     0.8952 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000     0.8952 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000     0.8952 0.000 0.000 1.000
#&gt; GSM28797     1  0.3192     0.8757 0.888 0.000 0.112
#&gt; GSM28786     1  0.3192     0.8757 0.888 0.000 0.112
#&gt; GSM28800     1  0.0829     0.9119 0.984 0.012 0.004
#&gt; GSM11310     1  0.1643     0.9077 0.956 0.000 0.044
#&gt; GSM28787     3  0.0661     0.8909 0.008 0.004 0.988
#&gt; GSM11304     3  0.5905     0.4369 0.352 0.000 0.648
#&gt; GSM11303     3  0.0000     0.8952 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000     0.8952 0.000 0.000 1.000
#&gt; GSM11311     1  0.2165     0.9001 0.936 0.000 0.064
#&gt; GSM28799     1  0.1643     0.9077 0.956 0.000 0.044
#&gt; GSM28791     1  0.0237     0.9135 0.996 0.000 0.004
#&gt; GSM28794     2  0.5292     0.7255 0.172 0.800 0.028
#&gt; GSM28780     1  0.0892     0.9118 0.980 0.000 0.020
#&gt; GSM28795     1  0.3682     0.8485 0.876 0.008 0.116
#&gt; GSM11301     2  0.1989     0.9166 0.048 0.948 0.004
#&gt; GSM11297     3  0.6225     0.2047 0.432 0.000 0.568
#&gt; GSM11298     1  0.0237     0.9133 0.996 0.004 0.000
#&gt; GSM11314     1  0.8892    -0.0143 0.444 0.120 0.436
#&gt; GSM11299     3  0.0424     0.8931 0.008 0.000 0.992
#&gt; GSM28783     1  0.0424     0.9139 0.992 0.000 0.008
#&gt; GSM11308     1  0.6357     0.5145 0.684 0.020 0.296
#&gt; GSM28782     1  0.0000     0.9132 1.000 0.000 0.000
#&gt; GSM28779     1  0.0424     0.9131 0.992 0.008 0.000
#&gt; GSM11302     1  0.0892     0.9092 0.980 0.020 0.000
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.1389     0.9212 0.048 0.000 0.000 0.952
#&gt; GSM28789     4  0.1022     0.9229 0.032 0.000 0.000 0.968
#&gt; GSM28790     1  0.0188     0.8033 0.996 0.000 0.000 0.004
#&gt; GSM11300     3  0.0779     0.9772 0.016 0.000 0.980 0.004
#&gt; GSM28798     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9306 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0592     0.8043 0.984 0.000 0.000 0.016
#&gt; GSM28792     1  0.0921     0.8076 0.972 0.000 0.000 0.028
#&gt; GSM11295     3  0.0817     0.9772 0.000 0.000 0.976 0.024
#&gt; GSM28793     1  0.0188     0.8041 0.996 0.000 0.000 0.004
#&gt; GSM11312     1  0.2345     0.7973 0.900 0.000 0.000 0.100
#&gt; GSM28778     1  0.5883     0.6369 0.648 0.064 0.000 0.288
#&gt; GSM28796     1  0.0188     0.8041 0.996 0.000 0.000 0.004
#&gt; GSM11309     4  0.0921     0.9294 0.028 0.000 0.000 0.972
#&gt; GSM11315     1  0.0336     0.8047 0.992 0.000 0.000 0.008
#&gt; GSM11306     4  0.3764     0.6750 0.216 0.000 0.000 0.784
#&gt; GSM28776     1  0.4431     0.6827 0.696 0.000 0.000 0.304
#&gt; GSM28777     3  0.0000     0.9872 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9872 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9872 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.0921     0.9294 0.028 0.000 0.000 0.972
#&gt; GSM28786     4  0.0592     0.9209 0.016 0.000 0.000 0.984
#&gt; GSM28800     1  0.0188     0.8041 0.996 0.000 0.000 0.004
#&gt; GSM11310     1  0.4428     0.6985 0.720 0.000 0.004 0.276
#&gt; GSM28787     3  0.1211     0.9665 0.000 0.000 0.960 0.040
#&gt; GSM11304     1  0.7499     0.2394 0.420 0.000 0.400 0.180
#&gt; GSM11303     3  0.0000     0.9872 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9872 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.4713     0.5915 0.640 0.000 0.000 0.360
#&gt; GSM28799     1  0.5432     0.6373 0.652 0.000 0.032 0.316
#&gt; GSM28791     1  0.2081     0.8008 0.916 0.000 0.000 0.084
#&gt; GSM28794     2  0.7660    -0.0651 0.324 0.448 0.000 0.228
#&gt; GSM28780     1  0.3356     0.7719 0.824 0.000 0.000 0.176
#&gt; GSM28795     1  0.3975     0.7350 0.760 0.000 0.000 0.240
#&gt; GSM11301     2  0.1716     0.8648 0.000 0.936 0.000 0.064
#&gt; GSM11297     1  0.7530     0.2755 0.436 0.000 0.376 0.188
#&gt; GSM11298     1  0.0336     0.8056 0.992 0.000 0.000 0.008
#&gt; GSM11314     1  0.8040     0.4788 0.524 0.060 0.112 0.304
#&gt; GSM11299     3  0.0779     0.9772 0.016 0.000 0.980 0.004
#&gt; GSM28783     1  0.3444     0.7677 0.816 0.000 0.000 0.184
#&gt; GSM11308     1  0.5156     0.7137 0.720 0.000 0.044 0.236
#&gt; GSM28782     1  0.0469     0.8064 0.988 0.000 0.000 0.012
#&gt; GSM28779     1  0.0592     0.8065 0.984 0.000 0.000 0.016
#&gt; GSM11302     1  0.0469     0.8068 0.988 0.000 0.000 0.012
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.4168     0.7806 0.052 0.000 0.000 0.764 0.184
#&gt; GSM28789     4  0.3694     0.8008 0.032 0.000 0.000 0.796 0.172
#&gt; GSM28790     1  0.2806     0.7303 0.844 0.000 0.000 0.004 0.152
#&gt; GSM11300     3  0.2104     0.9338 0.000 0.000 0.916 0.024 0.060
#&gt; GSM28798     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0162     0.9851 0.000 0.996 0.000 0.000 0.004
#&gt; GSM11307     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9883 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.2513     0.7167 0.876 0.000 0.000 0.008 0.116
#&gt; GSM28792     1  0.3163     0.6816 0.824 0.000 0.000 0.012 0.164
#&gt; GSM11295     3  0.2561     0.9217 0.000 0.000 0.884 0.020 0.096
#&gt; GSM28793     1  0.0579     0.7508 0.984 0.000 0.000 0.008 0.008
#&gt; GSM11312     1  0.4251     0.3633 0.672 0.000 0.000 0.012 0.316
#&gt; GSM28778     5  0.4916     0.6150 0.192 0.008 0.008 0.060 0.732
#&gt; GSM28796     1  0.0798     0.7488 0.976 0.000 0.000 0.016 0.008
#&gt; GSM11309     4  0.0798     0.8312 0.008 0.000 0.000 0.976 0.016
#&gt; GSM11315     1  0.0693     0.7507 0.980 0.000 0.000 0.012 0.008
#&gt; GSM11306     4  0.5417     0.6223 0.116 0.000 0.000 0.648 0.236
#&gt; GSM28776     5  0.5447     0.6050 0.248 0.000 0.000 0.112 0.640
#&gt; GSM28777     3  0.0000     0.9513 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9513 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9513 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4  0.0771     0.8318 0.004 0.000 0.000 0.976 0.020
#&gt; GSM28786     4  0.0451     0.8271 0.004 0.000 0.000 0.988 0.008
#&gt; GSM28800     1  0.1124     0.7440 0.960 0.000 0.000 0.004 0.036
#&gt; GSM11310     5  0.6012     0.4564 0.376 0.000 0.000 0.120 0.504
#&gt; GSM28787     3  0.2921     0.9037 0.000 0.000 0.856 0.020 0.124
#&gt; GSM11304     5  0.7831     0.4696 0.196 0.000 0.240 0.108 0.456
#&gt; GSM11303     3  0.0000     0.9513 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9513 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     5  0.6391     0.4891 0.316 0.000 0.004 0.168 0.512
#&gt; GSM28799     5  0.6296     0.5721 0.304 0.000 0.016 0.124 0.556
#&gt; GSM28791     1  0.4504     0.3076 0.564 0.000 0.000 0.008 0.428
#&gt; GSM28794     5  0.5963     0.4497 0.084 0.180 0.000 0.064 0.672
#&gt; GSM28780     5  0.4627     0.0657 0.444 0.000 0.000 0.012 0.544
#&gt; GSM28795     5  0.4516     0.5228 0.264 0.000 0.008 0.024 0.704
#&gt; GSM11301     2  0.2700     0.8777 0.004 0.884 0.000 0.024 0.088
#&gt; GSM11297     5  0.7793     0.4764 0.192 0.000 0.236 0.108 0.464
#&gt; GSM11298     1  0.2516     0.7164 0.860 0.000 0.000 0.000 0.140
#&gt; GSM11314     5  0.4970     0.6149 0.132 0.008 0.024 0.076 0.760
#&gt; GSM11299     3  0.3197     0.9002 0.008 0.000 0.852 0.024 0.116
#&gt; GSM28783     5  0.5460     0.3097 0.420 0.000 0.004 0.052 0.524
#&gt; GSM11308     5  0.6810     0.4340 0.344 0.000 0.068 0.080 0.508
#&gt; GSM28782     1  0.3550     0.6904 0.760 0.000 0.000 0.004 0.236
#&gt; GSM28779     1  0.2966     0.6811 0.816 0.000 0.000 0.000 0.184
#&gt; GSM11302     1  0.2929     0.6810 0.820 0.000 0.000 0.000 0.180
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28788     4  0.4614    0.78734 0.028 0.000 0.000 0.728 0.168 NA
#&gt; GSM28789     4  0.4345    0.79511 0.016 0.000 0.000 0.744 0.164 NA
#&gt; GSM28790     1  0.1493    0.62615 0.936 0.000 0.000 0.004 0.056 NA
#&gt; GSM11300     3  0.2009    0.78046 0.000 0.000 0.904 0.004 0.008 NA
#&gt; GSM28798     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11296     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28801     2  0.1333    0.94512 0.000 0.944 0.000 0.008 0.000 NA
#&gt; GSM11319     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28781     2  0.0146    0.97149 0.000 0.996 0.000 0.004 0.000 NA
#&gt; GSM11305     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28784     2  0.1196    0.94981 0.000 0.952 0.000 0.008 0.000 NA
#&gt; GSM11307     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11313     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM28785     2  0.0000    0.97286 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM11318     1  0.1410    0.62659 0.944 0.000 0.000 0.004 0.044 NA
#&gt; GSM28792     1  0.2056    0.60246 0.904 0.000 0.000 0.004 0.080 NA
#&gt; GSM11295     3  0.1774    0.78676 0.004 0.000 0.936 0.016 0.020 NA
#&gt; GSM28793     1  0.5365    0.57739 0.616 0.000 0.000 0.008 0.160 NA
#&gt; GSM11312     5  0.5445    0.48468 0.188 0.000 0.000 0.020 0.632 NA
#&gt; GSM28778     5  0.3411    0.59143 0.092 0.004 0.000 0.016 0.836 NA
#&gt; GSM28796     1  0.3658    0.63317 0.800 0.000 0.000 0.004 0.092 NA
#&gt; GSM11309     4  0.1053    0.81762 0.004 0.000 0.000 0.964 0.020 NA
#&gt; GSM11315     1  0.5359    0.57257 0.604 0.000 0.000 0.004 0.164 NA
#&gt; GSM11306     4  0.6384    0.55694 0.072 0.000 0.000 0.528 0.272 NA
#&gt; GSM28776     5  0.4584    0.58151 0.112 0.000 0.000 0.040 0.748 NA
#&gt; GSM28777     3  0.2048    0.81741 0.000 0.000 0.880 0.000 0.000 NA
#&gt; GSM11316     3  0.3446    0.80895 0.000 0.000 0.692 0.000 0.000 NA
#&gt; GSM11320     3  0.3482    0.80891 0.000 0.000 0.684 0.000 0.000 NA
#&gt; GSM28797     4  0.0972    0.81851 0.000 0.000 0.000 0.964 0.028 NA
#&gt; GSM28786     4  0.0632    0.81668 0.000 0.000 0.000 0.976 0.024 NA
#&gt; GSM28800     1  0.5453    0.55865 0.592 0.000 0.000 0.004 0.184 NA
#&gt; GSM11310     5  0.4901    0.57198 0.144 0.000 0.000 0.028 0.708 NA
#&gt; GSM28787     3  0.2838    0.76364 0.008 0.000 0.880 0.016 0.044 NA
#&gt; GSM11304     5  0.5131    0.50374 0.048 0.000 0.016 0.032 0.680 NA
#&gt; GSM11303     3  0.3482    0.80891 0.000 0.000 0.684 0.000 0.000 NA
#&gt; GSM11317     3  0.3446    0.80895 0.000 0.000 0.692 0.000 0.000 NA
#&gt; GSM11311     5  0.4579    0.58032 0.076 0.000 0.000 0.068 0.756 NA
#&gt; GSM28799     5  0.4371    0.58242 0.040 0.000 0.004 0.044 0.760 NA
#&gt; GSM28791     1  0.4959   -0.05346 0.556 0.000 0.000 0.016 0.388 NA
#&gt; GSM28794     5  0.5659    0.44996 0.044 0.184 0.000 0.012 0.656 NA
#&gt; GSM28780     5  0.5197    0.18675 0.452 0.000 0.000 0.016 0.480 NA
#&gt; GSM28795     5  0.4900    0.45911 0.296 0.000 0.000 0.032 0.636 NA
#&gt; GSM11301     2  0.3945    0.80981 0.020 0.808 0.000 0.012 0.088 NA
#&gt; GSM11297     5  0.5062    0.50293 0.048 0.000 0.016 0.028 0.684 NA
#&gt; GSM11298     5  0.6060   -0.13631 0.364 0.000 0.000 0.004 0.416 NA
#&gt; GSM11314     5  0.3508    0.58598 0.076 0.000 0.008 0.032 0.840 NA
#&gt; GSM11299     3  0.2633    0.75866 0.000 0.000 0.864 0.004 0.020 NA
#&gt; GSM28783     5  0.3890    0.55503 0.200 0.000 0.004 0.020 0.760 NA
#&gt; GSM11308     5  0.6202    0.31152 0.332 0.000 0.004 0.028 0.496 NA
#&gt; GSM28782     1  0.4289    0.24131 0.660 0.000 0.000 0.004 0.304 NA
#&gt; GSM28779     5  0.6032   -0.03072 0.320 0.000 0.000 0.004 0.452 NA
#&gt; GSM11302     5  0.6015   -0.00472 0.312 0.000 0.000 0.004 0.460 NA
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> MAD:mclust 42     0.384 2
#> MAD:mclust 49     0.368 3
#> MAD:mclust 48     0.462 4
#> MAD:mclust 42     0.428 5
#> MAD:mclust 42     0.452 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.980       0.991         0.3953 0.599   0.599
#> 3 3 1.000           0.970       0.988         0.4846 0.729   0.576
#> 4 4 0.723           0.780       0.884         0.2428 0.880   0.712
#> 5 5 0.820           0.804       0.898         0.0917 0.881   0.619
#> 6 6 0.855           0.729       0.869         0.0450 0.943   0.741
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0376      0.995 0.996 0.004
#&gt; GSM28789     2  0.8443      0.641 0.272 0.728
#&gt; GSM28790     1  0.0000      0.998 1.000 0.000
#&gt; GSM11300     1  0.0000      0.998 1.000 0.000
#&gt; GSM28798     2  0.0000      0.969 0.000 1.000
#&gt; GSM11296     2  0.0000      0.969 0.000 1.000
#&gt; GSM28801     2  0.0000      0.969 0.000 1.000
#&gt; GSM11319     2  0.0000      0.969 0.000 1.000
#&gt; GSM28781     2  0.0000      0.969 0.000 1.000
#&gt; GSM11305     2  0.0000      0.969 0.000 1.000
#&gt; GSM28784     2  0.0000      0.969 0.000 1.000
#&gt; GSM11307     2  0.0000      0.969 0.000 1.000
#&gt; GSM11313     2  0.0000      0.969 0.000 1.000
#&gt; GSM28785     2  0.0000      0.969 0.000 1.000
#&gt; GSM11318     1  0.0000      0.998 1.000 0.000
#&gt; GSM28792     1  0.0000      0.998 1.000 0.000
#&gt; GSM11295     1  0.0000      0.998 1.000 0.000
#&gt; GSM28793     1  0.0000      0.998 1.000 0.000
#&gt; GSM11312     1  0.0000      0.998 1.000 0.000
#&gt; GSM28778     2  0.5519      0.850 0.128 0.872
#&gt; GSM28796     1  0.0000      0.998 1.000 0.000
#&gt; GSM11309     1  0.0000      0.998 1.000 0.000
#&gt; GSM11315     1  0.0000      0.998 1.000 0.000
#&gt; GSM11306     1  0.0000      0.998 1.000 0.000
#&gt; GSM28776     1  0.0000      0.998 1.000 0.000
#&gt; GSM28777     1  0.0000      0.998 1.000 0.000
#&gt; GSM11316     1  0.0672      0.991 0.992 0.008
#&gt; GSM11320     1  0.0000      0.998 1.000 0.000
#&gt; GSM28797     1  0.0000      0.998 1.000 0.000
#&gt; GSM28786     1  0.0000      0.998 1.000 0.000
#&gt; GSM28800     1  0.0000      0.998 1.000 0.000
#&gt; GSM11310     1  0.0000      0.998 1.000 0.000
#&gt; GSM28787     1  0.2423      0.958 0.960 0.040
#&gt; GSM11304     1  0.0000      0.998 1.000 0.000
#&gt; GSM11303     1  0.0000      0.998 1.000 0.000
#&gt; GSM11317     1  0.0000      0.998 1.000 0.000
#&gt; GSM11311     1  0.0000      0.998 1.000 0.000
#&gt; GSM28799     1  0.0000      0.998 1.000 0.000
#&gt; GSM28791     1  0.0000      0.998 1.000 0.000
#&gt; GSM28794     2  0.0000      0.969 0.000 1.000
#&gt; GSM28780     1  0.0000      0.998 1.000 0.000
#&gt; GSM28795     1  0.0000      0.998 1.000 0.000
#&gt; GSM11301     2  0.0000      0.969 0.000 1.000
#&gt; GSM11297     1  0.0000      0.998 1.000 0.000
#&gt; GSM11298     1  0.0000      0.998 1.000 0.000
#&gt; GSM11314     1  0.1414      0.979 0.980 0.020
#&gt; GSM11299     1  0.0000      0.998 1.000 0.000
#&gt; GSM28783     1  0.0000      0.998 1.000 0.000
#&gt; GSM11308     1  0.0000      0.998 1.000 0.000
#&gt; GSM28782     1  0.0000      0.998 1.000 0.000
#&gt; GSM28779     1  0.0000      0.998 1.000 0.000
#&gt; GSM11302     1  0.0000      0.998 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28789     1  0.1289      0.962 0.968 0.032 0.000
#&gt; GSM28790     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11309     1  0.0237      0.987 0.996 0.000 0.004
#&gt; GSM11315     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28786     3  0.5859      0.461 0.344 0.000 0.656
#&gt; GSM28800     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM11304     1  0.1753      0.947 0.952 0.000 0.048
#&gt; GSM11303     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28780     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11297     1  0.3879      0.818 0.848 0.000 0.152
#&gt; GSM11298     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11314     1  0.1482      0.965 0.968 0.012 0.020
#&gt; GSM11299     3  0.0000      0.948 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.990 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.990 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.2216     0.8896 0.092 0.000 0.000 0.908
#&gt; GSM28789     4  0.2179     0.8848 0.064 0.012 0.000 0.924
#&gt; GSM28790     1  0.0188     0.7599 0.996 0.000 0.000 0.004
#&gt; GSM11300     3  0.0188     0.9381 0.000 0.000 0.996 0.004
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0188     0.7582 0.996 0.000 0.000 0.004
#&gt; GSM28792     1  0.0707     0.7615 0.980 0.000 0.000 0.020
#&gt; GSM11295     3  0.3569     0.7843 0.000 0.000 0.804 0.196
#&gt; GSM28793     1  0.1474     0.7605 0.948 0.000 0.000 0.052
#&gt; GSM11312     1  0.4072     0.6599 0.748 0.000 0.000 0.252
#&gt; GSM28778     1  0.4072     0.6373 0.748 0.000 0.000 0.252
#&gt; GSM28796     1  0.1474     0.7605 0.948 0.000 0.000 0.052
#&gt; GSM11309     4  0.2530     0.8840 0.100 0.000 0.004 0.896
#&gt; GSM11315     1  0.1557     0.7592 0.944 0.000 0.000 0.056
#&gt; GSM11306     4  0.1389     0.8659 0.048 0.000 0.000 0.952
#&gt; GSM28776     1  0.4277     0.5616 0.720 0.000 0.000 0.280
#&gt; GSM28777     3  0.0188     0.9370 0.000 0.000 0.996 0.004
#&gt; GSM11316     3  0.0000     0.9386 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9386 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.2081     0.8910 0.084 0.000 0.000 0.916
#&gt; GSM28786     4  0.3307     0.8185 0.028 0.000 0.104 0.868
#&gt; GSM28800     1  0.1940     0.7519 0.924 0.000 0.000 0.076
#&gt; GSM11310     1  0.4985    -0.0198 0.532 0.000 0.000 0.468
#&gt; GSM28787     3  0.4313     0.7044 0.004 0.000 0.736 0.260
#&gt; GSM11304     1  0.5076     0.6224 0.756 0.000 0.172 0.072
#&gt; GSM11303     3  0.0188     0.9381 0.000 0.000 0.996 0.004
#&gt; GSM11317     3  0.0000     0.9386 0.000 0.000 1.000 0.000
#&gt; GSM11311     4  0.4746     0.4486 0.368 0.000 0.000 0.632
#&gt; GSM28799     1  0.4855     0.2550 0.600 0.000 0.000 0.400
#&gt; GSM28791     1  0.3942     0.6561 0.764 0.000 0.000 0.236
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28780     1  0.4356     0.6040 0.708 0.000 0.000 0.292
#&gt; GSM28795     1  0.4746     0.5014 0.632 0.000 0.000 0.368
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.6640     0.3173 0.552 0.000 0.352 0.096
#&gt; GSM11298     1  0.1302     0.7616 0.956 0.000 0.000 0.044
#&gt; GSM11314     1  0.6557     0.3992 0.548 0.004 0.072 0.376
#&gt; GSM11299     3  0.0188     0.9381 0.000 0.000 0.996 0.004
#&gt; GSM28783     1  0.4564     0.5576 0.672 0.000 0.000 0.328
#&gt; GSM11308     1  0.3355     0.7058 0.836 0.000 0.004 0.160
#&gt; GSM28782     1  0.0817     0.7546 0.976 0.000 0.000 0.024
#&gt; GSM28779     1  0.1474     0.7605 0.948 0.000 0.000 0.052
#&gt; GSM11302     1  0.1474     0.7614 0.948 0.000 0.000 0.052
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1 p2    p3    p4    p5
#&gt; GSM28788     4  0.0671     0.9501 0.016  0 0.000 0.980 0.004
#&gt; GSM28789     4  0.0579     0.9467 0.008  0 0.000 0.984 0.008
#&gt; GSM28790     1  0.2280     0.7809 0.880  0 0.000 0.000 0.120
#&gt; GSM11300     3  0.0898     0.9687 0.008  0 0.972 0.000 0.020
#&gt; GSM28798     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11318     1  0.2280     0.7788 0.880  0 0.000 0.000 0.120
#&gt; GSM28792     1  0.2068     0.7998 0.904  0 0.000 0.004 0.092
#&gt; GSM11295     5  0.5318     0.0102 0.004  0 0.460 0.040 0.496
#&gt; GSM28793     1  0.1041     0.8127 0.964  0 0.000 0.004 0.032
#&gt; GSM11312     5  0.6643    -0.0233 0.372  0 0.000 0.224 0.404
#&gt; GSM28778     5  0.3366     0.6574 0.212  0 0.000 0.004 0.784
#&gt; GSM28796     1  0.0703     0.8112 0.976  0 0.000 0.000 0.024
#&gt; GSM11309     4  0.0771     0.9488 0.020  0 0.000 0.976 0.004
#&gt; GSM11315     1  0.0510     0.8097 0.984  0 0.000 0.000 0.016
#&gt; GSM11306     4  0.1809     0.9080 0.012  0 0.000 0.928 0.060
#&gt; GSM28776     1  0.5570     0.5154 0.608  0 0.000 0.288 0.104
#&gt; GSM28777     3  0.0798     0.9703 0.000  0 0.976 0.008 0.016
#&gt; GSM11316     3  0.0794     0.9664 0.000  0 0.972 0.000 0.028
#&gt; GSM11320     3  0.0000     0.9804 0.000  0 1.000 0.000 0.000
#&gt; GSM28797     4  0.0671     0.9501 0.016  0 0.000 0.980 0.004
#&gt; GSM28786     4  0.0865     0.9462 0.024  0 0.004 0.972 0.000
#&gt; GSM28800     1  0.0794     0.8030 0.972  0 0.000 0.000 0.028
#&gt; GSM11310     1  0.3398     0.6576 0.780  0 0.000 0.216 0.004
#&gt; GSM28787     5  0.5480     0.3034 0.012  0 0.340 0.052 0.596
#&gt; GSM11304     1  0.5098     0.5356 0.660  0 0.276 0.004 0.060
#&gt; GSM11303     3  0.0000     0.9804 0.000  0 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9804 0.000  0 1.000 0.000 0.000
#&gt; GSM11311     4  0.3171     0.7842 0.176  0 0.000 0.816 0.008
#&gt; GSM28799     1  0.4138     0.3627 0.616  0 0.000 0.384 0.000
#&gt; GSM28791     5  0.2179     0.7616 0.112  0 0.000 0.000 0.888
#&gt; GSM28794     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM28780     5  0.1965     0.7675 0.096  0 0.000 0.000 0.904
#&gt; GSM28795     5  0.1205     0.7644 0.040  0 0.000 0.004 0.956
#&gt; GSM11301     2  0.0000     1.0000 0.000  1 0.000 0.000 0.000
#&gt; GSM11297     1  0.5707     0.3488 0.564  0 0.364 0.016 0.056
#&gt; GSM11298     1  0.1430     0.8111 0.944  0 0.000 0.004 0.052
#&gt; GSM11314     5  0.1074     0.7463 0.012  0 0.016 0.004 0.968
#&gt; GSM11299     3  0.1117     0.9629 0.016  0 0.964 0.000 0.020
#&gt; GSM28783     5  0.1831     0.7701 0.076  0 0.000 0.004 0.920
#&gt; GSM11308     5  0.2179     0.7615 0.112  0 0.000 0.000 0.888
#&gt; GSM28782     1  0.3586     0.6184 0.736  0 0.000 0.000 0.264
#&gt; GSM28779     1  0.1121     0.8129 0.956  0 0.000 0.000 0.044
#&gt; GSM11302     1  0.1638     0.8093 0.932  0 0.000 0.004 0.064
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.0603     0.9541 0.004 0.000 0.000 0.980 0.000 0.016
#&gt; GSM28789     4  0.0748     0.9534 0.004 0.000 0.000 0.976 0.004 0.016
#&gt; GSM28790     1  0.0909     0.7805 0.968 0.000 0.000 0.000 0.020 0.012
#&gt; GSM11300     3  0.2996     0.5572 0.000 0.000 0.772 0.000 0.000 0.228
#&gt; GSM28798     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0146     0.9956 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28781     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0146     0.9956 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11307     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9964 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0146     0.9956 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11318     1  0.1297     0.7725 0.948 0.000 0.000 0.000 0.012 0.040
#&gt; GSM28792     1  0.2118     0.7268 0.888 0.000 0.000 0.000 0.008 0.104
#&gt; GSM11295     3  0.6133     0.0947 0.008 0.000 0.484 0.016 0.140 0.352
#&gt; GSM28793     1  0.0692     0.7806 0.976 0.000 0.000 0.000 0.004 0.020
#&gt; GSM11312     5  0.6027     0.4441 0.184 0.000 0.000 0.200 0.576 0.040
#&gt; GSM28778     5  0.1225     0.8126 0.036 0.000 0.000 0.000 0.952 0.012
#&gt; GSM28796     1  0.0547     0.7824 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM11309     4  0.0508     0.9543 0.004 0.000 0.000 0.984 0.000 0.012
#&gt; GSM11315     1  0.0405     0.7824 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM11306     4  0.1152     0.9313 0.000 0.000 0.000 0.952 0.004 0.044
#&gt; GSM28776     1  0.5840     0.3503 0.536 0.000 0.000 0.216 0.008 0.240
#&gt; GSM28777     3  0.1588     0.7252 0.000 0.000 0.924 0.000 0.004 0.072
#&gt; GSM11316     3  0.1265     0.7389 0.000 0.000 0.948 0.000 0.008 0.044
#&gt; GSM11320     3  0.0363     0.7552 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM28797     4  0.0291     0.9555 0.004 0.000 0.000 0.992 0.000 0.004
#&gt; GSM28786     4  0.0748     0.9517 0.004 0.000 0.004 0.976 0.000 0.016
#&gt; GSM28800     1  0.4333     0.1846 0.512 0.000 0.000 0.000 0.020 0.468
#&gt; GSM11310     1  0.4988     0.1721 0.484 0.000 0.000 0.068 0.000 0.448
#&gt; GSM28787     6  0.7214    -0.2731 0.032 0.000 0.348 0.032 0.228 0.360
#&gt; GSM11304     6  0.5742     0.4495 0.152 0.000 0.236 0.000 0.024 0.588
#&gt; GSM11303     3  0.0260     0.7550 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM11317     3  0.0363     0.7552 0.000 0.000 0.988 0.000 0.000 0.012
#&gt; GSM11311     4  0.2771     0.8310 0.116 0.000 0.000 0.852 0.000 0.032
#&gt; GSM28799     1  0.5956     0.2643 0.488 0.000 0.004 0.236 0.000 0.272
#&gt; GSM28791     5  0.1434     0.8077 0.048 0.000 0.000 0.000 0.940 0.012
#&gt; GSM28794     2  0.0713     0.9755 0.000 0.972 0.000 0.000 0.000 0.028
#&gt; GSM28780     5  0.0622     0.8118 0.012 0.000 0.000 0.000 0.980 0.008
#&gt; GSM28795     5  0.0146     0.8076 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; GSM11301     2  0.0146     0.9956 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM11297     6  0.5747     0.4564 0.156 0.000 0.220 0.004 0.020 0.600
#&gt; GSM11298     1  0.0146     0.7831 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11314     5  0.1367     0.7852 0.000 0.000 0.012 0.000 0.944 0.044
#&gt; GSM11299     3  0.3684     0.3621 0.000 0.000 0.664 0.000 0.004 0.332
#&gt; GSM28783     5  0.2070     0.7872 0.012 0.000 0.000 0.000 0.896 0.092
#&gt; GSM11308     5  0.2263     0.7734 0.016 0.000 0.000 0.000 0.884 0.100
#&gt; GSM28782     5  0.5984     0.1271 0.280 0.000 0.000 0.000 0.444 0.276
#&gt; GSM28779     1  0.1411     0.7662 0.936 0.000 0.000 0.000 0.004 0.060
#&gt; GSM11302     1  0.1168     0.7793 0.956 0.000 0.000 0.000 0.016 0.028
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> MAD:NMF 52     0.396 2
#> MAD:NMF 51     0.371 3
#> MAD:NMF 47     0.462 4
#> MAD:NMF 47     0.424 5
#> MAD:NMF 41     0.423 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.997       0.997         0.2930 0.708   0.708
#> 3 3 0.908           0.962       0.983         0.9035 0.719   0.604
#> 4 4 0.956           0.930       0.973         0.0889 0.977   0.947
#> 5 5 0.915           0.841       0.912         0.0223 0.986   0.966
#> 6 6 0.926           0.877       0.938         0.0356 0.958   0.892
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2 3 4
```

There is also optional best $k$ = 2 3 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.998 1.000 0.000
#&gt; GSM28789     1  0.0000      0.998 1.000 0.000
#&gt; GSM28790     1  0.0000      0.998 1.000 0.000
#&gt; GSM11300     2  0.0672      1.000 0.008 0.992
#&gt; GSM28798     1  0.0672      0.994 0.992 0.008
#&gt; GSM11296     1  0.0672      0.994 0.992 0.008
#&gt; GSM28801     1  0.0672      0.994 0.992 0.008
#&gt; GSM11319     1  0.0672      0.994 0.992 0.008
#&gt; GSM28781     1  0.0672      0.994 0.992 0.008
#&gt; GSM11305     1  0.0672      0.994 0.992 0.008
#&gt; GSM28784     1  0.0672      0.994 0.992 0.008
#&gt; GSM11307     1  0.0672      0.994 0.992 0.008
#&gt; GSM11313     1  0.0672      0.994 0.992 0.008
#&gt; GSM28785     1  0.0672      0.994 0.992 0.008
#&gt; GSM11318     1  0.0000      0.998 1.000 0.000
#&gt; GSM28792     1  0.0000      0.998 1.000 0.000
#&gt; GSM11295     2  0.0672      1.000 0.008 0.992
#&gt; GSM28793     1  0.0000      0.998 1.000 0.000
#&gt; GSM11312     1  0.0000      0.998 1.000 0.000
#&gt; GSM28778     1  0.0000      0.998 1.000 0.000
#&gt; GSM28796     1  0.0000      0.998 1.000 0.000
#&gt; GSM11309     1  0.0000      0.998 1.000 0.000
#&gt; GSM11315     1  0.0000      0.998 1.000 0.000
#&gt; GSM11306     1  0.0000      0.998 1.000 0.000
#&gt; GSM28776     1  0.0000      0.998 1.000 0.000
#&gt; GSM28777     2  0.0672      1.000 0.008 0.992
#&gt; GSM11316     2  0.0672      1.000 0.008 0.992
#&gt; GSM11320     2  0.0672      1.000 0.008 0.992
#&gt; GSM28797     1  0.0000      0.998 1.000 0.000
#&gt; GSM28786     1  0.0000      0.998 1.000 0.000
#&gt; GSM28800     1  0.0000      0.998 1.000 0.000
#&gt; GSM11310     1  0.0000      0.998 1.000 0.000
#&gt; GSM28787     2  0.0672      1.000 0.008 0.992
#&gt; GSM11304     1  0.0000      0.998 1.000 0.000
#&gt; GSM11303     2  0.0672      1.000 0.008 0.992
#&gt; GSM11317     2  0.0672      1.000 0.008 0.992
#&gt; GSM11311     1  0.0000      0.998 1.000 0.000
#&gt; GSM28799     1  0.0000      0.998 1.000 0.000
#&gt; GSM28791     1  0.0000      0.998 1.000 0.000
#&gt; GSM28794     1  0.0376      0.996 0.996 0.004
#&gt; GSM28780     1  0.0000      0.998 1.000 0.000
#&gt; GSM28795     1  0.0000      0.998 1.000 0.000
#&gt; GSM11301     1  0.0672      0.994 0.992 0.008
#&gt; GSM11297     1  0.0000      0.998 1.000 0.000
#&gt; GSM11298     1  0.0000      0.998 1.000 0.000
#&gt; GSM11314     1  0.0000      0.998 1.000 0.000
#&gt; GSM11299     2  0.0672      1.000 0.008 0.992
#&gt; GSM28783     1  0.0000      0.998 1.000 0.000
#&gt; GSM11308     1  0.0000      0.998 1.000 0.000
#&gt; GSM28782     1  0.0000      0.998 1.000 0.000
#&gt; GSM28779     1  0.0000      0.998 1.000 0.000
#&gt; GSM11302     1  0.0000      0.998 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11300     3  0.0424      0.990 0.008 0.000 0.992
#&gt; GSM28798     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM28801     2  0.4555      0.772 0.200 0.800 0.000
#&gt; GSM11319     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM28784     2  0.4555      0.772 0.200 0.800 0.000
#&gt; GSM11307     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.883 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28786     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28800     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM11304     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11303     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28794     2  0.5327      0.681 0.272 0.728 0.000
#&gt; GSM28780     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11301     2  0.4555      0.772 0.200 0.800 0.000
#&gt; GSM11297     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11299     3  0.0424      0.990 0.008 0.000 0.992
#&gt; GSM28783     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      1.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM28789     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM28790     1  0.0188      0.982 0.996 0.000 0.000 0.004
#&gt; GSM11300     3  0.0336      0.992 0.000 0.000 0.992 0.008
#&gt; GSM28798     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.3688      0.794 0.000 0.792 0.000 0.208
#&gt; GSM11319     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.3688      0.794 0.000 0.792 0.000 0.208
#&gt; GSM11307     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.909 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11312     1  0.0188      0.982 0.996 0.000 0.000 0.004
#&gt; GSM28778     1  0.3610      0.763 0.800 0.000 0.000 0.200
#&gt; GSM28796     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11309     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11315     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.0817      0.966 0.976 0.000 0.000 0.024
#&gt; GSM28776     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM28797     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28786     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM28800     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11304     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM11303     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000      0.998 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.0188      0.982 0.996 0.000 0.000 0.004
#&gt; GSM28799     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28791     1  0.0188      0.982 0.996 0.000 0.000 0.004
#&gt; GSM28794     2  0.5458      0.680 0.076 0.720 0.000 0.204
#&gt; GSM28780     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28795     1  0.3266      0.802 0.832 0.000 0.000 0.168
#&gt; GSM11301     2  0.3688      0.794 0.000 0.792 0.000 0.208
#&gt; GSM11297     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM11298     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11314     4  0.0469      0.000 0.012 0.000 0.000 0.988
#&gt; GSM11299     3  0.0336      0.992 0.000 0.000 0.992 0.008
#&gt; GSM28783     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM11308     1  0.0336      0.980 0.992 0.000 0.000 0.008
#&gt; GSM28782     1  0.0000      0.984 1.000 0.000 0.000 0.000
#&gt; GSM28779     1  0.0188      0.982 0.996 0.000 0.000 0.004
#&gt; GSM11302     1  0.0188      0.982 0.996 0.000 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4 p5
#&gt; GSM28788     1  0.0290      0.974 0.992 0.008 0.000 0.000  0
#&gt; GSM28789     1  0.0290      0.974 0.992 0.008 0.000 0.000  0
#&gt; GSM28790     1  0.0162      0.976 0.996 0.004 0.000 0.000  0
#&gt; GSM11300     4  0.3612      0.627 0.000 0.000 0.268 0.732  0
#&gt; GSM28798     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM11296     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM28801     2  0.0000      0.632 0.000 1.000 0.000 0.000  0
#&gt; GSM11319     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM28781     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM11305     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM28784     2  0.0000      0.632 0.000 1.000 0.000 0.000  0
#&gt; GSM11307     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM11313     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM28785     2  0.4171      0.846 0.000 0.604 0.396 0.000  0
#&gt; GSM11318     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28792     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11295     3  0.4300      0.551 0.000 0.000 0.524 0.476  0
#&gt; GSM28793     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11312     1  0.0162      0.976 0.996 0.004 0.000 0.000  0
#&gt; GSM28778     1  0.4428      0.620 0.700 0.032 0.000 0.268  0
#&gt; GSM28796     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11309     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11315     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11306     1  0.0703      0.960 0.976 0.024 0.000 0.000  0
#&gt; GSM28776     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28777     3  0.4171      0.933 0.000 0.000 0.604 0.396  0
#&gt; GSM11316     3  0.4171      0.933 0.000 0.000 0.604 0.396  0
#&gt; GSM11320     3  0.4171      0.933 0.000 0.000 0.604 0.396  0
#&gt; GSM28797     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28786     1  0.0290      0.973 0.992 0.000 0.000 0.008  0
#&gt; GSM28800     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11310     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28787     4  0.4305     -0.555 0.000 0.000 0.488 0.512  0
#&gt; GSM11304     1  0.0290      0.973 0.992 0.000 0.000 0.008  0
#&gt; GSM11303     3  0.4171      0.933 0.000 0.000 0.604 0.396  0
#&gt; GSM11317     3  0.4171      0.933 0.000 0.000 0.604 0.396  0
#&gt; GSM11311     1  0.0162      0.976 0.996 0.000 0.000 0.004  0
#&gt; GSM28799     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28791     1  0.0162      0.976 0.996 0.004 0.000 0.000  0
#&gt; GSM28794     2  0.1671      0.579 0.076 0.924 0.000 0.000  0
#&gt; GSM28780     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28795     1  0.3612      0.665 0.732 0.000 0.000 0.268  0
#&gt; GSM11301     2  0.0000      0.632 0.000 1.000 0.000 0.000  0
#&gt; GSM11297     1  0.0290      0.973 0.992 0.000 0.000 0.008  0
#&gt; GSM11298     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11314     5  0.0000      0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM11299     4  0.3612      0.627 0.000 0.000 0.268 0.732  0
#&gt; GSM28783     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM11308     1  0.0290      0.973 0.992 0.000 0.000 0.008  0
#&gt; GSM28782     1  0.0000      0.977 1.000 0.000 0.000 0.000  0
#&gt; GSM28779     1  0.0162      0.976 0.996 0.004 0.000 0.000  0
#&gt; GSM11302     1  0.0162      0.976 0.996 0.004 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM28788     1  0.0260      0.987 0.992 0.008 0.000 0.000 0.000  0
#&gt; GSM28789     1  0.0260      0.987 0.992 0.008 0.000 0.000 0.000  0
#&gt; GSM28790     1  0.0146      0.992 0.996 0.004 0.000 0.000 0.000  0
#&gt; GSM11300     4  0.0000      0.654 0.000 0.000 0.000 1.000 0.000  0
#&gt; GSM28798     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM11296     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM28801     2  0.0000      0.605 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM11319     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM28781     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM11305     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM28784     2  0.0000      0.605 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM11307     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM11313     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM28785     2  0.3797      0.836 0.000 0.580 0.000 0.000 0.420  0
#&gt; GSM11318     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28792     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11295     3  0.2823      0.673 0.000 0.000 0.796 0.204 0.000  0
#&gt; GSM28793     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11312     1  0.0146      0.992 0.996 0.004 0.000 0.000 0.000  0
#&gt; GSM28778     5  0.4445      0.946 0.396 0.032 0.000 0.000 0.572  0
#&gt; GSM28796     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11309     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11315     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11306     1  0.0632      0.961 0.976 0.024 0.000 0.000 0.000  0
#&gt; GSM28776     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28777     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM11316     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM11320     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM28797     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28786     1  0.0260      0.987 0.992 0.000 0.000 0.008 0.000  0
#&gt; GSM28800     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11310     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28787     4  0.3804      0.180 0.000 0.000 0.424 0.576 0.000  0
#&gt; GSM11304     1  0.0260      0.987 0.992 0.000 0.000 0.008 0.000  0
#&gt; GSM11303     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM11317     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000  0
#&gt; GSM11311     1  0.0146      0.991 0.996 0.000 0.000 0.004 0.000  0
#&gt; GSM28799     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28791     1  0.0146      0.992 0.996 0.004 0.000 0.000 0.000  0
#&gt; GSM28794     2  0.1501      0.559 0.076 0.924 0.000 0.000 0.000  0
#&gt; GSM28780     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28795     5  0.3797      0.948 0.420 0.000 0.000 0.000 0.580  0
#&gt; GSM11301     2  0.0000      0.605 0.000 1.000 0.000 0.000 0.000  0
#&gt; GSM11297     1  0.0260      0.987 0.992 0.000 0.000 0.008 0.000  0
#&gt; GSM11298     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11314     6  0.0000      0.000 0.000 0.000 0.000 0.000 0.000  1
#&gt; GSM11299     4  0.0000      0.654 0.000 0.000 0.000 1.000 0.000  0
#&gt; GSM28783     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM11308     1  0.0260      0.987 0.992 0.000 0.000 0.008 0.000  0
#&gt; GSM28782     1  0.0000      0.994 1.000 0.000 0.000 0.000 0.000  0
#&gt; GSM28779     1  0.0146      0.992 0.996 0.004 0.000 0.000 0.000  0
#&gt; GSM11302     1  0.0146      0.992 0.996 0.004 0.000 0.000 0.000  0
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:hclust 52     0.396 2
#> ATC:hclust 52     0.372 3
#> ATC:hclust 51     0.371 4
#> ATC:hclust 50     0.349 5
#> ATC:hclust 50     0.331 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.514           0.747       0.798         0.3313 0.660   0.660
#> 3 3 1.000           0.981       0.993         0.6252 0.738   0.621
#> 4 4 0.684           0.704       0.861         0.2173 0.953   0.899
#> 5 5 0.630           0.619       0.778         0.1189 0.851   0.651
#> 6 6 0.665           0.640       0.782         0.0622 0.899   0.659
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.800 1.000 0.000
#&gt; GSM28789     1  0.0000      0.800 1.000 0.000
#&gt; GSM28790     1  0.0000      0.800 1.000 0.000
#&gt; GSM11300     1  0.9881      0.440 0.564 0.436
#&gt; GSM28798     2  0.9881      0.998 0.436 0.564
#&gt; GSM11296     2  0.9881      0.998 0.436 0.564
#&gt; GSM28801     1  0.9970     -0.804 0.532 0.468
#&gt; GSM11319     2  0.9881      0.998 0.436 0.564
#&gt; GSM28781     2  0.9881      0.998 0.436 0.564
#&gt; GSM11305     2  0.9881      0.998 0.436 0.564
#&gt; GSM28784     2  0.9881      0.998 0.436 0.564
#&gt; GSM11307     2  0.9881      0.998 0.436 0.564
#&gt; GSM11313     2  0.9881      0.998 0.436 0.564
#&gt; GSM28785     2  0.9881      0.998 0.436 0.564
#&gt; GSM11318     1  0.0000      0.800 1.000 0.000
#&gt; GSM28792     1  0.0000      0.800 1.000 0.000
#&gt; GSM11295     1  0.9922      0.432 0.552 0.448
#&gt; GSM28793     1  0.0000      0.800 1.000 0.000
#&gt; GSM11312     1  0.0000      0.800 1.000 0.000
#&gt; GSM28778     1  0.0000      0.800 1.000 0.000
#&gt; GSM28796     1  0.0000      0.800 1.000 0.000
#&gt; GSM11309     1  0.0000      0.800 1.000 0.000
#&gt; GSM11315     1  0.0000      0.800 1.000 0.000
#&gt; GSM11306     1  0.0000      0.800 1.000 0.000
#&gt; GSM28776     1  0.0000      0.800 1.000 0.000
#&gt; GSM28777     1  0.9922      0.432 0.552 0.448
#&gt; GSM11316     1  0.9922      0.432 0.552 0.448
#&gt; GSM11320     1  0.9922      0.432 0.552 0.448
#&gt; GSM28797     1  0.0000      0.800 1.000 0.000
#&gt; GSM28786     1  0.0000      0.800 1.000 0.000
#&gt; GSM28800     1  0.0000      0.800 1.000 0.000
#&gt; GSM11310     1  0.0000      0.800 1.000 0.000
#&gt; GSM28787     1  0.9922      0.432 0.552 0.448
#&gt; GSM11304     1  0.0000      0.800 1.000 0.000
#&gt; GSM11303     1  0.9922      0.432 0.552 0.448
#&gt; GSM11317     1  0.9922      0.432 0.552 0.448
#&gt; GSM11311     1  0.0000      0.800 1.000 0.000
#&gt; GSM28799     1  0.0000      0.800 1.000 0.000
#&gt; GSM28791     1  0.0000      0.800 1.000 0.000
#&gt; GSM28794     2  0.9922      0.978 0.448 0.552
#&gt; GSM28780     1  0.0000      0.800 1.000 0.000
#&gt; GSM28795     1  0.0000      0.800 1.000 0.000
#&gt; GSM11301     2  0.9881      0.998 0.436 0.564
#&gt; GSM11297     1  0.0000      0.800 1.000 0.000
#&gt; GSM11298     1  0.0000      0.800 1.000 0.000
#&gt; GSM11314     1  0.0938      0.784 0.988 0.012
#&gt; GSM11299     1  0.9881      0.440 0.564 0.436
#&gt; GSM28783     1  0.0000      0.800 1.000 0.000
#&gt; GSM11308     1  0.0000      0.800 1.000 0.000
#&gt; GSM28782     1  0.0000      0.800 1.000 0.000
#&gt; GSM28779     1  0.0000      0.800 1.000 0.000
#&gt; GSM11302     1  0.0000      0.800 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28789     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28790     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11300     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28798     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11296     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM28801     2   0.470      0.657 0.212 0.788 0.000
#&gt; GSM11319     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM28781     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11305     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM28784     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11307     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11313     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM28785     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11318     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28792     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11295     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28793     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11312     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28778     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28796     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11309     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11315     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11306     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28776     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28777     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11316     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11320     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM28797     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28786     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28800     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11310     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28787     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11304     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11303     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11317     3   0.000      1.000 0.000 0.000 1.000
#&gt; GSM11311     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28799     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28791     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28794     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28780     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28795     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11301     2   0.000      0.968 0.000 1.000 0.000
#&gt; GSM11297     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11298     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11314     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11299     1   0.362      0.843 0.864 0.000 0.136
#&gt; GSM28783     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11308     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28782     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM28779     1   0.000      0.996 1.000 0.000 0.000
#&gt; GSM11302     1   0.000      0.996 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.3528      0.688 0.808 0.000 0.000 0.192
#&gt; GSM28789     1  0.4008      0.613 0.756 0.000 0.000 0.244
#&gt; GSM28790     1  0.2760      0.743 0.872 0.000 0.000 0.128
#&gt; GSM11300     3  0.4454      0.660 0.000 0.000 0.692 0.308
#&gt; GSM28798     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.5483      0.462 0.016 0.536 0.000 0.448
#&gt; GSM11319     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.4955      0.503 0.000 0.556 0.000 0.444
#&gt; GSM11307     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.2469      0.718 0.892 0.000 0.000 0.108
#&gt; GSM28792     1  0.2469      0.718 0.892 0.000 0.000 0.108
#&gt; GSM11295     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.1389      0.749 0.952 0.000 0.000 0.048
#&gt; GSM11312     1  0.2760      0.743 0.872 0.000 0.000 0.128
#&gt; GSM28778     1  0.4907      0.106 0.580 0.000 0.000 0.420
#&gt; GSM28796     1  0.0336      0.762 0.992 0.000 0.000 0.008
#&gt; GSM11309     1  0.3311      0.712 0.828 0.000 0.000 0.172
#&gt; GSM11315     1  0.1557      0.761 0.944 0.000 0.000 0.056
#&gt; GSM11306     1  0.3610      0.679 0.800 0.000 0.000 0.200
#&gt; GSM28776     1  0.1557      0.747 0.944 0.000 0.000 0.056
#&gt; GSM28777     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM28797     1  0.3311      0.731 0.828 0.000 0.000 0.172
#&gt; GSM28786     1  0.4877      0.242 0.592 0.000 0.000 0.408
#&gt; GSM28800     1  0.1474      0.761 0.948 0.000 0.000 0.052
#&gt; GSM11310     1  0.0817      0.764 0.976 0.000 0.000 0.024
#&gt; GSM28787     3  0.1867      0.906 0.000 0.000 0.928 0.072
#&gt; GSM11304     1  0.4164      0.522 0.736 0.000 0.000 0.264
#&gt; GSM11303     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000      0.949 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.2408      0.718 0.896 0.000 0.000 0.104
#&gt; GSM28799     1  0.2469      0.718 0.892 0.000 0.000 0.108
#&gt; GSM28791     1  0.2704      0.743 0.876 0.000 0.000 0.124
#&gt; GSM28794     4  0.4877      0.150 0.408 0.000 0.000 0.592
#&gt; GSM28780     1  0.2704      0.743 0.876 0.000 0.000 0.124
#&gt; GSM28795     1  0.4643      0.421 0.656 0.000 0.000 0.344
#&gt; GSM11301     2  0.4040      0.726 0.000 0.752 0.000 0.248
#&gt; GSM11297     1  0.4164      0.522 0.736 0.000 0.000 0.264
#&gt; GSM11298     1  0.0000      0.763 1.000 0.000 0.000 0.000
#&gt; GSM11314     4  0.3172      0.315 0.160 0.000 0.000 0.840
#&gt; GSM11299     1  0.6079      0.152 0.568 0.000 0.052 0.380
#&gt; GSM28783     1  0.2469      0.757 0.892 0.000 0.000 0.108
#&gt; GSM11308     1  0.4164      0.522 0.736 0.000 0.000 0.264
#&gt; GSM28782     1  0.2704      0.743 0.876 0.000 0.000 0.124
#&gt; GSM28779     1  0.1474      0.761 0.948 0.000 0.000 0.052
#&gt; GSM11302     1  0.2704      0.743 0.876 0.000 0.000 0.124
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1  0.4119     0.5923 0.752 0.000 0.000 0.212 0.036
#&gt; GSM28789     1  0.4325     0.5712 0.736 0.000 0.000 0.220 0.044
#&gt; GSM28790     1  0.1043     0.7092 0.960 0.000 0.000 0.040 0.000
#&gt; GSM11300     5  0.5159    -0.1961 0.000 0.000 0.400 0.044 0.556
#&gt; GSM28798     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     4  0.4555     0.2619 0.020 0.344 0.000 0.636 0.000
#&gt; GSM11319     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     4  0.4722     0.2218 0.024 0.368 0.000 0.608 0.000
#&gt; GSM11307     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9288 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4956     0.4450 0.636 0.000 0.000 0.048 0.316
#&gt; GSM28792     1  0.5027     0.4476 0.640 0.000 0.000 0.056 0.304
#&gt; GSM11295     3  0.0963     0.9543 0.000 0.000 0.964 0.036 0.000
#&gt; GSM28793     1  0.3863     0.5835 0.740 0.000 0.000 0.012 0.248
#&gt; GSM11312     1  0.1965     0.7001 0.904 0.000 0.000 0.096 0.000
#&gt; GSM28778     1  0.6036    -0.0693 0.452 0.000 0.000 0.432 0.116
#&gt; GSM28796     1  0.3630     0.6276 0.780 0.000 0.000 0.016 0.204
#&gt; GSM11309     1  0.4302     0.6066 0.744 0.000 0.000 0.208 0.048
#&gt; GSM11315     1  0.2046     0.6977 0.916 0.000 0.000 0.016 0.068
#&gt; GSM11306     1  0.4134     0.5995 0.760 0.000 0.000 0.196 0.044
#&gt; GSM28776     1  0.3819     0.6031 0.756 0.000 0.000 0.016 0.228
#&gt; GSM28777     3  0.0963     0.9543 0.000 0.000 0.964 0.036 0.000
#&gt; GSM11316     3  0.0000     0.9619 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9619 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     1  0.6083     0.5216 0.572 0.000 0.000 0.204 0.224
#&gt; GSM28786     5  0.2612     0.6057 0.124 0.000 0.000 0.008 0.868
#&gt; GSM28800     1  0.1877     0.7009 0.924 0.000 0.000 0.012 0.064
#&gt; GSM11310     1  0.1952     0.6979 0.912 0.000 0.000 0.004 0.084
#&gt; GSM28787     3  0.3966     0.8155 0.000 0.000 0.796 0.072 0.132
#&gt; GSM11304     5  0.3913     0.5325 0.324 0.000 0.000 0.000 0.676
#&gt; GSM11303     3  0.0000     0.9619 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9619 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     1  0.4306     0.4660 0.660 0.000 0.000 0.012 0.328
#&gt; GSM28799     1  0.5027     0.4476 0.640 0.000 0.000 0.056 0.304
#&gt; GSM28791     1  0.2020     0.6988 0.900 0.000 0.000 0.100 0.000
#&gt; GSM28794     4  0.3074     0.3785 0.196 0.000 0.000 0.804 0.000
#&gt; GSM28780     1  0.2020     0.6988 0.900 0.000 0.000 0.100 0.000
#&gt; GSM28795     4  0.6627    -0.0727 0.296 0.000 0.000 0.452 0.252
#&gt; GSM11301     2  0.4294     0.0104 0.000 0.532 0.000 0.468 0.000
#&gt; GSM11297     5  0.3913     0.5325 0.324 0.000 0.000 0.000 0.676
#&gt; GSM11298     1  0.2966     0.6506 0.816 0.000 0.000 0.000 0.184
#&gt; GSM11314     5  0.4383    -0.0104 0.004 0.000 0.000 0.424 0.572
#&gt; GSM11299     5  0.3355     0.6002 0.132 0.000 0.000 0.036 0.832
#&gt; GSM28783     1  0.4430     0.6596 0.752 0.000 0.000 0.076 0.172
#&gt; GSM11308     5  0.3895     0.5353 0.320 0.000 0.000 0.000 0.680
#&gt; GSM28782     1  0.2179     0.6996 0.896 0.000 0.000 0.100 0.004
#&gt; GSM28779     1  0.1942     0.7003 0.920 0.000 0.000 0.012 0.068
#&gt; GSM11302     1  0.0771     0.7107 0.976 0.000 0.000 0.020 0.004
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.3428     0.6148 0.304 0.000 0.000 0.696 0.000 0.000
#&gt; GSM28789     4  0.3409     0.6187 0.300 0.000 0.000 0.700 0.000 0.000
#&gt; GSM28790     1  0.3595     0.4987 0.704 0.000 0.000 0.288 0.008 0.000
#&gt; GSM11300     6  0.3780     0.2088 0.000 0.000 0.248 0.004 0.020 0.728
#&gt; GSM28798     2  0.0603     0.9852 0.000 0.980 0.000 0.016 0.000 0.004
#&gt; GSM11296     2  0.0000     0.9912 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     5  0.2442     0.8075 0.000 0.144 0.000 0.004 0.852 0.000
#&gt; GSM11319     2  0.0603     0.9852 0.000 0.980 0.000 0.016 0.000 0.004
#&gt; GSM28781     2  0.0603     0.9852 0.000 0.980 0.000 0.016 0.000 0.004
#&gt; GSM11305     2  0.0000     0.9912 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     5  0.2854     0.8064 0.000 0.208 0.000 0.000 0.792 0.000
#&gt; GSM11307     2  0.0000     0.9912 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9912 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9912 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.3432     0.5235 0.836 0.000 0.000 0.044 0.036 0.084
#&gt; GSM28792     1  0.3581     0.5142 0.824 0.000 0.000 0.044 0.036 0.096
#&gt; GSM11295     3  0.2066     0.9115 0.000 0.000 0.908 0.052 0.040 0.000
#&gt; GSM28793     1  0.0458     0.6524 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM11312     1  0.4101     0.2518 0.580 0.000 0.000 0.408 0.012 0.000
#&gt; GSM28778     4  0.5176     0.3782 0.072 0.000 0.000 0.696 0.156 0.076
#&gt; GSM28796     1  0.0405     0.6617 0.988 0.000 0.000 0.008 0.004 0.000
#&gt; GSM11309     4  0.4053     0.6162 0.300 0.000 0.000 0.676 0.020 0.004
#&gt; GSM11315     1  0.2442     0.6510 0.852 0.000 0.000 0.144 0.004 0.000
#&gt; GSM11306     4  0.3672     0.6082 0.304 0.000 0.000 0.688 0.008 0.000
#&gt; GSM28776     1  0.0820     0.6592 0.972 0.000 0.000 0.016 0.000 0.012
#&gt; GSM28777     3  0.2066     0.9115 0.000 0.000 0.908 0.052 0.040 0.000
#&gt; GSM11316     3  0.0000     0.9318 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9318 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     4  0.4972     0.4388 0.424 0.000 0.000 0.524 0.020 0.032
#&gt; GSM28786     6  0.3936     0.6307 0.228 0.000 0.000 0.024 0.012 0.736
#&gt; GSM28800     1  0.2491     0.6461 0.836 0.000 0.000 0.164 0.000 0.000
#&gt; GSM11310     1  0.2378     0.6542 0.848 0.000 0.000 0.152 0.000 0.000
#&gt; GSM28787     3  0.4946     0.7300 0.000 0.000 0.692 0.056 0.048 0.204
#&gt; GSM11304     6  0.4093     0.5930 0.440 0.000 0.000 0.004 0.004 0.552
#&gt; GSM11303     3  0.0000     0.9318 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3  0.0000     0.9318 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     1  0.1970     0.6066 0.900 0.000 0.000 0.008 0.000 0.092
#&gt; GSM28799     1  0.3559     0.5213 0.828 0.000 0.000 0.052 0.036 0.084
#&gt; GSM28791     1  0.4322     0.1061 0.528 0.000 0.000 0.452 0.020 0.000
#&gt; GSM28794     5  0.3416     0.6281 0.056 0.000 0.000 0.140 0.804 0.000
#&gt; GSM28780     1  0.4456     0.0803 0.520 0.000 0.000 0.456 0.020 0.004
#&gt; GSM28795     4  0.6485     0.2188 0.164 0.000 0.000 0.548 0.200 0.088
#&gt; GSM11301     5  0.3707     0.6865 0.000 0.312 0.000 0.008 0.680 0.000
#&gt; GSM11297     6  0.4098     0.5884 0.444 0.000 0.000 0.004 0.004 0.548
#&gt; GSM11298     1  0.1080     0.6658 0.960 0.000 0.000 0.032 0.004 0.004
#&gt; GSM11314     6  0.6823    -0.0744 0.060 0.000 0.000 0.204 0.320 0.416
#&gt; GSM11299     6  0.3087     0.5973 0.176 0.000 0.000 0.004 0.012 0.808
#&gt; GSM28783     1  0.3030     0.5521 0.816 0.000 0.000 0.168 0.008 0.008
#&gt; GSM11308     6  0.4093     0.5930 0.440 0.000 0.000 0.004 0.004 0.552
#&gt; GSM28782     1  0.4456     0.0803 0.520 0.000 0.000 0.456 0.020 0.004
#&gt; GSM28779     1  0.2520     0.6532 0.844 0.000 0.000 0.152 0.004 0.000
#&gt; GSM11302     1  0.3265     0.5597 0.748 0.000 0.000 0.248 0.004 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:kmeans 42     0.384 2
#> ATC:kmeans 52     0.372 3
#> ATC:kmeans 45     0.363 4
#> ATC:kmeans 40     0.332 5
#> ATC:kmeans 42     0.423 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.933       0.974         0.4388 0.581   0.581
#> 3 3 0.912           0.923       0.967         0.4581 0.736   0.567
#> 4 4 0.807           0.778       0.904         0.1717 0.833   0.571
#> 5 5 0.764           0.690       0.806         0.0592 0.889   0.593
#> 6 6 0.782           0.557       0.773         0.0342 0.914   0.637
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.0000      0.963 1.000 0.000
#&gt; GSM28789     2  0.1184      0.983 0.016 0.984
#&gt; GSM28790     1  0.9358      0.469 0.648 0.352
#&gt; GSM11300     1  0.0000      0.963 1.000 0.000
#&gt; GSM28798     2  0.0000      0.999 0.000 1.000
#&gt; GSM11296     2  0.0000      0.999 0.000 1.000
#&gt; GSM28801     2  0.0000      0.999 0.000 1.000
#&gt; GSM11319     2  0.0000      0.999 0.000 1.000
#&gt; GSM28781     2  0.0000      0.999 0.000 1.000
#&gt; GSM11305     2  0.0000      0.999 0.000 1.000
#&gt; GSM28784     2  0.0000      0.999 0.000 1.000
#&gt; GSM11307     2  0.0000      0.999 0.000 1.000
#&gt; GSM11313     2  0.0000      0.999 0.000 1.000
#&gt; GSM28785     2  0.0000      0.999 0.000 1.000
#&gt; GSM11318     1  0.0000      0.963 1.000 0.000
#&gt; GSM28792     1  0.0000      0.963 1.000 0.000
#&gt; GSM11295     1  0.0000      0.963 1.000 0.000
#&gt; GSM28793     1  0.0000      0.963 1.000 0.000
#&gt; GSM11312     1  0.0376      0.960 0.996 0.004
#&gt; GSM28778     2  0.0000      0.999 0.000 1.000
#&gt; GSM28796     1  0.0000      0.963 1.000 0.000
#&gt; GSM11309     1  0.0000      0.963 1.000 0.000
#&gt; GSM11315     1  0.1633      0.943 0.976 0.024
#&gt; GSM11306     1  0.9988      0.116 0.520 0.480
#&gt; GSM28776     1  0.0000      0.963 1.000 0.000
#&gt; GSM28777     1  0.0000      0.963 1.000 0.000
#&gt; GSM11316     1  0.0000      0.963 1.000 0.000
#&gt; GSM11320     1  0.0000      0.963 1.000 0.000
#&gt; GSM28797     1  0.0000      0.963 1.000 0.000
#&gt; GSM28786     1  0.0000      0.963 1.000 0.000
#&gt; GSM28800     1  0.0000      0.963 1.000 0.000
#&gt; GSM11310     1  0.0000      0.963 1.000 0.000
#&gt; GSM28787     1  0.0000      0.963 1.000 0.000
#&gt; GSM11304     1  0.0000      0.963 1.000 0.000
#&gt; GSM11303     1  0.0000      0.963 1.000 0.000
#&gt; GSM11317     1  0.0000      0.963 1.000 0.000
#&gt; GSM11311     1  0.0000      0.963 1.000 0.000
#&gt; GSM28799     1  0.0000      0.963 1.000 0.000
#&gt; GSM28791     1  0.1633      0.943 0.976 0.024
#&gt; GSM28794     2  0.0000      0.999 0.000 1.000
#&gt; GSM28780     1  0.0000      0.963 1.000 0.000
#&gt; GSM28795     1  0.9850      0.263 0.572 0.428
#&gt; GSM11301     2  0.0000      0.999 0.000 1.000
#&gt; GSM11297     1  0.0000      0.963 1.000 0.000
#&gt; GSM11298     1  0.0000      0.963 1.000 0.000
#&gt; GSM11314     2  0.0000      0.999 0.000 1.000
#&gt; GSM11299     1  0.0000      0.963 1.000 0.000
#&gt; GSM28783     1  0.0000      0.963 1.000 0.000
#&gt; GSM11308     1  0.0000      0.963 1.000 0.000
#&gt; GSM28782     1  0.0000      0.963 1.000 0.000
#&gt; GSM28779     1  0.0000      0.963 1.000 0.000
#&gt; GSM11302     1  0.0000      0.963 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28789     1  0.4399      0.762 0.812 0.188 0.000
#&gt; GSM28790     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11318     1  0.4555      0.761 0.800 0.000 0.200
#&gt; GSM28792     1  0.4555      0.761 0.800 0.000 0.200
#&gt; GSM11295     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28778     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28796     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28786     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM28800     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM11304     1  0.6079      0.440 0.612 0.000 0.388
#&gt; GSM11303     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM11311     1  0.1163      0.923 0.972 0.000 0.028
#&gt; GSM28799     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28794     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM28780     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28795     3  0.0892      0.956 0.020 0.000 0.980
#&gt; GSM11301     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM11297     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11314     3  0.4931      0.677 0.000 0.232 0.768
#&gt; GSM11299     3  0.0000      0.975 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11308     1  0.6305      0.174 0.516 0.000 0.484
#&gt; GSM28782     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      0.942 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.942 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     4  0.0707     0.6955 0.020 0.000 0.000 0.980
#&gt; GSM28789     4  0.1174     0.6935 0.020 0.012 0.000 0.968
#&gt; GSM28790     4  0.4998     0.0466 0.488 0.000 0.000 0.512
#&gt; GSM11300     3  0.0469     0.9701 0.000 0.000 0.988 0.012
#&gt; GSM28798     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.0000     0.8349 1.000 0.000 0.000 0.000
#&gt; GSM28792     1  0.0188     0.8349 0.996 0.000 0.004 0.000
#&gt; GSM11295     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.0336     0.8359 0.992 0.000 0.000 0.008
#&gt; GSM11312     4  0.4103     0.6097 0.256 0.000 0.000 0.744
#&gt; GSM28778     4  0.3024     0.6147 0.000 0.148 0.000 0.852
#&gt; GSM28796     1  0.0469     0.8360 0.988 0.000 0.000 0.012
#&gt; GSM11309     4  0.0000     0.6901 0.000 0.000 0.000 1.000
#&gt; GSM11315     1  0.4040     0.6215 0.752 0.000 0.000 0.248
#&gt; GSM11306     4  0.0707     0.6955 0.020 0.000 0.000 0.980
#&gt; GSM28776     1  0.1211     0.8294 0.960 0.000 0.000 0.040
#&gt; GSM28777     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM28797     4  0.4164     0.4710 0.264 0.000 0.000 0.736
#&gt; GSM28786     3  0.1174     0.9573 0.012 0.000 0.968 0.020
#&gt; GSM28800     1  0.4454     0.5363 0.692 0.000 0.000 0.308
#&gt; GSM11310     1  0.4277     0.5838 0.720 0.000 0.000 0.280
#&gt; GSM28787     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM11304     1  0.1174     0.8247 0.968 0.000 0.012 0.020
#&gt; GSM11303     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9751 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.0817     0.8287 0.976 0.000 0.000 0.024
#&gt; GSM28799     1  0.1118     0.8275 0.964 0.000 0.000 0.036
#&gt; GSM28791     4  0.4008     0.6236 0.244 0.000 0.000 0.756
#&gt; GSM28794     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM28780     4  0.3907     0.6322 0.232 0.000 0.000 0.768
#&gt; GSM28795     4  0.6340     0.0563 0.064 0.000 0.408 0.528
#&gt; GSM11301     2  0.0000     1.0000 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.0921     0.8302 0.972 0.000 0.000 0.028
#&gt; GSM11298     1  0.1211     0.8289 0.960 0.000 0.000 0.040
#&gt; GSM11314     3  0.3636     0.7874 0.008 0.172 0.820 0.000
#&gt; GSM11299     3  0.0469     0.9701 0.000 0.000 0.988 0.012
#&gt; GSM28783     1  0.4907     0.0540 0.580 0.000 0.000 0.420
#&gt; GSM11308     1  0.1520     0.8171 0.956 0.000 0.024 0.020
#&gt; GSM28782     4  0.4008     0.6236 0.244 0.000 0.000 0.756
#&gt; GSM28779     1  0.4500     0.5174 0.684 0.000 0.000 0.316
#&gt; GSM11302     4  0.4996     0.0698 0.484 0.000 0.000 0.516
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4   0.413     0.7035 0.380 0.000 0.000 0.620 0.000
#&gt; GSM28789     4   0.413     0.7035 0.380 0.000 0.000 0.620 0.000
#&gt; GSM28790     1   0.347     0.6013 0.836 0.000 0.000 0.072 0.092
#&gt; GSM11300     3   0.223     0.8662 0.000 0.000 0.892 0.004 0.104
#&gt; GSM28798     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11307     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     5   0.530     0.6249 0.292 0.000 0.000 0.080 0.628
#&gt; GSM28792     5   0.581     0.6248 0.236 0.000 0.004 0.140 0.620
#&gt; GSM11295     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     5   0.425     0.5689 0.340 0.000 0.000 0.008 0.652
#&gt; GSM11312     1   0.213     0.5056 0.892 0.000 0.000 0.108 0.000
#&gt; GSM28778     4   0.424     0.5995 0.100 0.084 0.000 0.800 0.016
#&gt; GSM28796     5   0.471     0.3586 0.436 0.000 0.000 0.016 0.548
#&gt; GSM11309     4   0.526     0.6820 0.368 0.000 0.000 0.576 0.056
#&gt; GSM11315     1   0.482     0.1661 0.616 0.000 0.000 0.032 0.352
#&gt; GSM11306     4   0.417     0.6895 0.396 0.000 0.000 0.604 0.000
#&gt; GSM28776     1   0.437     0.0140 0.580 0.000 0.000 0.004 0.416
#&gt; GSM28777     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     4   0.606     0.5205 0.168 0.000 0.000 0.564 0.268
#&gt; GSM28786     3   0.467     0.6815 0.000 0.000 0.684 0.044 0.272
#&gt; GSM28800     1   0.345     0.5253 0.784 0.000 0.000 0.008 0.208
#&gt; GSM11310     1   0.337     0.5107 0.768 0.000 0.000 0.000 0.232
#&gt; GSM28787     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11304     5   0.284     0.6171 0.064 0.000 0.016 0.032 0.888
#&gt; GSM11303     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3   0.000     0.9171 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     5   0.376     0.6637 0.188 0.000 0.000 0.028 0.784
#&gt; GSM28799     5   0.609     0.5646 0.304 0.000 0.000 0.152 0.544
#&gt; GSM28791     1   0.289     0.4525 0.844 0.000 0.000 0.148 0.008
#&gt; GSM28794     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28780     1   0.410     0.2501 0.748 0.000 0.000 0.220 0.032
#&gt; GSM28795     4   0.479     0.4470 0.036 0.000 0.176 0.748 0.040
#&gt; GSM11301     2   0.000     1.0000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     5   0.253     0.6204 0.076 0.000 0.000 0.032 0.892
#&gt; GSM11298     1   0.450    -0.0249 0.568 0.000 0.000 0.008 0.424
#&gt; GSM11314     3   0.576     0.6179 0.000 0.080 0.664 0.220 0.036
#&gt; GSM11299     3   0.229     0.8637 0.000 0.000 0.888 0.004 0.108
#&gt; GSM28783     1   0.603     0.3498 0.572 0.000 0.000 0.172 0.256
#&gt; GSM11308     5   0.293     0.6067 0.048 0.000 0.032 0.032 0.888
#&gt; GSM28782     1   0.365     0.3709 0.796 0.000 0.000 0.176 0.028
#&gt; GSM28779     1   0.311     0.5294 0.800 0.000 0.000 0.000 0.200
#&gt; GSM11302     1   0.277     0.6093 0.876 0.000 0.000 0.032 0.092
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4   0.551   -0.06914 0.020 0.000 0.000 0.520 0.380 0.080
#&gt; GSM28789     4   0.552   -0.07338 0.020 0.000 0.000 0.516 0.384 0.080
#&gt; GSM28790     4   0.441    0.25253 0.320 0.000 0.000 0.644 0.024 0.012
#&gt; GSM11300     3   0.242    0.78356 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM28798     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11307     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1   0.238    0.55679 0.884 0.000 0.000 0.008 0.012 0.096
#&gt; GSM28792     1   0.280    0.48751 0.856 0.000 0.000 0.012 0.016 0.116
#&gt; GSM11295     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28793     1   0.552    0.43101 0.536 0.000 0.000 0.160 0.000 0.304
#&gt; GSM11312     4   0.340    0.44039 0.132 0.000 0.000 0.820 0.020 0.028
#&gt; GSM28778     5   0.169    0.82847 0.000 0.012 0.000 0.064 0.924 0.000
#&gt; GSM28796     1   0.514    0.56209 0.632 0.000 0.000 0.136 0.004 0.228
#&gt; GSM11309     4   0.636   -0.07820 0.036 0.000 0.000 0.472 0.324 0.168
#&gt; GSM11315     1   0.499    0.40959 0.628 0.000 0.000 0.292 0.016 0.064
#&gt; GSM11306     4   0.536    0.00941 0.024 0.000 0.000 0.568 0.340 0.068
#&gt; GSM28776     1   0.661    0.17628 0.420 0.000 0.000 0.316 0.036 0.228
#&gt; GSM28777     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     6   0.650   -0.12844 0.024 0.000 0.000 0.272 0.280 0.424
#&gt; GSM28786     3   0.561    0.35897 0.080 0.000 0.532 0.004 0.020 0.364
#&gt; GSM28800     4   0.609    0.00114 0.376 0.000 0.000 0.472 0.036 0.116
#&gt; GSM11310     4   0.628   -0.01975 0.356 0.000 0.000 0.456 0.032 0.156
#&gt; GSM28787     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11304     6   0.276    0.61873 0.084 0.000 0.020 0.024 0.000 0.872
#&gt; GSM11303     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11317     3   0.000    0.86909 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     6   0.510    0.03074 0.436 0.000 0.000 0.040 0.020 0.504
#&gt; GSM28799     1   0.215    0.53088 0.904 0.000 0.000 0.008 0.016 0.072
#&gt; GSM28791     4   0.255    0.46356 0.088 0.000 0.000 0.880 0.020 0.012
#&gt; GSM28794     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28780     4   0.271    0.46502 0.036 0.000 0.000 0.884 0.040 0.040
#&gt; GSM28795     5   0.274    0.82520 0.028 0.000 0.064 0.012 0.884 0.012
#&gt; GSM11301     2   0.000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11297     6   0.233    0.60703 0.080 0.000 0.000 0.032 0.000 0.888
#&gt; GSM11298     4   0.605   -0.23398 0.328 0.000 0.000 0.404 0.000 0.268
#&gt; GSM11314     3   0.662    0.28691 0.112 0.068 0.544 0.000 0.256 0.020
#&gt; GSM11299     3   0.253    0.77549 0.000 0.000 0.832 0.000 0.000 0.168
#&gt; GSM28783     4   0.649    0.14812 0.120 0.000 0.000 0.492 0.076 0.312
#&gt; GSM11308     6   0.282    0.61052 0.068 0.000 0.044 0.016 0.000 0.872
#&gt; GSM28782     4   0.288    0.46375 0.056 0.000 0.000 0.872 0.024 0.048
#&gt; GSM28779     4   0.584    0.06669 0.360 0.000 0.000 0.512 0.032 0.096
#&gt; GSM11302     4   0.438    0.29540 0.268 0.000 0.000 0.684 0.012 0.036
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n tissue(p) k
#> ATC:skmeans 49     0.393 2
#> ATC:skmeans 50     0.370 3
#> ATC:skmeans 47     0.440 4
#> ATC:skmeans 43     0.400 5
#> ATC:skmeans 29     0.379 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.979       0.992         0.3314 0.660   0.660
#> 3 3 1.000           0.959       0.987         0.5908 0.801   0.698
#> 4 4 0.665           0.692       0.815         0.2014 0.940   0.873
#> 5 5 0.708           0.766       0.889         0.0812 0.894   0.756
#> 6 6 0.727           0.722       0.876         0.0254 0.971   0.913
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette  p1  p2
#&gt; GSM28788     1   0.000      1.000 1.0 0.0
#&gt; GSM28789     1   0.000      1.000 1.0 0.0
#&gt; GSM28790     1   0.000      1.000 1.0 0.0
#&gt; GSM11300     1   0.000      1.000 1.0 0.0
#&gt; GSM28798     2   0.000      0.960 0.0 1.0
#&gt; GSM11296     2   0.000      0.960 0.0 1.0
#&gt; GSM28801     2   0.971      0.333 0.4 0.6
#&gt; GSM11319     2   0.000      0.960 0.0 1.0
#&gt; GSM28781     2   0.000      0.960 0.0 1.0
#&gt; GSM11305     2   0.000      0.960 0.0 1.0
#&gt; GSM28784     2   0.000      0.960 0.0 1.0
#&gt; GSM11307     2   0.000      0.960 0.0 1.0
#&gt; GSM11313     2   0.000      0.960 0.0 1.0
#&gt; GSM28785     2   0.000      0.960 0.0 1.0
#&gt; GSM11318     1   0.000      1.000 1.0 0.0
#&gt; GSM28792     1   0.000      1.000 1.0 0.0
#&gt; GSM11295     1   0.000      1.000 1.0 0.0
#&gt; GSM28793     1   0.000      1.000 1.0 0.0
#&gt; GSM11312     1   0.000      1.000 1.0 0.0
#&gt; GSM28778     1   0.000      1.000 1.0 0.0
#&gt; GSM28796     1   0.000      1.000 1.0 0.0
#&gt; GSM11309     1   0.000      1.000 1.0 0.0
#&gt; GSM11315     1   0.000      1.000 1.0 0.0
#&gt; GSM11306     1   0.000      1.000 1.0 0.0
#&gt; GSM28776     1   0.000      1.000 1.0 0.0
#&gt; GSM28777     1   0.000      1.000 1.0 0.0
#&gt; GSM11316     1   0.000      1.000 1.0 0.0
#&gt; GSM11320     1   0.000      1.000 1.0 0.0
#&gt; GSM28797     1   0.000      1.000 1.0 0.0
#&gt; GSM28786     1   0.000      1.000 1.0 0.0
#&gt; GSM28800     1   0.000      1.000 1.0 0.0
#&gt; GSM11310     1   0.000      1.000 1.0 0.0
#&gt; GSM28787     1   0.000      1.000 1.0 0.0
#&gt; GSM11304     1   0.000      1.000 1.0 0.0
#&gt; GSM11303     1   0.000      1.000 1.0 0.0
#&gt; GSM11317     1   0.000      1.000 1.0 0.0
#&gt; GSM11311     1   0.000      1.000 1.0 0.0
#&gt; GSM28799     1   0.000      1.000 1.0 0.0
#&gt; GSM28791     1   0.000      1.000 1.0 0.0
#&gt; GSM28794     1   0.000      1.000 1.0 0.0
#&gt; GSM28780     1   0.000      1.000 1.0 0.0
#&gt; GSM28795     1   0.000      1.000 1.0 0.0
#&gt; GSM11301     2   0.000      0.960 0.0 1.0
#&gt; GSM11297     1   0.000      1.000 1.0 0.0
#&gt; GSM11298     1   0.000      1.000 1.0 0.0
#&gt; GSM11314     1   0.000      1.000 1.0 0.0
#&gt; GSM11299     1   0.000      1.000 1.0 0.0
#&gt; GSM28783     1   0.000      1.000 1.0 0.0
#&gt; GSM11308     1   0.000      1.000 1.0 0.0
#&gt; GSM28782     1   0.000      1.000 1.0 0.0
#&gt; GSM28779     1   0.000      1.000 1.0 0.0
#&gt; GSM11302     1   0.000      1.000 1.0 0.0
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11300     3  0.5254      0.585 0.264 0.000 0.736
#&gt; GSM28798     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM28801     2  0.6126      0.338 0.400 0.600 0.000
#&gt; GSM11319     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM28784     2  0.0747      0.916 0.016 0.984 0.000
#&gt; GSM11307     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28792     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11295     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28786     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28800     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11304     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11303     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000      0.943 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28794     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28780     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28795     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11301     2  0.0000      0.935 0.000 1.000 0.000
#&gt; GSM11297     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11314     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11299     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28783     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000      1.000 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000      1.000 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM28789     1  0.3528     0.6843 0.808 0.000 0.000 0.192
#&gt; GSM28790     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM11300     2  0.7830    -0.3171 0.000 0.400 0.268 0.332
#&gt; GSM28798     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM11296     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM28801     4  0.6537     0.7679 0.164 0.200 0.000 0.636
#&gt; GSM11319     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM28781     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM11305     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM28784     4  0.6537     0.7679 0.164 0.200 0.000 0.636
#&gt; GSM11307     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM11313     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM28785     2  0.4855     0.3919 0.000 0.600 0.000 0.400
#&gt; GSM11318     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM28792     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM11295     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM28793     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM11312     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM28778     1  0.3942     0.6258 0.764 0.000 0.000 0.236
#&gt; GSM28796     1  0.2921     0.8269 0.860 0.000 0.000 0.140
#&gt; GSM11309     1  0.1389     0.8490 0.952 0.000 0.000 0.048
#&gt; GSM11315     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.0188     0.8525 0.996 0.000 0.000 0.004
#&gt; GSM28776     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM28777     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM28797     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM28786     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM28800     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.3123     0.8216 0.844 0.000 0.000 0.156
#&gt; GSM28787     3  0.3610     0.8257 0.000 0.000 0.800 0.200
#&gt; GSM11304     1  0.7393     0.3672 0.436 0.400 0.000 0.164
#&gt; GSM11303     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM11317     3  0.0000     0.9727 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM28799     1  0.3219     0.8188 0.836 0.000 0.000 0.164
#&gt; GSM28791     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM28794     1  0.3942     0.6258 0.764 0.000 0.000 0.236
#&gt; GSM28780     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM28795     1  0.0592     0.8537 0.984 0.000 0.000 0.016
#&gt; GSM11301     4  0.4730     0.3688 0.000 0.364 0.000 0.636
#&gt; GSM11297     1  0.7393     0.3672 0.436 0.400 0.000 0.164
#&gt; GSM11298     1  0.0188     0.8545 0.996 0.000 0.000 0.004
#&gt; GSM11314     1  0.4830     0.6217 0.608 0.000 0.000 0.392
#&gt; GSM11299     2  0.7756    -0.0769 0.236 0.400 0.000 0.364
#&gt; GSM28783     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM11308     1  0.7393     0.3672 0.436 0.400 0.000 0.164
#&gt; GSM28782     1  0.0188     0.8545 0.996 0.000 0.000 0.004
#&gt; GSM28779     1  0.0000     0.8544 1.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.8544 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28789     1  0.3143      0.570 0.796 0.000 0.000 0.204 0.000
#&gt; GSM28790     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     5  0.0963      0.446 0.000 0.000 0.036 0.000 0.964
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     4  0.3109      0.842 0.000 0.200 0.000 0.800 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     4  0.2690      0.874 0.000 0.156 0.000 0.844 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.3586      0.638 0.736 0.000 0.000 0.000 0.264
#&gt; GSM28792     1  0.3741      0.637 0.732 0.000 0.000 0.004 0.264
#&gt; GSM11295     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28793     1  0.3231      0.702 0.800 0.000 0.000 0.004 0.196
#&gt; GSM11312     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28778     1  0.4307      0.136 0.504 0.000 0.000 0.496 0.000
#&gt; GSM28796     1  0.2813      0.725 0.832 0.000 0.000 0.000 0.168
#&gt; GSM11309     1  0.2439      0.755 0.876 0.000 0.000 0.004 0.120
#&gt; GSM11315     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.0162      0.811 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28776     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28777     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11316     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11320     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     1  0.3741      0.637 0.732 0.000 0.000 0.004 0.264
#&gt; GSM28786     1  0.3741      0.637 0.732 0.000 0.000 0.004 0.264
#&gt; GSM28800     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.3561      0.643 0.740 0.000 0.000 0.000 0.260
#&gt; GSM28787     3  0.3966      0.645 0.000 0.000 0.664 0.000 0.336
#&gt; GSM11304     5  0.3966      0.645 0.336 0.000 0.000 0.000 0.664
#&gt; GSM11303     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11317     3  0.0000      0.948 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     1  0.3741      0.637 0.732 0.000 0.000 0.004 0.264
#&gt; GSM28799     1  0.3741      0.637 0.732 0.000 0.000 0.004 0.264
#&gt; GSM28791     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28794     4  0.2690      0.663 0.156 0.000 0.000 0.844 0.000
#&gt; GSM28780     1  0.0162      0.811 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28795     1  0.0609      0.806 0.980 0.000 0.000 0.000 0.020
#&gt; GSM11301     4  0.2690      0.874 0.000 0.156 0.000 0.844 0.000
#&gt; GSM11297     5  0.3966      0.645 0.336 0.000 0.000 0.000 0.664
#&gt; GSM11298     1  0.0162      0.811 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11314     1  0.6746     -0.157 0.392 0.000 0.000 0.344 0.264
#&gt; GSM11299     5  0.0000      0.479 0.000 0.000 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11308     5  0.3966      0.645 0.336 0.000 0.000 0.000 0.664
#&gt; GSM28782     1  0.0162      0.811 0.996 0.000 0.000 0.004 0.000
#&gt; GSM28779     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.811 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28789     1  0.2416      0.659 0.844 0.000 0.000 0.000 0.156 0.000
#&gt; GSM28790     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11300     6  0.2697     -0.097 0.000 0.000 0.000 0.188 0.000 0.812
#&gt; GSM28798     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     5  0.2793      0.730 0.000 0.200 0.000 0.000 0.800 0.000
#&gt; GSM11319     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     5  0.0146      0.910 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11307     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.3221      0.682 0.736 0.000 0.000 0.000 0.000 0.264
#&gt; GSM28792     1  0.3360      0.680 0.732 0.000 0.000 0.000 0.004 0.264
#&gt; GSM11295     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM28793     1  0.2902      0.739 0.800 0.000 0.000 0.000 0.004 0.196
#&gt; GSM11312     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28778     1  0.3838      0.251 0.552 0.000 0.000 0.000 0.448 0.000
#&gt; GSM28796     1  0.2527      0.761 0.832 0.000 0.000 0.000 0.000 0.168
#&gt; GSM11309     1  0.2191      0.788 0.876 0.000 0.000 0.000 0.004 0.120
#&gt; GSM11315     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11306     1  0.0146      0.842 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28776     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28777     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM11316     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM11320     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM28797     1  0.3360      0.680 0.732 0.000 0.000 0.000 0.004 0.264
#&gt; GSM28786     1  0.3360      0.680 0.732 0.000 0.000 0.000 0.004 0.264
#&gt; GSM28800     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11310     1  0.3198      0.686 0.740 0.000 0.000 0.000 0.000 0.260
#&gt; GSM28787     4  0.3563      0.000 0.000 0.000 0.000 0.664 0.000 0.336
#&gt; GSM11304     6  0.3563      0.555 0.336 0.000 0.000 0.000 0.000 0.664
#&gt; GSM11303     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM11317     3  0.3862      0.707 0.000 0.000 0.524 0.476 0.000 0.000
#&gt; GSM11311     1  0.3360      0.680 0.732 0.000 0.000 0.000 0.004 0.264
#&gt; GSM28799     1  0.3360      0.680 0.732 0.000 0.000 0.000 0.004 0.264
#&gt; GSM28791     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM28794     5  0.0146      0.906 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; GSM28780     1  0.0146      0.842 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28795     1  0.0547      0.838 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM11301     5  0.0146      0.910 0.000 0.004 0.000 0.000 0.996 0.000
#&gt; GSM11297     6  0.3563      0.555 0.336 0.000 0.000 0.000 0.000 0.664
#&gt; GSM11298     1  0.0146      0.842 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM11314     3  0.5771     -0.248 0.000 0.000 0.476 0.336 0.000 0.188
#&gt; GSM11299     6  0.2697     -0.097 0.000 0.000 0.000 0.188 0.000 0.812
#&gt; GSM28783     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11308     6  0.3563      0.555 0.336 0.000 0.000 0.000 0.000 0.664
#&gt; GSM28782     1  0.0146      0.842 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM28779     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM11302     1  0.0000      0.843 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:pam 51     0.395 2
#> ATC:pam 51     0.371 3
#> ATC:pam 38     0.351 4
#> ATC:pam 48     0.328 5
#> ATC:pam 47     0.326 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.629           0.840       0.926         0.4187 0.618   0.618
#> 3 3 0.743           0.819       0.916         0.4252 0.750   0.601
#> 4 4 0.575           0.653       0.824         0.1702 0.880   0.721
#> 5 5 0.543           0.533       0.718         0.0548 0.860   0.649
#> 6 6 0.604           0.509       0.721         0.0479 0.864   0.598
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.1633      0.889 0.976 0.024
#&gt; GSM28789     1  0.5519      0.804 0.872 0.128
#&gt; GSM28790     1  0.0000      0.900 1.000 0.000
#&gt; GSM11300     1  0.8081      0.725 0.752 0.248
#&gt; GSM28798     2  0.0000      0.952 0.000 1.000
#&gt; GSM11296     2  0.0000      0.952 0.000 1.000
#&gt; GSM28801     2  0.0000      0.952 0.000 1.000
#&gt; GSM11319     2  0.0000      0.952 0.000 1.000
#&gt; GSM28781     2  0.0000      0.952 0.000 1.000
#&gt; GSM11305     2  0.0000      0.952 0.000 1.000
#&gt; GSM28784     2  0.0000      0.952 0.000 1.000
#&gt; GSM11307     2  0.0000      0.952 0.000 1.000
#&gt; GSM11313     2  0.0000      0.952 0.000 1.000
#&gt; GSM28785     2  0.0000      0.952 0.000 1.000
#&gt; GSM11318     1  0.0000      0.900 1.000 0.000
#&gt; GSM28792     1  0.0000      0.900 1.000 0.000
#&gt; GSM11295     1  0.8207      0.717 0.744 0.256
#&gt; GSM28793     1  0.0000      0.900 1.000 0.000
#&gt; GSM11312     1  0.0000      0.900 1.000 0.000
#&gt; GSM28778     1  0.9460      0.395 0.636 0.364
#&gt; GSM28796     1  0.0000      0.900 1.000 0.000
#&gt; GSM11309     1  0.0376      0.899 0.996 0.004
#&gt; GSM11315     1  0.0000      0.900 1.000 0.000
#&gt; GSM11306     1  0.2423      0.879 0.960 0.040
#&gt; GSM28776     1  0.0000      0.900 1.000 0.000
#&gt; GSM28777     1  0.8207      0.717 0.744 0.256
#&gt; GSM11316     1  0.8207      0.717 0.744 0.256
#&gt; GSM11320     1  0.8207      0.717 0.744 0.256
#&gt; GSM28797     1  0.0000      0.900 1.000 0.000
#&gt; GSM28786     1  0.5294      0.833 0.880 0.120
#&gt; GSM28800     1  0.0000      0.900 1.000 0.000
#&gt; GSM11310     1  0.0000      0.900 1.000 0.000
#&gt; GSM28787     1  0.8144      0.721 0.748 0.252
#&gt; GSM11304     1  0.0000      0.900 1.000 0.000
#&gt; GSM11303     1  0.8207      0.717 0.744 0.256
#&gt; GSM11317     1  0.8207      0.717 0.744 0.256
#&gt; GSM11311     1  0.0000      0.900 1.000 0.000
#&gt; GSM28799     1  0.0000      0.900 1.000 0.000
#&gt; GSM28791     1  0.1633      0.889 0.976 0.024
#&gt; GSM28794     2  0.1184      0.937 0.016 0.984
#&gt; GSM28780     1  0.1633      0.889 0.976 0.024
#&gt; GSM28795     1  0.9248      0.535 0.660 0.340
#&gt; GSM11301     2  0.0000      0.952 0.000 1.000
#&gt; GSM11297     1  0.0000      0.900 1.000 0.000
#&gt; GSM11298     1  0.0000      0.900 1.000 0.000
#&gt; GSM11314     2  0.9983     -0.135 0.476 0.524
#&gt; GSM11299     1  0.8016      0.729 0.756 0.244
#&gt; GSM28783     1  0.0000      0.900 1.000 0.000
#&gt; GSM11308     1  0.0000      0.900 1.000 0.000
#&gt; GSM28782     1  0.0672      0.897 0.992 0.008
#&gt; GSM28779     1  0.0000      0.900 1.000 0.000
#&gt; GSM11302     1  0.0000      0.900 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0424      0.961 0.992 0.000 0.008
#&gt; GSM28789     1  0.4164      0.804 0.848 0.008 0.144
#&gt; GSM28790     1  0.0424      0.961 0.992 0.000 0.008
#&gt; GSM11300     3  0.6653      0.597 0.288 0.032 0.680
#&gt; GSM28798     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM28801     2  0.6111      0.462 0.000 0.604 0.396
#&gt; GSM11319     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM28784     2  0.6045      0.493 0.000 0.620 0.380
#&gt; GSM11307     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000      0.827 0.000 1.000 0.000
#&gt; GSM11318     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM28792     1  0.3752      0.807 0.856 0.000 0.144
#&gt; GSM11295     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM28793     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM11312     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM28778     3  0.9228      0.319 0.416 0.152 0.432
#&gt; GSM28796     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM11309     1  0.1411      0.950 0.964 0.000 0.036
#&gt; GSM11315     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM11306     1  0.1878      0.934 0.952 0.004 0.044
#&gt; GSM28776     1  0.0000      0.962 1.000 0.000 0.000
#&gt; GSM28777     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM11316     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM11320     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM28797     1  0.1753      0.943 0.952 0.000 0.048
#&gt; GSM28786     1  0.4750      0.700 0.784 0.000 0.216
#&gt; GSM28800     1  0.0424      0.961 0.992 0.000 0.008
#&gt; GSM11310     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM28787     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM11304     1  0.1031      0.956 0.976 0.000 0.024
#&gt; GSM11303     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM11317     3  0.0237      0.764 0.004 0.000 0.996
#&gt; GSM11311     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM28799     1  0.3192      0.855 0.888 0.000 0.112
#&gt; GSM28791     1  0.0424      0.961 0.992 0.000 0.008
#&gt; GSM28794     2  0.7112      0.369 0.024 0.552 0.424
#&gt; GSM28780     1  0.0237      0.960 0.996 0.000 0.004
#&gt; GSM28795     3  0.8037      0.503 0.352 0.076 0.572
#&gt; GSM11301     2  0.6045      0.493 0.000 0.620 0.380
#&gt; GSM11297     1  0.0892      0.958 0.980 0.000 0.020
#&gt; GSM11298     1  0.0000      0.962 1.000 0.000 0.000
#&gt; GSM11314     3  0.5931      0.628 0.084 0.124 0.792
#&gt; GSM11299     3  0.6570      0.596 0.292 0.028 0.680
#&gt; GSM28783     1  0.0424      0.962 0.992 0.000 0.008
#&gt; GSM11308     1  0.0747      0.959 0.984 0.000 0.016
#&gt; GSM28782     1  0.0892      0.958 0.980 0.000 0.020
#&gt; GSM28779     1  0.0237      0.962 0.996 0.000 0.004
#&gt; GSM11302     1  0.0237      0.960 0.996 0.000 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.4855     0.2958 0.600 0.000 0.000 0.400
#&gt; GSM28789     4  0.4356     0.4754 0.292 0.000 0.000 0.708
#&gt; GSM28790     1  0.1637     0.7346 0.940 0.000 0.000 0.060
#&gt; GSM11300     1  0.7236     0.1932 0.552 0.008 0.300 0.140
#&gt; GSM28798     2  0.0188     0.8732 0.000 0.996 0.000 0.004
#&gt; GSM11296     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.5740     0.6212 0.000 0.700 0.208 0.092
#&gt; GSM11319     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0188     0.8732 0.000 0.996 0.000 0.004
#&gt; GSM11305     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.5910     0.6113 0.000 0.688 0.208 0.104
#&gt; GSM11307     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.8743 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.3172     0.6900 0.840 0.000 0.000 0.160
#&gt; GSM28792     1  0.5936     0.3817 0.576 0.000 0.044 0.380
#&gt; GSM11295     3  0.1211     0.9165 0.000 0.000 0.960 0.040
#&gt; GSM28793     1  0.2704     0.7207 0.876 0.000 0.000 0.124
#&gt; GSM11312     1  0.2216     0.7318 0.908 0.000 0.000 0.092
#&gt; GSM28778     4  0.5750     0.6331 0.112 0.032 0.100 0.756
#&gt; GSM28796     1  0.1716     0.7300 0.936 0.000 0.000 0.064
#&gt; GSM11309     1  0.5168    -0.0581 0.500 0.000 0.004 0.496
#&gt; GSM11315     1  0.2216     0.7314 0.908 0.000 0.000 0.092
#&gt; GSM11306     4  0.4866     0.2340 0.404 0.000 0.000 0.596
#&gt; GSM28776     1  0.3751     0.6601 0.800 0.000 0.004 0.196
#&gt; GSM28777     3  0.0000     0.9345 0.000 0.000 1.000 0.000
#&gt; GSM11316     3  0.0000     0.9345 0.000 0.000 1.000 0.000
#&gt; GSM11320     3  0.0000     0.9345 0.000 0.000 1.000 0.000
#&gt; GSM28797     1  0.3908     0.6461 0.784 0.000 0.004 0.212
#&gt; GSM28786     1  0.6516     0.3222 0.576 0.000 0.092 0.332
#&gt; GSM28800     1  0.4134     0.5929 0.740 0.000 0.000 0.260
#&gt; GSM11310     1  0.1557     0.7358 0.944 0.000 0.000 0.056
#&gt; GSM28787     3  0.2281     0.8759 0.000 0.000 0.904 0.096
#&gt; GSM11304     1  0.2730     0.7249 0.896 0.000 0.016 0.088
#&gt; GSM11303     3  0.0188     0.9317 0.004 0.000 0.996 0.000
#&gt; GSM11317     3  0.0000     0.9345 0.000 0.000 1.000 0.000
#&gt; GSM11311     1  0.2760     0.6923 0.872 0.000 0.000 0.128
#&gt; GSM28799     1  0.5855     0.4169 0.600 0.000 0.044 0.356
#&gt; GSM28791     1  0.3801     0.6381 0.780 0.000 0.000 0.220
#&gt; GSM28794     4  0.6929     0.3721 0.032 0.108 0.212 0.648
#&gt; GSM28780     1  0.4761     0.3759 0.628 0.000 0.000 0.372
#&gt; GSM28795     4  0.7605     0.4663 0.164 0.020 0.264 0.552
#&gt; GSM11301     2  0.7531     0.2861 0.000 0.476 0.208 0.316
#&gt; GSM11297     1  0.0921     0.7329 0.972 0.000 0.000 0.028
#&gt; GSM11298     1  0.0707     0.7353 0.980 0.000 0.000 0.020
#&gt; GSM11314     3  0.5565     0.6664 0.008 0.044 0.700 0.248
#&gt; GSM11299     1  0.7199     0.2079 0.560 0.008 0.292 0.140
#&gt; GSM28783     1  0.3649     0.6830 0.796 0.000 0.000 0.204
#&gt; GSM11308     1  0.0817     0.7321 0.976 0.000 0.000 0.024
#&gt; GSM28782     1  0.3610     0.6519 0.800 0.000 0.000 0.200
#&gt; GSM28779     1  0.1557     0.7290 0.944 0.000 0.000 0.056
#&gt; GSM11302     1  0.2281     0.7286 0.904 0.000 0.000 0.096
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     1  0.3630     0.5429 0.780 0.000 0.000 0.204 0.016
#&gt; GSM28789     1  0.5467     0.2698 0.524 0.000 0.000 0.412 0.064
#&gt; GSM28790     1  0.2685     0.6180 0.880 0.000 0.000 0.092 0.028
#&gt; GSM11300     4  0.7665     0.1636 0.160 0.000 0.344 0.412 0.084
#&gt; GSM28798     2  0.0703     0.8986 0.000 0.976 0.000 0.000 0.024
#&gt; GSM11296     2  0.0000     0.9071 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.6727    -0.2438 0.000 0.436 0.188 0.008 0.368
#&gt; GSM11319     2  0.0510     0.9026 0.000 0.984 0.000 0.000 0.016
#&gt; GSM28781     2  0.0703     0.8986 0.000 0.976 0.000 0.000 0.024
#&gt; GSM11305     2  0.0000     0.9071 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     5  0.6700     0.2403 0.000 0.348 0.188 0.008 0.456
#&gt; GSM11307     2  0.0000     0.9071 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9071 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9071 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.4901     0.5396 0.708 0.000 0.000 0.096 0.196
#&gt; GSM28792     1  0.6494     0.2390 0.492 0.000 0.000 0.252 0.256
#&gt; GSM11295     3  0.0510     0.9443 0.000 0.000 0.984 0.000 0.016
#&gt; GSM28793     1  0.2616     0.6077 0.880 0.000 0.000 0.100 0.020
#&gt; GSM11312     1  0.4430     0.5945 0.752 0.000 0.000 0.076 0.172
#&gt; GSM28778     5  0.7875    -0.1291 0.264 0.000 0.076 0.268 0.392
#&gt; GSM28796     1  0.2233     0.6139 0.904 0.000 0.000 0.080 0.016
#&gt; GSM11309     4  0.4966    -0.1095 0.404 0.000 0.000 0.564 0.032
#&gt; GSM11315     1  0.3011     0.5990 0.844 0.000 0.000 0.140 0.016
#&gt; GSM11306     1  0.4730     0.4589 0.688 0.000 0.000 0.260 0.052
#&gt; GSM28776     1  0.4725     0.5561 0.720 0.000 0.000 0.080 0.200
#&gt; GSM28777     3  0.0162     0.9518 0.000 0.000 0.996 0.000 0.004
#&gt; GSM11316     3  0.0162     0.9512 0.000 0.000 0.996 0.004 0.000
#&gt; GSM11320     3  0.0000     0.9519 0.000 0.000 1.000 0.000 0.000
#&gt; GSM28797     1  0.4705     0.1547 0.504 0.000 0.004 0.484 0.008
#&gt; GSM28786     4  0.6215    -0.0303 0.428 0.000 0.008 0.456 0.108
#&gt; GSM28800     1  0.5886     0.4116 0.600 0.000 0.000 0.224 0.176
#&gt; GSM11310     1  0.2769     0.6074 0.876 0.000 0.000 0.092 0.032
#&gt; GSM28787     3  0.3730     0.7457 0.004 0.000 0.800 0.028 0.168
#&gt; GSM11304     1  0.4734     0.3196 0.604 0.000 0.000 0.372 0.024
#&gt; GSM11303     3  0.0703     0.9373 0.000 0.000 0.976 0.024 0.000
#&gt; GSM11317     3  0.0000     0.9519 0.000 0.000 1.000 0.000 0.000
#&gt; GSM11311     1  0.4252     0.3614 0.652 0.000 0.000 0.340 0.008
#&gt; GSM28799     1  0.6420     0.2649 0.508 0.000 0.000 0.260 0.232
#&gt; GSM28791     1  0.2629     0.5951 0.860 0.000 0.000 0.136 0.004
#&gt; GSM28794     5  0.6956     0.4457 0.092 0.012 0.188 0.100 0.608
#&gt; GSM28780     1  0.4309     0.5140 0.676 0.000 0.000 0.308 0.016
#&gt; GSM28795     5  0.7990     0.1318 0.172 0.000 0.152 0.228 0.448
#&gt; GSM11301     5  0.7050     0.3872 0.004 0.272 0.188 0.028 0.508
#&gt; GSM11297     1  0.4770     0.3861 0.644 0.000 0.000 0.320 0.036
#&gt; GSM11298     1  0.1018     0.6234 0.968 0.000 0.000 0.016 0.016
#&gt; GSM11314     5  0.6251     0.3625 0.016 0.004 0.216 0.152 0.612
#&gt; GSM11299     4  0.7852     0.3314 0.236 0.000 0.248 0.428 0.088
#&gt; GSM28783     1  0.4521     0.5849 0.748 0.000 0.000 0.088 0.164
#&gt; GSM11308     1  0.4498     0.4372 0.688 0.000 0.000 0.280 0.032
#&gt; GSM28782     1  0.4238     0.4169 0.628 0.000 0.000 0.368 0.004
#&gt; GSM28779     1  0.3772     0.5883 0.792 0.000 0.000 0.036 0.172
#&gt; GSM11302     1  0.2011     0.6177 0.908 0.000 0.000 0.088 0.004
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     1  0.3532     0.5359 0.796 0.000 0.000 0.140 0.000 0.064
#&gt; GSM28789     1  0.5501     0.3925 0.564 0.000 0.000 0.200 0.000 0.236
#&gt; GSM28790     1  0.0820     0.6152 0.972 0.000 0.000 0.016 0.000 0.012
#&gt; GSM11300     6  0.7586    -0.0634 0.020 0.000 0.160 0.240 0.148 0.432
#&gt; GSM28798     2  0.0820     0.9757 0.000 0.972 0.000 0.000 0.012 0.016
#&gt; GSM11296     2  0.0000     0.9867 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     6  0.6137     0.3819 0.000 0.248 0.000 0.004 0.336 0.412
#&gt; GSM11319     2  0.0717     0.9781 0.000 0.976 0.000 0.000 0.008 0.016
#&gt; GSM28781     2  0.0820     0.9757 0.000 0.972 0.000 0.000 0.012 0.016
#&gt; GSM11305     2  0.0000     0.9867 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     6  0.6110     0.3863 0.000 0.236 0.000 0.004 0.344 0.416
#&gt; GSM11307     2  0.0000     0.9867 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9867 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9867 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     4  0.4413     0.1647 0.484 0.000 0.000 0.496 0.012 0.008
#&gt; GSM28792     4  0.2482     0.4335 0.148 0.000 0.000 0.848 0.004 0.000
#&gt; GSM11295     3  0.2191     0.7952 0.000 0.000 0.876 0.000 0.120 0.004
#&gt; GSM28793     1  0.1856     0.6094 0.920 0.000 0.000 0.048 0.000 0.032
#&gt; GSM11312     1  0.4124     0.1991 0.644 0.000 0.000 0.332 0.000 0.024
#&gt; GSM28778     4  0.6970     0.0248 0.256 0.000 0.000 0.372 0.312 0.060
#&gt; GSM28796     1  0.2376     0.6086 0.888 0.000 0.000 0.068 0.000 0.044
#&gt; GSM11309     1  0.6676     0.2535 0.468 0.000 0.000 0.208 0.056 0.268
#&gt; GSM11315     1  0.2563     0.6118 0.876 0.000 0.000 0.052 0.000 0.072
#&gt; GSM11306     1  0.4251     0.4651 0.716 0.000 0.000 0.208 0.000 0.076
#&gt; GSM28776     4  0.4096     0.1583 0.484 0.000 0.000 0.508 0.000 0.008
#&gt; GSM28777     3  0.0000     0.9630 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11316     3  0.0000     0.9630 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11320     3  0.0000     0.9630 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28797     1  0.5368     0.4069 0.532 0.000 0.000 0.092 0.008 0.368
#&gt; GSM28786     4  0.6271     0.1441 0.148 0.000 0.000 0.524 0.048 0.280
#&gt; GSM28800     1  0.4177    -0.0895 0.520 0.000 0.000 0.468 0.000 0.012
#&gt; GSM11310     1  0.3301     0.4752 0.788 0.000 0.000 0.188 0.000 0.024
#&gt; GSM28787     5  0.5101     0.0782 0.004 0.000 0.436 0.036 0.508 0.016
#&gt; GSM11304     1  0.4988     0.4123 0.552 0.000 0.000 0.064 0.004 0.380
#&gt; GSM11303     3  0.0146     0.9599 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM11317     3  0.0000     0.9630 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11311     1  0.4340     0.4966 0.712 0.000 0.000 0.200 0.000 0.088
#&gt; GSM28799     4  0.2920     0.4521 0.168 0.000 0.000 0.820 0.004 0.008
#&gt; GSM28791     1  0.1049     0.6119 0.960 0.000 0.000 0.008 0.000 0.032
#&gt; GSM28794     6  0.6051     0.0689 0.004 0.000 0.000 0.212 0.372 0.412
#&gt; GSM28780     1  0.3727     0.5799 0.748 0.000 0.000 0.036 0.000 0.216
#&gt; GSM28795     5  0.6504     0.1433 0.164 0.000 0.000 0.260 0.512 0.064
#&gt; GSM11301     6  0.6404     0.3817 0.000 0.228 0.000 0.020 0.340 0.412
#&gt; GSM11297     1  0.4789     0.4819 0.640 0.000 0.000 0.092 0.000 0.268
#&gt; GSM11298     1  0.0972     0.6149 0.964 0.000 0.000 0.028 0.000 0.008
#&gt; GSM11314     5  0.0508     0.1542 0.000 0.000 0.000 0.004 0.984 0.012
#&gt; GSM11299     6  0.7715    -0.0370 0.096 0.000 0.052 0.272 0.152 0.428
#&gt; GSM28783     4  0.4705     0.0482 0.472 0.000 0.000 0.484 0.000 0.044
#&gt; GSM11308     1  0.4972     0.4594 0.628 0.000 0.000 0.116 0.000 0.256
#&gt; GSM28782     1  0.3518     0.5667 0.732 0.000 0.000 0.012 0.000 0.256
#&gt; GSM28779     1  0.3874     0.1507 0.636 0.000 0.000 0.356 0.000 0.008
#&gt; GSM11302     1  0.1265     0.6164 0.948 0.000 0.000 0.008 0.000 0.044
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n tissue(p) k
#> ATC:mclust 50     0.394 2
#> ATC:mclust 47     0.366 3
#> ATC:mclust 39     0.405 4
#> ATC:mclust 30     0.403 5
#> ATC:mclust 24     0.392 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21288 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.919           0.924       0.968         0.4409 0.566   0.566
#> 3 3 1.000           0.947       0.982         0.3644 0.681   0.502
#> 4 4 0.713           0.763       0.876         0.1524 0.905   0.767
#> 5 5 0.687           0.587       0.784         0.1124 0.876   0.640
#> 6 6 0.721           0.644       0.794         0.0456 0.931   0.736
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2
#&gt; GSM28788     1  0.4161      0.893 0.916 0.084
#&gt; GSM28789     2  0.0938      0.955 0.012 0.988
#&gt; GSM28790     2  0.6623      0.779 0.172 0.828
#&gt; GSM11300     1  0.0000      0.965 1.000 0.000
#&gt; GSM28798     2  0.0000      0.964 0.000 1.000
#&gt; GSM11296     2  0.0000      0.964 0.000 1.000
#&gt; GSM28801     2  0.0000      0.964 0.000 1.000
#&gt; GSM11319     2  0.0000      0.964 0.000 1.000
#&gt; GSM28781     2  0.0000      0.964 0.000 1.000
#&gt; GSM11305     2  0.0000      0.964 0.000 1.000
#&gt; GSM28784     2  0.0000      0.964 0.000 1.000
#&gt; GSM11307     2  0.0000      0.964 0.000 1.000
#&gt; GSM11313     2  0.0000      0.964 0.000 1.000
#&gt; GSM28785     2  0.0000      0.964 0.000 1.000
#&gt; GSM11318     1  0.0000      0.965 1.000 0.000
#&gt; GSM28792     1  0.0000      0.965 1.000 0.000
#&gt; GSM11295     1  0.0000      0.965 1.000 0.000
#&gt; GSM28793     1  0.0000      0.965 1.000 0.000
#&gt; GSM11312     1  0.8081      0.675 0.752 0.248
#&gt; GSM28778     2  0.0000      0.964 0.000 1.000
#&gt; GSM28796     1  0.0000      0.965 1.000 0.000
#&gt; GSM11309     1  0.0000      0.965 1.000 0.000
#&gt; GSM11315     1  0.8955      0.558 0.688 0.312
#&gt; GSM11306     2  0.9044      0.511 0.320 0.680
#&gt; GSM28776     1  0.0000      0.965 1.000 0.000
#&gt; GSM28777     1  0.0000      0.965 1.000 0.000
#&gt; GSM11316     1  0.0000      0.965 1.000 0.000
#&gt; GSM11320     1  0.0000      0.965 1.000 0.000
#&gt; GSM28797     1  0.0000      0.965 1.000 0.000
#&gt; GSM28786     1  0.0000      0.965 1.000 0.000
#&gt; GSM28800     1  0.3274      0.917 0.940 0.060
#&gt; GSM11310     1  0.0000      0.965 1.000 0.000
#&gt; GSM28787     1  0.0000      0.965 1.000 0.000
#&gt; GSM11304     1  0.0000      0.965 1.000 0.000
#&gt; GSM11303     1  0.0000      0.965 1.000 0.000
#&gt; GSM11317     1  0.0000      0.965 1.000 0.000
#&gt; GSM11311     1  0.0000      0.965 1.000 0.000
#&gt; GSM28799     1  0.0000      0.965 1.000 0.000
#&gt; GSM28791     1  0.9710      0.345 0.600 0.400
#&gt; GSM28794     2  0.0000      0.964 0.000 1.000
#&gt; GSM28780     1  0.1843      0.945 0.972 0.028
#&gt; GSM28795     1  0.0000      0.965 1.000 0.000
#&gt; GSM11301     2  0.0000      0.964 0.000 1.000
#&gt; GSM11297     1  0.0000      0.965 1.000 0.000
#&gt; GSM11298     1  0.0000      0.965 1.000 0.000
#&gt; GSM11314     1  0.1184      0.955 0.984 0.016
#&gt; GSM11299     1  0.0000      0.965 1.000 0.000
#&gt; GSM28783     1  0.0000      0.965 1.000 0.000
#&gt; GSM11308     1  0.0000      0.965 1.000 0.000
#&gt; GSM28782     1  0.0672      0.960 0.992 0.008
#&gt; GSM28779     1  0.1184      0.955 0.984 0.016
#&gt; GSM11302     1  0.0376      0.962 0.996 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3
#&gt; GSM28788     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28789     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28790     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11300     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM28798     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11296     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM28801     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11319     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM28781     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11305     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM28784     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11307     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11313     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM28785     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11318     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28792     1  0.1031     0.9601 0.976 0.000 0.024
#&gt; GSM11295     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM28793     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11312     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28778     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28796     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11309     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11315     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11306     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28776     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28777     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM11316     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM11320     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM28797     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28786     1  0.6111     0.2995 0.604 0.000 0.396
#&gt; GSM28800     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11310     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28787     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM11304     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11303     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM11317     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM11311     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28799     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28791     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28794     2  0.0747     0.9783 0.016 0.984 0.000
#&gt; GSM28780     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28795     3  0.6291     0.0989 0.468 0.000 0.532
#&gt; GSM11301     2  0.0000     0.9980 0.000 1.000 0.000
#&gt; GSM11297     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11298     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11314     3  0.1031     0.9153 0.000 0.024 0.976
#&gt; GSM11299     3  0.0000     0.9358 0.000 0.000 1.000
#&gt; GSM28783     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11308     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28782     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM28779     1  0.0000     0.9841 1.000 0.000 0.000
#&gt; GSM11302     1  0.0000     0.9841 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4
#&gt; GSM28788     1  0.4730     0.2228 0.636 0.000 0.000 0.364
#&gt; GSM28789     1  0.3837     0.6576 0.776 0.000 0.000 0.224
#&gt; GSM28790     1  0.0336     0.7958 0.992 0.000 0.000 0.008
#&gt; GSM11300     3  0.2281     0.8395 0.000 0.000 0.904 0.096
#&gt; GSM28798     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11296     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM28801     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11319     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM28781     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11305     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM28784     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11307     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11313     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM28785     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11318     1  0.1792     0.7758 0.932 0.000 0.000 0.068
#&gt; GSM28792     1  0.4100     0.6601 0.816 0.000 0.036 0.148
#&gt; GSM11295     3  0.0707     0.8654 0.000 0.000 0.980 0.020
#&gt; GSM28793     1  0.1474     0.7869 0.948 0.000 0.000 0.052
#&gt; GSM11312     1  0.4250     0.4119 0.724 0.000 0.000 0.276
#&gt; GSM28778     4  0.4770     0.6460 0.288 0.012 0.000 0.700
#&gt; GSM28796     1  0.2345     0.7603 0.900 0.000 0.000 0.100
#&gt; GSM11309     1  0.3074     0.7601 0.848 0.000 0.000 0.152
#&gt; GSM11315     1  0.2081     0.7719 0.916 0.000 0.000 0.084
#&gt; GSM11306     4  0.4992     0.3834 0.476 0.000 0.000 0.524
#&gt; GSM28776     1  0.2868     0.7217 0.864 0.000 0.000 0.136
#&gt; GSM28777     3  0.0592     0.8639 0.000 0.000 0.984 0.016
#&gt; GSM11316     3  0.0469     0.8647 0.000 0.000 0.988 0.012
#&gt; GSM11320     3  0.0469     0.8649 0.000 0.000 0.988 0.012
#&gt; GSM28797     1  0.4252     0.6063 0.744 0.000 0.004 0.252
#&gt; GSM28786     3  0.7321    -0.0404 0.328 0.000 0.500 0.172
#&gt; GSM28800     1  0.2921     0.7184 0.860 0.000 0.000 0.140
#&gt; GSM11310     1  0.1474     0.7850 0.948 0.000 0.000 0.052
#&gt; GSM28787     3  0.2589     0.8220 0.000 0.000 0.884 0.116
#&gt; GSM11304     1  0.4655     0.6193 0.760 0.000 0.032 0.208
#&gt; GSM11303     3  0.1716     0.8548 0.000 0.000 0.936 0.064
#&gt; GSM11317     3  0.0336     0.8651 0.000 0.000 0.992 0.008
#&gt; GSM11311     1  0.2216     0.7808 0.908 0.000 0.000 0.092
#&gt; GSM28799     1  0.3569     0.6504 0.804 0.000 0.000 0.196
#&gt; GSM28791     1  0.2011     0.7912 0.920 0.000 0.000 0.080
#&gt; GSM28794     2  0.1174     0.9599 0.020 0.968 0.000 0.012
#&gt; GSM28780     1  0.2868     0.7686 0.864 0.000 0.000 0.136
#&gt; GSM28795     4  0.5763     0.4679 0.156 0.000 0.132 0.712
#&gt; GSM11301     2  0.0000     0.9965 0.000 1.000 0.000 0.000
#&gt; GSM11297     1  0.2921     0.7609 0.860 0.000 0.000 0.140
#&gt; GSM11298     1  0.0817     0.7922 0.976 0.000 0.000 0.024
#&gt; GSM11314     3  0.5143     0.4593 0.000 0.004 0.540 0.456
#&gt; GSM11299     3  0.2593     0.8361 0.004 0.000 0.892 0.104
#&gt; GSM28783     4  0.4994     0.4948 0.480 0.000 0.000 0.520
#&gt; GSM11308     1  0.3074     0.7548 0.848 0.000 0.000 0.152
#&gt; GSM28782     1  0.2921     0.7631 0.860 0.000 0.000 0.140
#&gt; GSM28779     1  0.2469     0.7454 0.892 0.000 0.000 0.108
#&gt; GSM11302     1  0.0817     0.7939 0.976 0.000 0.000 0.024
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM28788     4  0.4395    0.50707 0.188 0.000 0.000 0.748 0.064
#&gt; GSM28789     4  0.3480    0.47476 0.248 0.000 0.000 0.752 0.000
#&gt; GSM28790     1  0.2136    0.63586 0.904 0.000 0.000 0.088 0.008
#&gt; GSM11300     3  0.3117    0.83217 0.004 0.000 0.860 0.036 0.100
#&gt; GSM28798     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28781     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.0451    0.98420 0.008 0.988 0.000 0.000 0.004
#&gt; GSM11307     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.2992    0.61438 0.868 0.000 0.000 0.064 0.068
#&gt; GSM28792     1  0.5112    0.49197 0.692 0.000 0.012 0.064 0.232
#&gt; GSM11295     3  0.1894    0.87185 0.000 0.000 0.920 0.008 0.072
#&gt; GSM28793     1  0.2408    0.63045 0.892 0.000 0.000 0.092 0.016
#&gt; GSM11312     1  0.6553   -0.00202 0.432 0.000 0.000 0.364 0.204
#&gt; GSM28778     4  0.5775   -0.45964 0.088 0.000 0.000 0.472 0.440
#&gt; GSM28796     1  0.1282    0.64302 0.952 0.000 0.000 0.004 0.044
#&gt; GSM11309     4  0.3884    0.43631 0.288 0.000 0.000 0.708 0.004
#&gt; GSM11315     1  0.1364    0.64214 0.952 0.000 0.000 0.012 0.036
#&gt; GSM11306     4  0.4291    0.31082 0.092 0.000 0.000 0.772 0.136
#&gt; GSM28776     1  0.4433    0.53177 0.740 0.000 0.000 0.200 0.060
#&gt; GSM28777     3  0.1557    0.87112 0.000 0.000 0.940 0.008 0.052
#&gt; GSM11316     3  0.1331    0.87390 0.000 0.000 0.952 0.008 0.040
#&gt; GSM11320     3  0.1697    0.87440 0.000 0.000 0.932 0.008 0.060
#&gt; GSM28797     4  0.4535    0.51308 0.184 0.000 0.020 0.756 0.040
#&gt; GSM28786     4  0.7310    0.22561 0.088 0.000 0.184 0.536 0.192
#&gt; GSM28800     1  0.4686    0.53151 0.736 0.000 0.000 0.160 0.104
#&gt; GSM11310     1  0.3039    0.62296 0.836 0.000 0.000 0.152 0.012
#&gt; GSM28787     3  0.2077    0.84571 0.000 0.000 0.908 0.008 0.084
#&gt; GSM11304     1  0.5383    0.44040 0.688 0.000 0.028 0.220 0.064
#&gt; GSM11303     3  0.3081    0.81515 0.000 0.000 0.832 0.012 0.156
#&gt; GSM11317     3  0.1484    0.87785 0.000 0.000 0.944 0.008 0.048
#&gt; GSM11311     1  0.4702    0.06723 0.552 0.000 0.000 0.432 0.016
#&gt; GSM28799     1  0.5379    0.45688 0.668 0.000 0.000 0.164 0.168
#&gt; GSM28791     1  0.3932    0.36589 0.672 0.000 0.000 0.328 0.000
#&gt; GSM28794     2  0.1082    0.95523 0.028 0.964 0.000 0.000 0.008
#&gt; GSM28780     4  0.4818    0.08993 0.460 0.000 0.000 0.520 0.020
#&gt; GSM28795     5  0.5988    0.24048 0.036 0.000 0.044 0.400 0.520
#&gt; GSM11301     2  0.0000    0.99473 0.000 1.000 0.000 0.000 0.000
#&gt; GSM11297     1  0.4916    0.26836 0.628 0.000 0.016 0.340 0.016
#&gt; GSM11298     1  0.1671    0.64243 0.924 0.000 0.000 0.076 0.000
#&gt; GSM11314     5  0.4474    0.16980 0.004 0.000 0.332 0.012 0.652
#&gt; GSM11299     3  0.4089    0.77343 0.024 0.000 0.808 0.044 0.124
#&gt; GSM28783     4  0.6232   -0.37580 0.128 0.000 0.004 0.488 0.380
#&gt; GSM11308     1  0.4964    0.03124 0.548 0.000 0.012 0.428 0.012
#&gt; GSM28782     4  0.4656    0.04860 0.480 0.000 0.000 0.508 0.012
#&gt; GSM28779     1  0.3521    0.59330 0.820 0.000 0.000 0.140 0.040
#&gt; GSM11302     1  0.2970    0.61569 0.828 0.000 0.000 0.168 0.004
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;          class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM28788     4  0.4253      0.400 0.064 0.000 0.000 0.704 0.232 0.000
#&gt; GSM28789     4  0.2554      0.618 0.076 0.000 0.000 0.876 0.048 0.000
#&gt; GSM28790     1  0.3091      0.653 0.824 0.000 0.000 0.148 0.004 0.024
#&gt; GSM11300     3  0.4391      0.618 0.008 0.000 0.716 0.052 0.004 0.220
#&gt; GSM28798     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11296     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28801     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11319     2  0.0146      0.991 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM28781     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11305     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28784     2  0.1167      0.965 0.008 0.960 0.000 0.000 0.020 0.012
#&gt; GSM11307     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11313     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM28785     2  0.0000      0.993 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM11318     1  0.3361      0.665 0.844 0.000 0.000 0.064 0.044 0.048
#&gt; GSM28792     1  0.6222      0.381 0.528 0.000 0.004 0.080 0.072 0.316
#&gt; GSM11295     3  0.1578      0.732 0.000 0.000 0.936 0.004 0.012 0.048
#&gt; GSM28793     1  0.2809      0.664 0.848 0.000 0.000 0.128 0.004 0.020
#&gt; GSM11312     1  0.6683      0.172 0.420 0.000 0.000 0.128 0.372 0.080
#&gt; GSM28778     5  0.2922      0.840 0.056 0.000 0.000 0.068 0.864 0.012
#&gt; GSM28796     1  0.1426      0.679 0.948 0.000 0.000 0.016 0.008 0.028
#&gt; GSM11309     4  0.2239      0.628 0.072 0.000 0.000 0.900 0.020 0.008
#&gt; GSM11315     1  0.1426      0.678 0.948 0.000 0.000 0.016 0.008 0.028
#&gt; GSM11306     4  0.4484      0.241 0.028 0.000 0.000 0.640 0.320 0.012
#&gt; GSM28776     1  0.5074      0.562 0.680 0.000 0.000 0.044 0.208 0.068
#&gt; GSM28777     3  0.2882      0.705 0.000 0.000 0.812 0.000 0.008 0.180
#&gt; GSM11316     3  0.2520      0.718 0.000 0.000 0.844 0.000 0.004 0.152
#&gt; GSM11320     3  0.1464      0.725 0.000 0.000 0.944 0.004 0.016 0.036
#&gt; GSM28797     4  0.3722      0.583 0.040 0.000 0.024 0.836 0.044 0.056
#&gt; GSM28786     4  0.5735      0.386 0.040 0.000 0.124 0.648 0.012 0.176
#&gt; GSM28800     1  0.4208      0.619 0.740 0.000 0.000 0.036 0.200 0.024
#&gt; GSM11310     1  0.5695      0.407 0.584 0.000 0.000 0.288 0.048 0.080
#&gt; GSM28787     3  0.3819      0.618 0.000 0.000 0.764 0.000 0.064 0.172
#&gt; GSM11304     1  0.6063      0.167 0.528 0.000 0.028 0.324 0.008 0.112
#&gt; GSM11303     3  0.3424      0.570 0.000 0.000 0.780 0.004 0.020 0.196
#&gt; GSM11317     3  0.0603      0.738 0.000 0.000 0.980 0.004 0.000 0.016
#&gt; GSM11311     4  0.4452      0.477 0.316 0.000 0.000 0.636 0.000 0.048
#&gt; GSM28799     1  0.5399      0.565 0.680 0.000 0.000 0.144 0.076 0.100
#&gt; GSM28791     1  0.5367      0.111 0.524 0.000 0.000 0.388 0.016 0.072
#&gt; GSM28794     2  0.0837      0.971 0.020 0.972 0.000 0.000 0.004 0.004
#&gt; GSM28780     4  0.5178      0.527 0.236 0.000 0.000 0.652 0.028 0.084
#&gt; GSM28795     5  0.2469      0.799 0.012 0.000 0.028 0.036 0.904 0.020
#&gt; GSM11301     2  0.0146      0.991 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM11297     4  0.5128      0.300 0.412 0.000 0.000 0.504 0.000 0.084
#&gt; GSM11298     1  0.3346      0.630 0.816 0.000 0.000 0.140 0.008 0.036
#&gt; GSM11314     6  0.5976      0.000 0.008 0.000 0.212 0.008 0.224 0.548
#&gt; GSM11299     3  0.5215      0.396 0.032 0.000 0.600 0.052 0.000 0.316
#&gt; GSM28783     5  0.3995      0.801 0.056 0.000 0.000 0.104 0.796 0.044
#&gt; GSM11308     4  0.5386      0.422 0.336 0.000 0.012 0.568 0.004 0.080
#&gt; GSM28782     4  0.4742      0.544 0.240 0.000 0.000 0.676 0.012 0.072
#&gt; GSM28779     1  0.3804      0.651 0.772 0.000 0.000 0.044 0.176 0.008
#&gt; GSM11302     1  0.3736      0.653 0.788 0.000 0.000 0.160 0.032 0.020
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n tissue(p) k
#> ATC:NMF 51     0.395 2
#> ATC:NMF 50     0.370 3
#> ATC:NMF 45     0.341 4
#> ATC:NMF 34     0.398 5
#> ATC:NMF 39     0.431 6
```



If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      parallel  stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0     ComplexHeatmap_2.1.1  markdown_1.1          knitr_1.26           
#>  [5] preprocessCore_1.46.0 cola_1.3.2            GEOquery_2.52.0       Biobase_2.44.0       
#>  [9] BiocGenerics_0.30.0   GetoptLong_0.1.7     
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6         matrixStats_0.55.0   bit64_0.9-7          doParallel_1.0.15   
#>  [5] RColorBrewer_1.1-2   httr_1.4.1           tools_3.6.0          backports_1.1.5     
#>  [9] R6_2.4.1             DBI_1.0.0            lazyeval_0.2.2       colorspace_1.4-1    
#> [13] withr_2.1.2          tidyselect_0.2.5     gridExtra_2.3        bit_1.1-14          
#> [17] compiler_3.6.0       xml2_1.2.2           microbenchmark_1.4-7 pkgmaker_0.28       
#> [21] slam_0.1-46          scales_1.1.0         readr_1.3.1          NMF_0.23.6          
#> [25] stringr_1.4.0        digest_0.6.23        pkgconfig_2.0.3      bibtex_0.4.2        
#> [29] highr_0.8            limma_3.40.6         rlang_0.4.2          GlobalOptions_0.1.1 
#> [33] RSQLite_2.1.2        impute_1.58.0        shape_1.4.4          mclust_5.4.5        
#> [37] dendextend_1.12.0    dplyr_0.8.3          RCurl_1.95-4.12      magrittr_1.5        
#> [41] Matrix_1.2-17        Rcpp_1.0.3           munsell_0.5.0        S4Vectors_0.22.1    
#> [45] viridis_0.5.1        lifecycle_0.1.0      stringi_1.4.3        plyr_1.8.4          
#> [49] blob_1.2.0           crayon_1.3.4         lattice_0.20-38      splines_3.6.0       
#> [53] annotate_1.62.0      circlize_0.4.9       hms_0.5.2            zeallot_0.1.0       
#> [57] pillar_1.4.2         rjson_0.2.20         rngtools_1.4         reshape2_1.4.3      
#> [61] codetools_0.2-16     stats4_3.6.0         XML_3.98-1.20        glue_1.3.1          
#> [65] evaluate_0.14        png_0.1-7            vctrs_0.2.0          foreach_1.4.7       
#> [69] polyclip_1.10-0      gtable_0.3.0         purrr_0.3.3          tidyr_1.0.0         
#> [73] clue_0.3-57          assertthat_0.2.1     ggplot2_3.2.1        xfun_0.11           
#> [77] gridBase_0.4-7       eulerr_6.0.0         xtable_1.8-4         skmeans_0.2-11      
#> [81] survival_2.44-1.1    viridisLite_0.3.0    tibble_2.1.3         iterators_1.0.12    
#> [85] AnnotationDbi_1.46.1 registry_0.5-1       memoise_1.1.0        IRanges_2.18.3      
#> [89] cluster_2.1.0        brew_1.0-6
```


