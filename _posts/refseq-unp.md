---
title: Feactures of Mapping Between RefSeq and UniProt
author: Zefeng Zhu
date: 2019-08-21 08:30:10 +0800
categories: [Notes, Research]
tags: [protein, pdb, protein structure, mapping]
---

## Example

Gene|NOTCH2NL|CALM2|ECE2|C4B
-|-|-|-|-
RefSeq Protein|NP_982283|NP_001734|NP_001032401|NP_001002029
RefSeq Nucleotide|NM_203458|NM_001743|NM_001037324|NM_001002029
(UniProt) Entry |{P0DPK4,Q7Z3S9}|{P0DP23,P0DP24,P0DP25}|{P0DPD6,P0DPD8}|{P0C0L4,P0C0L5}
Gene names | {NOTCH2NLC, (NOTCH2NLA N2N, NOTCH2NL)} | {CALM1, CALM2, CALM3} | {C4A,C4B}
Idencial Sequences|True|True|False|False
isoform| {False,True}|{False,False,False}|{True,True(Reference:P0DPD6)}|{True,False}
PDB|{False, False}|{True,True,True}| {False,False}|{True, True}


### Alignment (e.g)

```clustal
sp|P0C0L4|CO4A_HUMAN      FVLKVLSLAQEQVGGSPEKLQETSNWLLSQQQADGSFQDPCPVLDRSMQGGLVGNDETVA
sp|P0C0L5|CO4B_HUMAN      FVLKVLSLAQEQVGGSPEKLQETSNWLLSQQQADGSFQDLSPVIHRSMQGGLVGNDETVA
NP_001002029              FVLKVLSLAQEQVGGSPEKLQETSNWLLSQQQADGSFQDLSPVIHRSMQGGLVGNDETVA
                          *************************************** .**:.***************
```

### Superimpose

  <script src="../assets/js/ngl.js"></script>
  <script>
    document.addEventListener("DOMContentLoaded", function () {
      var stage1 = new NGL.Stage("viewport1");
      var stage2 = new NGL.Stage("viewport2");
      stage1.loadFile("../assets/data/5wsvA.4lzxA_FATCAT.pdb", {defaultRepresentation: true});
      stage2.loadFile("../assets/data/5wsvA.4lzxA_MultiProt.pdb").then(function (o) {
        o.addRepresentation("cartoon", { color: "modelindex" })
        o.autoView()
        });
      stage1.spinAnimation.axis.set(0, 1, 0);
      stage1.setSpin(true);
      stage2.spinAnimation.axis.set(1, 0, 0);
      stage2.setSpin(true);
    });
  </script>
  <table>
    <tr>
        <td>
            Superimpose result of FATCAT
        </td>
        <td>
          Superimpose result of MultiProt
        </td>
    </tr>
    <tr>
        <td>
            <div id="viewport1" style="width:20em; height:15em;"></div>
        </td>
        <td>
            <div id="viewport2" style="width:20em; height:15em;"></div>
        </td>
    </tr>
</table>

```clustal
4LZX:A|PDBID|CHAIN|SEQUENCE      -----ADQLTEEQIAEFKEAFSLFDKDGDGTITTKELGTVMRSLGQNPTEAELQDMINEV
sp|P0DP24|CALM2_HUMAN            ----MADQLTEEQIAEFKEAFSLFDKDGDGTITTKELGTVMRSLGQNPTEAELQDMINEV
5WSV:A|PDBID|CHAIN|SEQUENCE      GPGSMADQLTEEQIAEFKEAFSLFDKDGDGTITTKELGTVMRSLGQNPTEAELQDMINEV
                                      *******************************************************

4LZX:A|PDBID|CHAIN|SEQUENCE      DADGNGTIDFPEFLTMMARKMKDTDSEEEIREAFRVFDKDGNGYISAAELRHVMTNLGEK
sp|P0DP24|CALM2_HUMAN            DADGNGTIDFPEFLTMMARKMKDTDSEEEIREAFRVFDKDGNGYISAAELRHVMTNLGEK
5WSV:A|PDBID|CHAIN|SEQUENCE      DADGNGTIDFPEFLTMMARKMKDTDSEEEIREAFRVFDKDGNGYISAAELRHVMTNLGEK
                                 ************************************************************

4LZX:A|PDBID|CHAIN|SEQUENCE      LTDEEVDEMIREADIDGDGQVNYEEFVQMMTAK
sp|P0DP24|CALM2_HUMAN            LTDEEVDEMIREADIDGDGQVNYEEFVQMMTAK
5WSV:A|PDBID|CHAIN|SEQUENCE      LTDEEVDEMIREADIDGDGQVNYEEFVQMMT--
                                 ******************************* 
```

---

### Javascript Script For Displaying SuperImpose Results

> fork ngl.js from https://github.com/arose/ngl

```js
document.addEventListener("DOMContentLoaded", function () {
    var stage1 = new NGL.Stage("viewport1");
    var stage2 = new NGL.Stage("viewport2");
    stage1.loadFile("../assets/data/5wsvA.4lzxA_FATCAT.pdb", {defaultRepresentation: true});
    stage2.loadFile("../assets/data/5wsvA.4lzxA_MultiProt.pdb").then(function (o) {
        o.addRepresentation("cartoon", { color: "modelindex" })
        o.autoView()
    });
    stage1.spinAnimation.axis.set(0, 1, 0);
    stage1.setSpin(true);
    stage2.spinAnimation.axis.set(1, 0, 0);
    stage2.setSpin(true);
    });
```