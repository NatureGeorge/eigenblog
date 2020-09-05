---
title: Record for PDBe API Issues
author: Zefeng Zhu
date: 2020-09-04 17:39:51 +0800
categories: [Notes, Research]
tags: [pdbe, api, issue]
---

## Basic Information

> This Gist is a personal note that recording those issues encountered during my use cases.

## API References

### PDBe REST API

* <https://www.ebi.ac.uk/pdbe/api/doc/pdb.html>
* <https://www.ebi.ac.uk/pdbe/api/doc/pisa.html>
* <https://www.ebi.ac.uk/pdbe/api/doc/sifts.html>

### PDBe Graph API

> Neo4j Graph DataBase

* <https://www.ebi.ac.uk/pdbe/graph-api/pdbe_doc/>

### PDBe CoordinateServer API

* <https://www.ebi.ac.uk/pdbe/coordinates/index.html>

### PDBe ModelServer API

* <https://www.ebi.ac.uk/pdbe/model-server/>

## Chain Identifier Related

* `mmCIF`'s Internal ID Synonym
    * Named: `'asym chain id'`
    * `struct_asym_id` (implement by [PDBe REST API](https://www.ebi.ac.uk/pdbe/api/doc/pdb.html))
    * `label_asym_id` (implement by [PDBe ModelServer API](https://www.ebi.ac.uk/pdbe/model-server/))
    * `asymId ` (implement by [PDBe CoordinateServer API](https://www.ebi.ac.uk/pdbe/coordinates/index.html))

* `PDB` Format, 'traditional' ID Synonym
    * Named: `'auth chain id'`
    * `chain_id` (implement by [PDBe REST API](https://www.ebi.ac.uk/pdbe/api/doc/pdb.html))
    * `auth_asym_id` (implement by [PDBe ModelServer API](https://www.ebi.ac.uk/pdbe/model-server/))
    * `authAsymId ` (implement by [PDBe CoordinateServer API](https://www.ebi.ac.uk/pdbe/coordinates/index.html))

### Related Issues

* <https://github.com/biojava/biojava/issues/469>: Add better support for asym chain ids as well as auth ids
    * <https://github.com/biojava/biojava/pull/479>: Refactoring of structure data model

## Biological Assemblies Related

### Issue: `struct_asym_id` in assembly/ `in_chains` generate new `struct_asym_id`

Example:

<table>
	<thead>
		<tr>
			<td>pdb_id</td>
			<td>entity_id</td>
			<td>in_struct_asyms</td>
			<td>molecule_type</td>
		</tr>
	</thead>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>["A","B","C","D"]</td>
		<td>polypeptide(L)</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>["E","JA","Q","Z"]</td>
		<td>bound</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>["AA","BA","CA","DA","EA","F","FA","G","GA","H","I","J","K","KA","L","LA","M","MA","N","NA","OA","PA","R","S","T","U","V","W"]</td>
		<td>bound</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>4</td>
		<td>["HA","O","QA","X"]</td>
		<td>bound</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>5</td>
		<td>["IA","P","RA","Y"]</td>
		<td>bound</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>6</td>
		<td>["SA","TA","UA","VA"]</td>
		<td>water</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pdb/entry/molecules/2o2q>

<table>
	<thead>
		<tr>
			<td>pdb_id</td>
			<td>assembly_id</td>
			<td>entity_id</td>
			<td>in_chains</td>
		</tr>
	</thead>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>1</td>
		<td>["A","B","C","D"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>2</td>
		<td>["E","JA","Q","Z"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>3</td>
		<td>["AA","BA","CA","DA","EA","F","FA","G","GA","H","I","J","K","KA","L","LA","M","MA","N","NA","OA","PA","R","S","T","U","V","W"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>4</td>
		<td>["HA","O","QA","X"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>5</td>
		<td>["IA","P","RA","Y"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>1</td>
		<td>6</td>
		<td>["SA","TA","UA","VA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>1</td>
		<td>["A","AA","B","BA","C","CB","D","DB"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>2</td>
		<td>["E","EA","JB","JC","Q","QA","Z","ZA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>3</td>
		<td>["AB","AC","BB","BC","CA","CC","DA","DC","EB","EC","F","FA","FB","FC","G","GA","GB","GC","H","HA","I","IA","J","JA","K","KA","KB","KC","L","LA","LB","LC","M","MA","MB","MC","N","NA","NB","NC","OB","OC","PB","PC","R","RA","S","SB","T","TB","U","UA","V","VA","W","WA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>4</td>
		<td>["HB","HC","O","OA","QB","QC","X","XA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>5</td>
		<td>["IB","IC","P","PA","RB","RC","Y","YA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>3</td>
		<td>6</td>
		<td>["SA","SC","TA","TC","UB","UC","VB","VC"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>1</td>
		<td>["A","AB","B","BB","C","CB","D","DB"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>2</td>
		<td>["E","EB","JA","JC","Q","QB","Z","ZA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>3</td>
		<td>["AA","AC","BA","BC","CA","CC","DA","DC","EA","EC","F","FA","FB","FC","G","GA","GB","GC","H","HB","I","IB","J","JB","K","KA","KB","KC","L","LA","LB","LC","M","MA","MB","MC","N","NA","NB","NC","OA","OC","PA","PC","R","RB","S","SB","T","TB","U","UB","V","VB","W","WA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>4</td>
		<td>["HA","HC","O","OB","QA","QC","X","XA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>5</td>
		<td>["IA","IC","P","PB","RA","RC","Y","YA"]</td>
	</tr>
	<tr>
		<td>2o2q</td>
		<td>2</td>
		<td>6</td>
		<td>["SA","SC","TA","TC","UA","UC","VA","VC"]</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pdb/entry/assembly/2o2q>

The `in_chains` column contains a new version of `struct_asym_id` for that particular assemble and entity. The rule seems to be like this: In a specified assemble, for those original chains, keep their original `struct_asym_id`; for those replicated chains, generate new `struct_asym_id` that continues the original chain's `struct_asym_id` while not in collision with existing `struct_asym_id` in that assemble (i.e. assembly3's <entity3's PB PC><entity5's PA>, assembly2's <entity3's PA PC><entity5's PB>). 

And I found that the ID of chain provided by PDBe PISA API (i.e. <https://www.ebi.ac.uk/pdbe/api/pisa/interfacelist/2o2q/3>) are corresponding to the above ID (at least in my usage cases):

<table>
	<thead>
		<tr>
			<td>structure_1.range</td>
			<td>structure_2.range</td>
			<td>interface_id</td>
		</tr>
	</thead>
	<tr>
		<td>D</td>
		<td>C</td>
		<td>1</td>
	</tr>
	<tr>
		<td>DB</td>
		<td>CB</td>
		<td>2</td>
	</tr>
	<tr>
		<td>[GOL]YA:2</td>
		<td>BA</td>
		<td>20</td>
	</tr>
	<tr>
		<td>[GOL]PA:1</td>
		<td>AA</td>
		<td>22</td>
	</tr>
</table>

Since I can get all the `struct_asym_id` in the asymmetric unit and their replication result from mmCIF's `_pdbx_struct_assembly_gen` and `_pdbx_struct_oper_list` and I can infer their model_id and ranking:


<table>
	<thead>
		<tr>
			<td>assembly_id</td>
			<td>struct_asym_id</td>
			<td>model_id</td>
			<td>asym_id_rank</td>
			<td>oper_expression</td>
			<td>symmetry_operation</td>
		</tr>
	</thead>
	<tr>
		<td>2</td>
		<td>P</td>
		<td>1</td>
		<td>1</td>
		<td>["1"]</td>
		<td>["x,y,z"]</td>
	</tr>
	<tr>
		<td>2</td>
		<td>P</td>
		<td>2</td>
		<td>2</td>
		<td>["2"]</td>
		<td>["-x+1,y,-z+1"]</td>
	</tr>
	<tr>
		<td>2</td>
		<td>PA</td>
		<td>1</td>
		<td>1</td>
		<td>["1"]</td>
		<td>["x,y,z"]</td>
	</tr>
	<tr>
		<td>2</td>
		<td>PA</td>
		<td>2</td>
		<td>2</td>
		<td>["2"]</td>
		<td>["-x+1,y,-z+1"]</td>
	</tr>
	<tr>
		<td>3</td>
		<td>P</td>
		<td>1</td>
		<td>1</td>
		<td>["1"]</td>
		<td>["x,y,z"]</td>
	</tr>
	<tr>
		<td>3</td>
		<td>P</td>
		<td>2</td>
		<td>2</td>
		<td>["3"]</td>
		<td>["-x+1,y,-z"]</td>
	</tr>
	<tr>
		<td>3</td>
		<td>PA</td>
		<td>3</td>
		<td>1</td>
		<td>["4"]</td>
		<td>["x,y,z-1"]</td>
	</tr>
	<tr>
		<td>3</td>
		<td>PA</td>
		<td>4</td>
		<td>2</td>
		<td>["2"]</td>
		<td>["-x+1,y,-z+1"]</td>
	</tr>
</table>

I would like to apply the automatic rule to generate corresponding new struct_asym_id for a specified assemble and map them with the interfacelist provided by PISA API.

The issue is, what is the automatic rule?

### Related Issues

* <https://github.com/biojava/biojava/issues/220>: Better support for symmetry in the Structure model
* <https://github.com/biojava/biojava/issues/801>: Biological assembly expansion: chain ids should contain both operator ids in binary expression case
    * <https://github.com/biojava/biojava/pull/802>: Assembly chain ids for cases with composed operators in assembly expansion


## Carbohydrate  Related

During my implementation of the REST calls related to PISA service, I found some 'outdated' information caused by the recent Carbohydrate Remediation Project. For instance, the chain identifiers of those carbohydrate polymers have now been regenerated in the current wwPDB archive file and also affect some other chain instance's id.

Examples: 

* 2wmg: NAGs, FUCs, GALs used to have their own struct_asym_id but now are aggregated into one single chain instance
* 1wdq: MAL -> GLC
* 2aw3: other chain instance's struct_asym_id are also re-generated

I would like to know whether the PISA dataset would 'update' to keep pace with the Carbohydrate Remediation Project or not.

### Externel Links
* [The Carbohydrate Remediation Project](https://www.wwpdb.org/documentation/carbohydrate-remediation)
* <https://github.com/pdbxmmcifwg/carbohydrate-extension>

### Issue:  Outdated PISA dataset (e.g `2wmg`)?

<table>
	<thead>
		<tr>
			<td>entity_id</td>
			<td>in_chains</td>
			<td>in_struct_asyms</td>
			<td>length</td>
			<td>molecule_name</td>
			<td>molecule_type</td>
			<td>number_of_copies</td>
			<td>pdb_id</td>
		</tr>
	</thead>
	<tr>
		<td>1</td>
		<td>["A"]</td>
		<td>["A"]</td>
		<td>581</td>
		<td>["F5/8 type C domain-containing protein"]</td>
		<td>polypeptide(L)</td>
		<td>1</td>
		<td>2wmg</td>
	</tr>
	<tr>
		<td>2</td>
		<td>["B"]</td>
		<td>["B"]</td>
		<td></td>
		<td>["alpha-L-fucopyranose-(1-2)-beta-D-galactopyranose-(1-4)-[alpha-L-fucopyranose-(1-3)]2-acetamido-2-deoxy-beta-D-glucopyranose"]</td>
		<td>carbohydrate polymer</td>
		<td>4</td>
		<td>2wmg</td>
	</tr>
	<tr>
		<td>3</td>
		<td>["A"]</td>
		<td>["C"]</td>
		<td></td>
		<td>["water"]</td>
		<td>water</td>
		<td>334</td>
		<td>2wmg</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pdb/entry/molecules/2o2q>

<table>
	<thead>
		<tr>
			<td>structure_1.range</td>
			<td>structure_2.range</td>
			<td>css</td>
			<td>delta_g_interface</td>
			<td>structure_2.symmetry_operator</td>
		</tr>
	</thead>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.372</td>
		<td>x-1/2,-y+1/2,-z+1</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.906674</td>
		<td>-x+1,y-1/2,-z+1/2</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.105273</td>
		<td>x-1,y,z</td>
	</tr>
	<tr>
		<td>[NAG]D:1592</td>
		<td>A</td>
		<td>0</td>
		<td>2.37148</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[FUC]B:1590</td>
		<td>A</td>
		<td>0</td>
		<td>1.67611</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[GAL]C:1591</td>
		<td>A</td>
		<td>0</td>
		<td>2.43961</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[FUC]E:1593</td>
		<td>A</td>
		<td>0</td>
		<td>1.46851</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.656549</td>
		<td>-x,y-1/2,-z+1/2</td>
	</tr>
	<tr>
		<td>[NAG]D:1592</td>
		<td>[GAL]C:1591</td>
		<td>0</td>
		<td>1.25173</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[NAG]D:1592</td>
		<td>[FUC]E:1593</td>
		<td>0</td>
		<td>2.05322</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[GAL]C:1591</td>
		<td>[FUC]E:1593</td>
		<td>0</td>
		<td>1.44298</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[GAL]C:1591</td>
		<td>[FUC]B:1590</td>
		<td>0</td>
		<td>1.95678</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[FUC]E:1593</td>
		<td>[FUC]B:1590</td>
		<td>0</td>
		<td>1.98391</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.497071</td>
		<td>-x+1/2,-y+1,z-1/2</td>
	</tr>
	<tr>
		<td>[NAG]D:1592</td>
		<td>[FUC]B:1590</td>
		<td>0</td>
		<td>0.398597</td>
		<td>x,y,z</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pisa/interfacelist/2o2q/0>

![image](https://user-images.githubusercontent.com/43134199/92234724-20986080-eee5-11ea-8ccc-80d6bd00aa65.png)

#### Look into wwPDB Archive

<http://ftp-versioned.wwpdb.org/pdb_versioned/data/entries/wm/pdb_00002wmg/pdb_00002wmg_xyz_v1-2.cif.gz>:

```cif
HETATM 4499 C C1  . FUC B 2 .   ? 21.448  30.534 21.369 1.00 26.82 ? 1590 FUC A C1  1 
HETATM 4500 C C2  . FUC B 2 .   ? 20.079  29.853 21.519 1.00 26.87 ? 1590 FUC A C2  1 
HETATM 4501 C C3  . FUC B 2 .   ? 19.240  30.667 22.520 1.00 26.92 ? 1590 FUC A C3  1 
HETATM 4502 C C4  . FUC B 2 .   ? 19.073  32.098 21.990 1.00 26.06 ? 1590 FUC A C4  1 
HETATM 4503 C C5  . FUC B 2 .   ? 20.468  32.708 21.723 1.00 27.34 ? 1590 FUC A C5  1 
HETATM 4504 C C6  . FUC B 2 .   ? 20.412  34.110 21.105 1.00 27.22 ? 1590 FUC A C6  1 
HETATM 4505 O O2  . FUC B 2 .   ? 20.211  28.500 21.918 1.00 27.52 ? 1590 FUC A O2  1 
HETATM 4506 O O3  . FUC B 2 .   ? 17.984  30.074 22.824 1.00 27.23 ? 1590 FUC A O3  1 
HETATM 4507 O O4  . FUC B 2 .   ? 18.250  32.100 20.836 1.00 22.93 ? 1590 FUC A O4  1 
HETATM 4508 O O5  . FUC B 2 .   ? 21.275  31.860 20.900 1.00 27.65 ? 1590 FUC A O5  1 
HETATM 4509 C C1  . GAL C 3 .   ? 23.982  31.388 23.800 1.00 28.73 ? 1591 GAL A C1  1 
HETATM 4510 C C2  . GAL C 3 .   ? 23.540  30.520 22.595 1.00 27.78 ? 1591 GAL A C2  1 
HETATM 4511 C C3  . GAL C 3 .   ? 24.078  29.067 22.652 1.00 26.81 ? 1591 GAL A C3  1 
HETATM 4512 C C4  . GAL C 3 .   ? 25.532  29.029 23.115 1.00 27.03 ? 1591 GAL A C4  1 
HETATM 4513 C C5  . GAL C 3 .   ? 25.614  29.712 24.483 1.00 27.02 ? 1591 GAL A C5  1 
HETATM 4514 C C6  . GAL C 3 .   ? 27.020  29.735 25.059 1.00 26.52 ? 1591 GAL A C6  1 
HETATM 4515 O O2  . GAL C 3 .   ? 22.125  30.492 22.645 1.00 28.02 ? 1591 GAL A O2  1 
HETATM 4516 O O3  . GAL C 3 .   ? 23.954  28.363 21.425 1.00 24.40 ? 1591 GAL A O3  1 
HETATM 4517 O O4  . GAL C 3 .   ? 26.389  29.623 22.145 1.00 26.50 ? 1591 GAL A O4  1 
HETATM 4518 O O5  . GAL C 3 .   ? 25.240  31.061 24.376 1.00 28.14 ? 1591 GAL A O5  1 
HETATM 4519 O O6  . GAL C 3 .   ? 26.910  30.176 26.391 1.00 26.25 ? 1591 GAL A O6  1 
HETATM 4520 C C1  . NAG D 4 .   ? 26.561  36.013 23.263 1.00 35.36 ? 1592 NAG A C1  1 
HETATM 4521 C C2  . NAG D 4 .   ? 26.575  35.095 22.025 1.00 36.05 ? 1592 NAG A C2  1 
HETATM 4522 C C3  . NAG D 4 .   ? 25.434  34.037 22.061 1.00 35.34 ? 1592 NAG A C3  1 
HETATM 4523 C C4  . NAG D 4 .   ? 25.202  33.442 23.473 1.00 33.52 ? 1592 NAG A C4  1 
HETATM 4524 C C5  . NAG D 4 .   ? 25.377  34.458 24.623 1.00 34.35 ? 1592 NAG A C5  1 
HETATM 4525 C C6  . NAG D 4 .   ? 25.450  33.779 25.994 1.00 34.59 ? 1592 NAG A C6  1 
HETATM 4526 C C7  . NAG D 4 .   ? 27.588  36.194 20.056 1.00 37.78 ? 1592 NAG A C7  1 
HETATM 4527 C C8  . NAG D 4 .   ? 27.375  37.110 18.885 1.00 37.94 ? 1592 NAG A C8  1 
HETATM 4528 N N2  . NAG D 4 .   ? 26.519  35.920 20.818 1.00 36.64 ? 1592 NAG A N2  1 
HETATM 4529 O O1  . NAG D 4 .   ? 27.692  36.861 23.312 1.00 35.52 ? 1592 NAG A O1  1 
HETATM 4530 O O3  . NAG D 4 .   ? 25.589  32.903 21.193 1.00 35.46 ? 1592 NAG A O3  1 
HETATM 4531 O O4  . NAG D 4 .   ? 23.913  32.818 23.523 1.00 31.47 ? 1592 NAG A O4  1 
HETATM 4532 O O5  . NAG D 4 .   ? 26.546  35.245 24.449 1.00 35.47 ? 1592 NAG A O5  1 
HETATM 4533 O O6  . NAG D 4 .   ? 25.836  34.672 27.021 1.00 34.34 ? 1592 NAG A O6  1 
HETATM 4534 O O7  . NAG D 4 .   ? 28.715  35.736 20.261 1.00 38.61 ? 1592 NAG A O7  1 
HETATM 4535 C C1  . FUC E 2 .   ? 25.747  33.116 19.759 1.00 35.63 ? 1593 FUC A C1  1 
HETATM 4536 C C2  . FUC E 2 .   ? 25.953  31.766 19.050 1.00 35.47 ? 1593 FUC A C2  1 
HETATM 4537 C C3  . FUC E 2 .   ? 24.620  31.032 18.831 1.00 35.29 ? 1593 FUC A C3  1 
HETATM 4538 C C4  . FUC E 2 .   ? 23.605  31.935 18.138 1.00 34.97 ? 1593 FUC A C4  1 
HETATM 4539 C C5  . FUC E 2 .   ? 23.486  33.285 18.870 1.00 35.29 ? 1593 FUC A C5  1 
HETATM 4540 C C6  . FUC E 2 .   ? 22.572  34.252 18.123 1.00 33.61 ? 1593 FUC A C6  1 
HETATM 4541 O O2  . FUC E 2 .   ? 26.834  30.951 19.799 1.00 34.72 ? 1593 FUC A O2  1 
HETATM 4542 O O3  . FUC E 2 .   ? 24.793  29.854 18.066 1.00 34.23 ? 1593 FUC A O3  1 
HETATM 4543 O O4  . FUC E 2 .   ? 23.980  32.080 16.780 1.00 35.78 ? 1593 FUC A O4  1 
HETATM 4544 O O5  . FUC E 2 .   ? 24.751  33.904 19.097 1.00 35.04 ? 1593 FUC A O5  1
```

<http://ftp-versioned.wwpdb.org/pdb_versioned/data/entries/wm/pdb_00002wmg/pdb_00002wmg_xyz_v2-0.cif.gz>:

```cif
HETATM 4499 C C1 . NAG B 2 . ? 26.561 36.013 23.263 1.00 35.36 ? 1 NAG B C1 1 1
HETATM 4500 C C2 . NAG B 2 . ? 26.575 35.095 22.025 1.00 36.05 ? 1 NAG B C2 1 1
HETATM 4501 C C3 . NAG B 2 . ? 25.434 34.037 22.061 1.00 35.34 ? 1 NAG B C3 1 1
HETATM 4502 C C4 . NAG B 2 . ? 25.202 33.442 23.473 1.00 33.52 ? 1 NAG B C4 1 1
HETATM 4503 C C5 . NAG B 2 . ? 25.377 34.458 24.623 1.00 34.35 ? 1 NAG B C5 1 1
HETATM 4504 C C6 . NAG B 2 . ? 25.450 33.779 25.994 1.00 34.59 ? 1 NAG B C6 1 1
HETATM 4505 C C7 . NAG B 2 . ? 27.588 36.194 20.056 1.00 37.78 ? 1 NAG B C7 1 1
HETATM 4506 C C8 . NAG B 2 . ? 27.375 37.110 18.885 1.00 37.94 ? 1 NAG B C8 1 1
HETATM 4507 N N2 . NAG B 2 . ? 26.519 35.920 20.818 1.00 36.64 ? 1 NAG B N2 1 1
HETATM 4508 O O1 . NAG B 2 . ? 27.692 36.861 23.312 1.00 35.52 ? 1 NAG B O1 1 1
HETATM 4509 O O3 . NAG B 2 . ? 25.589 32.903 21.193 1.00 35.46 ? 1 NAG B O3 1 1
HETATM 4510 O O4 . NAG B 2 . ? 23.913 32.818 23.523 1.00 31.47 ? 1 NAG B O4 1 1
HETATM 4511 O O5 . NAG B 2 . ? 26.546 35.245 24.449 1.00 35.47 ? 1 NAG B O5 1 1
HETATM 4512 O O6 . NAG B 2 . ? 25.836 34.672 27.021 1.00 34.34 ? 1 NAG B O6 1 1
HETATM 4513 O O7 . NAG B 2 . ? 28.715 35.736 20.261 1.00 38.61 ? 1 NAG B O7 1 1
HETATM 4514 C C1 . GAL B 2 . ? 23.982 31.388 23.800 1.00 28.73 ? 2 GAL B C1 1 2
HETATM 4515 C C2 . GAL B 2 . ? 23.540 30.520 22.595 1.00 27.78 ? 2 GAL B C2 1 2
HETATM 4516 C C3 . GAL B 2 . ? 24.078 29.067 22.652 1.00 26.81 ? 2 GAL B C3 1 2
HETATM 4517 C C4 . GAL B 2 . ? 25.532 29.029 23.115 1.00 27.03 ? 2 GAL B C4 1 2
HETATM 4518 C C5 . GAL B 2 . ? 25.614 29.712 24.483 1.00 27.02 ? 2 GAL B C5 1 2
HETATM 4519 C C6 . GAL B 2 . ? 27.020 29.735 25.059 1.00 26.52 ? 2 GAL B C6 1 2
HETATM 4520 O O2 . GAL B 2 . ? 22.125 30.492 22.645 1.00 28.02 ? 2 GAL B O2 1 2
HETATM 4521 O O3 . GAL B 2 . ? 23.954 28.363 21.425 1.00 24.40 ? 2 GAL B O3 1 2
HETATM 4522 O O4 . GAL B 2 . ? 26.389 29.623 22.145 1.00 26.50 ? 2 GAL B O4 1 2
HETATM 4523 O O5 . GAL B 2 . ? 25.240 31.061 24.376 1.00 28.14 ? 2 GAL B O5 1 2
HETATM 4524 O O6 . GAL B 2 . ? 26.910 30.176 26.391 1.00 26.25 ? 2 GAL B O6 1 2
HETATM 4525 C C1 . FUC B 2 . ? 21.448 30.534 21.369 1.00 26.82 ? 3 FUC B C1 1 3
HETATM 4526 C C2 . FUC B 2 . ? 20.079 29.853 21.519 1.00 26.87 ? 3 FUC B C2 1 3
HETATM 4527 C C3 . FUC B 2 . ? 19.240 30.667 22.520 1.00 26.92 ? 3 FUC B C3 1 3
HETATM 4528 C C4 . FUC B 2 . ? 19.073 32.098 21.990 1.00 26.06 ? 3 FUC B C4 1 3
HETATM 4529 C C5 . FUC B 2 . ? 20.468 32.708 21.723 1.00 27.34 ? 3 FUC B C5 1 3
HETATM 4530 C C6 . FUC B 2 . ? 20.412 34.110 21.105 1.00 27.22 ? 3 FUC B C6 1 3
HETATM 4531 O O2 . FUC B 2 . ? 20.211 28.500 21.918 1.00 27.52 ? 3 FUC B O2 1 3
HETATM 4532 O O3 . FUC B 2 . ? 17.984 30.074 22.824 1.00 27.23 ? 3 FUC B O3 1 3
HETATM 4533 O O4 . FUC B 2 . ? 18.250 32.100 20.836 1.00 22.93 ? 3 FUC B O4 1 3
HETATM 4534 O O5 . FUC B 2 . ? 21.275 31.860 20.900 1.00 27.65 ? 3 FUC B O5 1 3
HETATM 4535 C C1 . FUC B 2 . ? 25.747 33.116 19.759 1.00 35.63 ? 4 FUC B C1 1 4
HETATM 4536 C C2 . FUC B 2 . ? 25.953 31.766 19.050 1.00 35.47 ? 4 FUC B C2 1 4
HETATM 4537 C C3 . FUC B 2 . ? 24.620 31.032 18.831 1.00 35.29 ? 4 FUC B C3 1 4
HETATM 4538 C C4 . FUC B 2 . ? 23.605 31.935 18.138 1.00 34.97 ? 4 FUC B C4 1 4
HETATM 4539 C C5 . FUC B 2 . ? 23.486 33.285 18.870 1.00 35.29 ? 4 FUC B C5 1 4
HETATM 4540 C C6 . FUC B 2 . ? 22.572 34.252 18.123 1.00 33.61 ? 4 FUC B C6 1 4
HETATM 4541 O O2 . FUC B 2 . ? 26.834 30.951 19.799 1.00 34.72 ? 4 FUC B O2 1 4
HETATM 4542 O O3 . FUC B 2 . ? 24.793 29.854 18.066 1.00 34.23 ? 4 FUC B O3 1 4
HETATM 4543 O O4 . FUC B 2 . ? 23.980 32.080 16.780 1.00 35.78 ? 4 FUC B O4 1 4
HETATM 4544 O O5 . FUC B 2 . ? 24.751 33.904 19.097 1.00 35.04 ? 4 FUC B O5 1 4
```

### Issue: Outdated PISA dataset (e.g 1wdq)?

<table>
	<thead>
		<tr>
			<td>entity_id</td>
			<td>gene_name</td>
			<td>in_chains</td>
			<td>in_struct_asyms</td>
			<td>length</td>
			<td>molecule_name</td>
			<td>molecule_type</td>
			<td>number_of_copies</td>
			<td>pdb_id</td>
		</tr>
	</thead>
	<tr>
		<td>1</td>
		<td>["BMY1"]</td>
		<td>["A"]</td>
		<td>["A"]</td>
		<td>495</td>
		<td>["Beta-amylase"]</td>
		<td>polypeptide(L)</td>
		<td>1</td>
		<td>1wdq</td>
	</tr>
	<tr>
		<td>2</td>
		<td></td>
		<td>["B","C"]</td>
		<td>["B","C"]</td>
		<td></td>
		<td>["alpha-D-glucopyranose-(1-4)-alpha-D-glucopyranose"]</td>
		<td>carbohydrate polymer</td>
		<td>4</td>
		<td>1wdq</td>
	</tr>
	<tr>
		<td>3</td>
		<td></td>
		<td>["A"]</td>
		<td>["D","E","F","G","H"]</td>
		<td></td>
		<td>["SULFATE ION"]</td>
		<td>bound</td>
		<td>5</td>
		<td>1wdq</td>
	</tr>
	<tr>
		<td>4</td>
		<td></td>
		<td>["A"]</td>
		<td>["I"]</td>
		<td></td>
		<td>["water"]</td>
		<td>water</td>
		<td>810</td>
		<td>1wdq</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pdb/entry/molecules/1wdq>

<table>
	<thead>
		<tr>
			<td>structure_1.range</td>
			<td>structure_2.range</td>
			<td>css</td>
			<td>delta_g_interface</td>
			<td>structure_2.symmetry_operator</td>
		</tr>
	</thead>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-3.79762</td>
		<td>-x+1,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>3.33369</td>
		<td>y,x,-z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-1.49174</td>
		<td>-y+1,x-y+1,z+1/3</td>
	</tr>
	<tr>
		<td>[MAL]C:498</td>
		<td>A</td>
		<td>0.00698</td>
		<td>-0.294266</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[MAL]B:496</td>
		<td>A</td>
		<td>0.005702</td>
		<td>0.047561</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-1.21817</td>
		<td>-x,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>[SO4]D:2000</td>
		<td>A</td>
		<td>0.018727</td>
		<td>-9.73464</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[SO4]E:2001</td>
		<td>A</td>
		<td>0.022233</td>
		<td>-11.8895</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[SO4]F:2002</td>
		<td>A</td>
		<td>0.015819</td>
		<td>-8.83509</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[SO4]G:2003</td>
		<td>A</td>
		<td>0.016383</td>
		<td>-8.73819</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>[SO4]H:2004</td>
		<td>0.012637</td>
		<td>-7.32349</td>
		<td>-y+1,x-y+1,z+1/3</td>
	</tr>
	<tr>
		<td>[MAL]C:498</td>
		<td>[MAL]B:496</td>
		<td>0.00027</td>
		<td>-0.165906</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>[SO4]E:2001</td>
		<td>A</td>
		<td>0</td>
		<td>-4.81706</td>
		<td>-y+1,x-y+1,z+1/3</td>
	</tr>
	<tr>
		<td>[SO4]H:2004</td>
		<td>A</td>
		<td>0</td>
		<td>-3.47703</td>
		<td>x,y,z</td>
	</tr>
	<tr>
		<td>A</td>
		<td>[SO4]H:2004</td>
		<td>0</td>
		<td>-5.19629</td>
		<td>-x+1,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>A</td>
		<td>[SO4]D:2000</td>
		<td>0</td>
		<td>-3.3604</td>
		<td>-x+1,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>[SO4]G:2003</td>
		<td>A</td>
		<td>0</td>
		<td>-2.768</td>
		<td>-x,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>A</td>
		<td>A</td>
		<td>0</td>
		<td>-0.079932</td>
		<td>x-y,-y+1,-z+2/3</td>
	</tr>
	<tr>
		<td>A</td>
		<td>[SO4]E:2001</td>
		<td>0</td>
		<td>-0.489989</td>
		<td>-x+1,-x+y,-z+1/3</td>
	</tr>
	<tr>
		<td>[SO4]E:2001</td>
		<td>[SO4]D:2000</td>
		<td>0.001249</td>
		<td>-0.767584</td>
		<td>x,y,z</td>
	</tr>
</table>

> provided by <https://www.ebi.ac.uk/pdbe/api/pisa/interfacelist/1wdq/0>

#### Look into wwPDB Archive

http://ftp-versioned.wwpdb.org/pdb_versioned/data/entries/wd/pdb_00001wdq/pdb_00001wdq_xyz_v1-2.cif.gz:

```cif
HETATM 3960 C C1    . MAL B 2 .   ? 0.923   28.692 21.983 1.00 15.32  ? 496  MAL A C1    1 
HETATM 3961 C C2    . MAL B 2 .   ? 1.717   27.407 21.659 1.00 15.58  ? 496  MAL A C2    1 
HETATM 3962 C C3    . MAL B 2 .   ? 3.103   27.861 21.193 1.00 13.17  ? 496  MAL A C3    1 
HETATM 3963 C C4    . MAL B 2 .   ? 2.942   28.664 19.909 1.00 11.86  ? 496  MAL A C4    1 
HETATM 3964 C C5    . MAL B 2 .   ? 2.087   29.904 20.193 1.00 14.42  ? 496  MAL A C5    1 
HETATM 3965 C C6    . MAL B 2 .   ? 1.720   30.758 18.985 1.00 14.61  ? 496  MAL A C6    1 
HETATM 3966 O O1    . MAL B 2 .   ? 1.510   29.412 23.067 1.00 18.37  ? 496  MAL A O1    1 
HETATM 3967 O O2    . MAL B 2 .   ? 1.862   26.661 22.871 1.00 14.82  ? 496  MAL A O2    1 
HETATM 3968 O O3    . MAL B 2 .   ? 3.882   26.682 20.953 1.00 12.67  ? 496  MAL A O3    1 
HETATM 3969 O O4    . MAL B 2 .   ? 4.256   29.108 19.514 1.00 10.20  ? 496  MAL A O4    1 
HETATM 3970 O O5    . MAL B 2 .   ? 0.836   29.453 20.754 1.00 15.61  ? 496  MAL A O5    1 
HETATM 3971 O O6    . MAL B 2 .   ? 0.983   30.021 17.980 1.00 14.23  ? 496  MAL A O6    1 
HETATM 3972 C "C1'" . MAL B 2 .   ? -0.216  32.841 24.571 1.00 30.41  ? 496  MAL A "C1'" 1 
HETATM 3973 C "C2'" . MAL B 2 .   ? -0.638  31.588 25.198 1.00 18.61  ? 496  MAL A "C2'" 1 
HETATM 3974 C "C3'" . MAL B 2 .   ? 0.057   30.300 24.820 1.00 23.53  ? 496  MAL A "C3'" 1 
HETATM 3975 C "C4'" . MAL B 2 .   ? 0.869   30.574 23.557 1.00 23.49  ? 496  MAL A "C4'" 1 
HETATM 3976 C "C5'" . MAL B 2 .   ? 1.885   31.667 23.948 1.00 19.64  ? 496  MAL A "C5'" 1 
HETATM 3977 C "C6'" . MAL B 2 .   ? 2.823   32.280 22.930 1.00 19.63  ? 496  MAL A "C6'" 1 
HETATM 3978 O "O1'" . MAL B 2 .   ? -0.827  34.119 24.372 1.00 17.28  ? 496  MAL A "O1'" 1 
HETATM 3979 O "O2'" . MAL B 2 .   ? -1.067  31.400 26.525 1.00 29.30  ? 496  MAL A "O2'" 1 
HETATM 3980 O "O3'" . MAL B 2 .   ? -0.875  29.278 24.527 1.00 23.41  ? 496  MAL A "O3'" 1 
HETATM 3981 O "O5'" . MAL B 2 .   ? 1.198   32.842 24.453 1.00 23.65  ? 496  MAL A "O5'" 1 
HETATM 3982 O "O6'" . MAL B 2 .   ? 2.048   32.851 21.795 1.00 14.06  ? 496  MAL A "O6'" 1 
HETATM 3983 C C1    . MAL C 2 .   ? 0.637   30.934 30.966 1.00 11.83  ? 498  MAL A C1    1 
HETATM 3984 C C2    . MAL C 2 .   ? 0.877   32.416 30.689 1.00 11.42  ? 498  MAL A C2    1 
HETATM 3985 C C3    . MAL C 2 .   ? 0.888   32.751 29.223 1.00 11.41  ? 498  MAL A C3    1 
HETATM 3986 C C4    . MAL C 2 .   ? 1.807   31.804 28.458 1.00 11.73  ? 498  MAL A C4    1 
HETATM 3987 C C5    . MAL C 2 .   ? 1.279   30.365 28.741 1.00 11.17  ? 498  MAL A C5    1 
HETATM 3988 C C6    . MAL C 2 .   ? 2.220   29.309 28.065 1.00 12.00  ? 498  MAL A C6    1 
HETATM 3989 O O1    . MAL C 2 .   ? -0.715  30.580 30.804 1.00 12.37  ? 498  MAL A O1    1 
HETATM 3990 O O2    . MAL C 2 .   ? -0.104  33.179 31.390 1.00 11.56  ? 498  MAL A O2    1 
HETATM 3991 O O3    . MAL C 2 .   ? 1.332   34.081 29.100 1.00 10.77  ? 498  MAL A O3    1 
HETATM 3992 O O4    . MAL C 2 .   ? 1.656   32.017 27.022 1.00 10.76  ? 498  MAL A O4    1 
HETATM 3993 O O5    . MAL C 2 .   ? 1.442   30.111 30.145 1.00 12.17  ? 498  MAL A O5    1 
HETATM 3994 O O6    . MAL C 2 .   ? 3.616   29.501 28.336 1.00 10.59  ? 498  MAL A O6    1 
HETATM 3995 C "C1'" . MAL C 2 .   ? -3.464  28.664 33.250 1.00 14.75  ? 498  MAL A "C1'" 1 
HETATM 3996 C "C2'" . MAL C 2 .   ? -3.231  30.138 33.496 1.00 14.62  ? 498  MAL A "C2'" 1 
HETATM 3997 C "C3'" . MAL C 2 .   ? -2.624  30.770 32.246 1.00 13.99  ? 498  MAL A "C3'" 1 
HETATM 3998 C "C4'" . MAL C 2 .   ? -1.351  29.988 31.943 1.00 13.33  ? 498  MAL A "C4'" 1 
HETATM 3999 C "C5'" . MAL C 2 .   ? -1.675  28.518 31.699 1.00 15.36  ? 498  MAL A "C5'" 1 
HETATM 4000 C "C6'" . MAL C 2 .   ? -0.423  27.660 31.535 1.00 15.60  ? 498  MAL A "C6'" 1 
HETATM 4001 O "O1'" A MAL C 2 .   ? -4.466  28.531 32.350 0.64 17.30  ? 498  MAL A "O1'" 1 
HETATM 4002 O "O1'" B MAL C 2 .   ? -4.214  27.971 34.112 0.36 17.59  ? 498  MAL A "O1'" 1 
HETATM 4003 O "O2'" . MAL C 2 .   ? -4.420  30.837 33.809 1.00 17.68  ? 498  MAL A "O2'" 1 
HETATM 4004 O "O3'" . MAL C 2 .   ? -2.284  32.101 32.588 1.00 13.18  ? 498  MAL A "O3'" 1 
HETATM 4005 O "O5'" . MAL C 2 .   ? -2.283  28.001 32.888 1.00 15.86  ? 498  MAL A "O5'" 1 
HETATM 4006 O "O6'" . MAL C 2 .   ? -0.732  26.334 31.256 1.00 17.79  ? 498  MAL A "O6'" 1 
```

http://ftp-versioned.wwpdb.org/pdb_versioned/data/entries/wd/pdb_00001wdq/pdb_00001wdq_xyz_v2-0.cif.gz:

```cif
HETATM 3960 C C1  . GLC B 2 .   ? -0.216  32.841 24.571 1.00 30.41  ? 1    GLC B C1  1 
HETATM 3961 C C2  . GLC B 2 .   ? -0.638  31.588 25.198 1.00 18.61  ? 1    GLC B C2  1 
HETATM 3962 C C3  . GLC B 2 .   ? 0.057   30.300 24.820 1.00 23.53  ? 1    GLC B C3  1 
HETATM 3963 C C4  . GLC B 2 .   ? 0.869   30.574 23.557 1.00 23.49  ? 1    GLC B C4  1 
HETATM 3964 C C5  . GLC B 2 .   ? 1.885   31.667 23.948 1.00 19.64  ? 1    GLC B C5  1 
HETATM 3965 C C6  . GLC B 2 .   ? 2.823   32.280 22.930 1.00 19.63  ? 1    GLC B C6  1 
HETATM 3966 O O1  . GLC B 2 .   ? -0.827  34.119 24.372 1.00 17.28  ? 1    GLC B O1  1 
HETATM 3967 O O2  . GLC B 2 .   ? -1.067  31.400 26.525 1.00 29.30  ? 1    GLC B O2  1 
HETATM 3968 O O3  . GLC B 2 .   ? -0.875  29.278 24.527 1.00 23.41  ? 1    GLC B O3  1 
HETATM 3969 O O4  . GLC B 2 .   ? 1.510   29.412 23.067 1.00 18.37  ? 1    GLC B O4  1 
HETATM 3970 O O5  . GLC B 2 .   ? 1.198   32.842 24.453 1.00 23.65  ? 1    GLC B O5  1 
HETATM 3971 O O6  . GLC B 2 .   ? 2.048   32.851 21.795 1.00 14.06  ? 1    GLC B O6  1 
HETATM 3972 C C1  . GLC B 2 .   ? 0.923   28.692 21.983 1.00 15.32  ? 2    GLC B C1  1 
HETATM 3973 C C2  . GLC B 2 .   ? 1.717   27.407 21.659 1.00 15.58  ? 2    GLC B C2  1 
HETATM 3974 C C3  . GLC B 2 .   ? 3.103   27.861 21.193 1.00 13.17  ? 2    GLC B C3  1 
HETATM 3975 C C4  . GLC B 2 .   ? 2.942   28.664 19.909 1.00 11.86  ? 2    GLC B C4  1 
HETATM 3976 C C5  . GLC B 2 .   ? 2.087   29.904 20.193 1.00 14.42  ? 2    GLC B C5  1 
HETATM 3977 C C6  . GLC B 2 .   ? 1.720   30.758 18.985 1.00 14.61  ? 2    GLC B C6  1 
HETATM 3978 O O2  . GLC B 2 .   ? 1.862   26.661 22.871 1.00 14.82  ? 2    GLC B O2  1 
HETATM 3979 O O3  . GLC B 2 .   ? 3.882   26.682 20.953 1.00 12.67  ? 2    GLC B O3  1 
HETATM 3980 O O4  . GLC B 2 .   ? 4.256   29.108 19.514 1.00 10.20  ? 2    GLC B O4  1 
HETATM 3981 O O5  . GLC B 2 .   ? 0.836   29.453 20.754 1.00 15.61  ? 2    GLC B O5  1 
HETATM 3982 O O6  . GLC B 2 .   ? 0.983   30.021 17.980 1.00 14.23  ? 2    GLC B O6  1 
HETATM 3983 C C1  . GLC C 2 .   ? -3.464  28.664 33.250 1.00 14.75  ? 1    GLC C C1  1 
HETATM 3984 C C2  . GLC C 2 .   ? -3.231  30.138 33.496 1.00 14.62  ? 1    GLC C C2  1 
HETATM 3985 C C3  . GLC C 2 .   ? -2.624  30.770 32.246 1.00 13.99  ? 1    GLC C C3  1 
HETATM 3986 C C4  . GLC C 2 .   ? -1.351  29.988 31.943 1.00 13.33  ? 1    GLC C C4  1 
HETATM 3987 C C5  . GLC C 2 .   ? -1.675  28.518 31.699 1.00 15.36  ? 1    GLC C C5  1 
HETATM 3988 C C6  . GLC C 2 .   ? -0.423  27.660 31.535 1.00 15.60  ? 1    GLC C C6  1 
HETATM 3989 O O1  A GLC C 2 .   ? -4.466  28.531 32.350 0.64 17.30  ? 1    GLC C O1  1 
HETATM 3990 O O1  B GLC C 2 .   ? -4.214  27.971 34.112 0.36 17.59  ? 1    GLC C O1  1 
HETATM 3991 O O2  . GLC C 2 .   ? -4.420  30.837 33.809 1.00 17.68  ? 1    GLC C O2  1 
HETATM 3992 O O3  . GLC C 2 .   ? -2.284  32.101 32.588 1.00 13.18  ? 1    GLC C O3  1 
HETATM 3993 O O4  . GLC C 2 .   ? -0.715  30.580 30.804 1.00 12.37  ? 1    GLC C O4  1 
HETATM 3994 O O5  . GLC C 2 .   ? -2.283  28.001 32.888 1.00 15.86  ? 1    GLC C O5  1 
HETATM 3995 O O6  . GLC C 2 .   ? -0.732  26.334 31.256 1.00 17.79  ? 1    GLC C O6  1 
HETATM 3996 C C1  . GLC C 2 .   ? 0.637   30.934 30.966 1.00 11.83  ? 2    GLC C C1  1 
HETATM 3997 C C2  . GLC C 2 .   ? 0.877   32.416 30.689 1.00 11.42  ? 2    GLC C C2  1 
HETATM 3998 C C3  . GLC C 2 .   ? 0.888   32.751 29.223 1.00 11.41  ? 2    GLC C C3  1 
HETATM 3999 C C4  . GLC C 2 .   ? 1.807   31.804 28.458 1.00 11.73  ? 2    GLC C C4  1 
HETATM 4000 C C5  . GLC C 2 .   ? 1.279   30.365 28.741 1.00 11.17  ? 2    GLC C C5  1 
HETATM 4001 C C6  . GLC C 2 .   ? 2.220   29.309 28.065 1.00 12.00  ? 2    GLC C C6  1 
HETATM 4002 O O2  . GLC C 2 .   ? -0.104  33.179 31.390 1.00 11.56  ? 2    GLC C O2  1 
HETATM 4003 O O3  . GLC C 2 .   ? 1.332   34.081 29.100 1.00 10.77  ? 2    GLC C O3  1 
HETATM 4004 O O4  . GLC C 2 .   ? 1.656   32.017 27.022 1.00 10.76  ? 2    GLC C O4  1 
HETATM 4005 O O5  . GLC C 2 .   ? 1.442   30.111 30.145 1.00 12.17  ? 2    GLC C O5  1 
HETATM 4006 O O6  . GLC C 2 .   ? 3.616   29.501 28.336 1.00 10.59  ? 2    GLC C O6  1 
```

<table>
<tr><td>pdb_00001wdq_xyz_v1-2.cif.gz</td><td>pdb_00001wdq_xyz_v2-0.cif.gz</td></tr>
<tr><td><img src="https://user-images.githubusercontent.com/43134199/92237863-aec31580-eeea-11ea-8e43-4bc89181fba2.png"></td>
<td><img src="https://user-images.githubusercontent.com/43134199/92237830-9fdc6300-eeea-11ea-8c20-f4f5582cdd9b.png"></td></tr>
</table>

### Issue: Outdated PISA dataset (e.g 2aw3)? - with outdated `struct_asym_id`

chem_comp_ids | entity_id | in_chains | in_struct_asyms | length | molecule_name | molecule_type | number_of_copies | pdb_id
-- | -- | -- | -- | -- | -- | -- | -- | --
  | 1 | ["A","B"] | ["A","B"] | 796 | ["Maltodextrin phosphorylase"] | polypeptide(L) | 2 | 2aw3
["BGC","GLC"] | 2 | ["C","D"] | ["C","D"] |   | ["alpha-D-glucopyranose-(1-4)-alpha-D-glucopyranose-(1-4)-alpha-D-glucopyranose-(1-4)-alpha-D-glucopyranose-(1-4)-beta-D-glucopyranose"] | carbohydrate polymer | 10 | 2aw3
["SO4"] | 3 | ["A","B"] | ["E","G"] |   | ["SULFATE ION"] | bound | 2 | 2aw3
["PLP"] | 4 | ["A","B"] | ["F","H"] |   | ["PYRIDOXAL-5'-PHOSPHATE"] | bound | 2 | 2aw3
["HOH"] | 5 | ["A","B"] | ["I","J"] |   | ["water"] | water | 1162 | 2aw3

> provided by <https://www.ebi.ac.uk/pdbe/api/pdb/entry/molecules/2aw3>

structure_1.range | structure_2.range | css | delta_g_interface | structure_2.symmetry_operator
-- | -- | -- | -- | --
B | A | 0.439004 | -19.4029 | x,y,z
B | A | 0 | -2.16533 | -x,y-1/2,-z+1/2
A | B | 0 | 1.03891 | -x,y-1/2,-z+1/2
A | B | 0 | -1.80886 | -x+1,y-1/2,-z+1/2
A | A | 0 | 0.111518 | x-1/2,-y+1/2,-z+1
[PLP]E:900 | A | 0.094355 | -0.23042 | x,y,z
[PLP]H:900 | B | 0.094355 | -0.36542 | x,y,z
A | A | 0 | 0.124812 | x-1,y,z
[GLC]F:995 | B | 0 | 2.50296 | x,y,z
[GLC]C:995 | A | 0 | 2.52312 | x,y,z
[GLC]F:996 | B | 0 | 3.2986 | x,y,z
[GLC]C:996 | A | 0 | 3.22187 | x,y,z
B | A | 0 | 3.66116 | -x+1/2,-y,z-1/2
[GLC]C:998 | A | 0.025217 | 3.01159 | x,y,z
[GLC]F:998 | B | 0.025217 | 2.95375 | x,y,z
[GLC]C:997 | A | 0 | 2.45925 | x,y,z
[GLC]F:997 | B | 0 | 2.37176 | x,y,z
[BGC]F:994 | B | 0 | 1.2581 | x,y,z
[BGC]C:994 | A | 0 | 0.920195 | x,y,z
B | A | 0 | -2.43397 | -x+1,y-1/2,-z+1/2
B | B | 0 | 1.02116 | x-1,y,z
[SO4]G:2999 | B | 0.382227 | -9.9445 | x,y,z
[SO4]D:1999 | A | 0.382227 | -10.0581 | x,y,z
[GLC]C:995 | [BGC]C:994 | 0 | 1.52676 | x,y,z
[GLC]F:995 | [BGC]F:994 | 0 | 1.42288 | x,y,z
[GLC]F:996 | [GLC]F:995 | 0 | 1.32526 | x,y,z
[GLC]C:996 | [GLC]C:995 | 0 | 1.28204 | x,y,z
[GLC]C:997 | [GLC]C:996 | 0 | 1.29394 | x,y,z
[GLC]F:997 | [GLC]F:996 | 0 | 1.27743 | x,y,z
B | B | 0 | 0.207328 | -x,y-1/2,-z+1/2
[GLC]F:998 | [GLC]F:997 | 0 | 0.761414 | x,y,z
[GLC]C:998 | [GLC]C:997 | 0 | 0.682544 | x,y,z
[SO4]G:2999 | [GLC]F:998 | 0.101796 | -3.32977 | x,y,z
[SO4]D:1999 | [GLC]C:998 | 0.101796 | -3.06167 | x,y,z
[PLP]H:900 | [SO4]G:2999 | 0.131857 | -4.28057 | x,y,z
[PLP]E:900 | [SO4]D:1999 | 0.131857 | -3.99833 | x,y,z
[SO4]G:2999 | [GLC]F:997 | 0.094466 | -2.95927 | x,y,z
[SO4]D:1999 | [GLC]C:997 | 0.094466 | -2.97197 | x,y,z
[PLP]H:900 | [GLC]F:998 | 0.017104 | -0.56505 | x,y,z
[PLP]E:900 | [GLC]C:998 | 0.017104 | -0.50888 | x,y,z
[GLC]C:997 | [GLC]C:995 | 0 | 0.011675 | x,y,z
[GLC]F:997 | [GLC]F:995 | 0 | 0.004026 | x,y,z
[GLC]F:996 | [BGC]F:994 | 0 | 0.004843 | x,y,z
[GLC]C:996 | [BGC]C:994 | 0 | 0.006433 | x,y,z
[GLC]C:998 | [GLC]C:996 | 0 | 0.013314 | x,y,z
[GLC]F:998 | [GLC]F:996 | 0 | 0.003201 | x,y,z

> provided by <https://www.ebi.ac.uk/pdbe/api/pisa/interfacelist/2aw3/0>

### Related Issues

* <https://github.com/biojava/biojava/pull/868>: Minimal read support for files with 'branched' entities


