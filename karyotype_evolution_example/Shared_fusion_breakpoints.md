### Shared Fusion Breakpoints

We will use an ancestral karyotype as an example to comprehensively explain how to build a fusion positions database and detect shared breakpoints across different species.

---

#### Identifying Fusion Breakpoints and Generating the Database

We start by using the WGDI toolkit (Sun et al., 2022). With the `-d` parameter, we generate a homologous gene dotplot between *Cardamine hirsuta* and the ancestral karyotype (ACBK). Then, by applying the `-km` parameter of WGDI, we map the protochromosomes of the ancestral karyotype to the query genome based on collinearity, creating the ancestor file [chi2s.ancestor.txt](./data/chi2s.ancestor.txt).

![Dot - plot](<./data/chi2s_ACBK.dotplot.order.png>)

From the dotplot, we can see a reciprocal chromosome translocation between chromosomes ACBK7 and ACBK8, labeled as ACBK/7_8/RCT. By reading the [chi2s.ancestor.txt](./data/chi2s.ancestor.txt) file, we can find the fusion breakpoints at Chr3:2208 and Chr8:759.

This information is recorded in a document as described in the **Fusion Positions File** section of the [WGDI documentation](https://wgdi.readthedocs.io/en/master/usage.html). An example of such a document is [chi2s.txt](./data/chi2s.txt). By running the WGDI command with the `-fpd` parameter (e.g., `wgdi -fpd chi2s.conf`), we generate the corresponding fusion positions database. This process produces several output files, including the essential `.gff`, `.lens`, `.pep` files, and the ancestor file (e.g., [fusion_breakpoints.ancestor.txt](./data/fusion_breakpoints.ancestor.txt)). For more details and additional files like [chi2s.conf](./data/chi2s.conf), refer to the [data/](./data/) directory.

When dealing with multiple fusion breakpoints from different species, ensure the output file remains consistent and each input record (e.g., [chi2s.txt](./data/chi2s.txt)) has a unique marker in the last column. Repeatedly executing `wgdi -fpd *.conf` will add more fusion breakpoints to the same output file.

---

#### Testing for Shared Fusion Breakpoints in Additional Species

To check if the identified fusion breakpoints are shared among other species, we compare their genomes with the generated fusion breakpoints database using the WGDI parameters `-d`, `-icl`, `-bi`, and `-fd`.

Note that the fusion breakpoints database may contain regions extracted multiple times. So, collinearity extraction should be done through interchromosomal comparisons. When using the `-icl` parameter, set `comparison = chromosomes` and adjust `repeat_number` to a smaller value, such as `repeat_number = 5`.

![Dotplot for Capsella rubella](<./data/cru1s_fusion_breakpoints.dotplot.order.png>)

The criterion for identifying shared fusion positions is that a synteny block spans the fusion breakpoint. Specifically, there must be collinear genes on both sides of the breakpoint, which is controlled by the `min_genes_per_side` parameter.

The final output of the `-fd` parameter will list the shared fusion breakpoints. For example, when testing with *Capsella rubella*, the `-fd` parameter output was:

The following are the shared fusion breakpoints:
AK1_1, AK1_2

This shows that *Capsella rubella* has these two shared fusion breakpoints: AK1_1 and AK1_2. If there is no such output, it means these fusion breakpoints are absent in the tested species.

![Dotplot for Thlaspi arvense](<./data/tar1s_fusion_breakpoints.dotplot.order.png>)

The following are the shared fusion breakpoints:
AK17

This indicates that *Thlaspi arvense* has the shared fusion breakpoint: AK17.