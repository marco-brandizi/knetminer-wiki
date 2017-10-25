Everything related to the evaluation of Neo4j as the graph database of KnetMiner...


Cypher queries representing the semantic motifs used in KnetMiner
```
//Q1: Path length = 1; All direct neighbours of target gene (ignore direction)
MATCH p = (g:Gene)--() 
WHERE g.name='RIN4'
RETURN p
UNION
//Q2: Path length = 2; direction: Gene-->A-->B
MATCH p = (g:Gene)-->()-[:ortho|:h_s_s|:xref|:has_domain|:associated_with|:pub_in|:has_function|:participates_in|:cat_c|:cooc_wi|:is_a]->()
WHERE g.name='RIN4'
RETURN p
UNION
//Q3: Path length = 2; special case: [cooc_wi] can be in both directions
MATCH p = (g:Gene)-->()-[:cooc_wi]-(:TO)
WHERE g.name='RIN4'
RETURN p
UNION
//Q4: Path length = 3; direction: Gene-->A-->B-->C
MATCH p = (g:Gene)-->()-[:ortho|:h_s_s|:xref|:has_domain]->()-[:pub_in|:has_function|:participates_in|:equ|:xref|:cat_c]->()
WHERE g.name='RIN4'
RETURN p
UNION
//Q5: Path length = 4; Gene-->A--B<-[enc]-C-->D
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:pub_in|:has_function|:participates_in|:has_variation|:has_observ_pheno|:differentially_expressed]->()
WHERE g.name='RIN4'
RETURN p
UNION
//Q6: Path length = 4; special case: Gene-->A--B<-[enc]-C-[cooc_wi]-D 
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:cooc_wi]-(:TO)
WHERE g.name='RIN4'
RETURN p
UNION
//Q7: Path length = 4; special case: Gene-->A--B<-[enc]-C-[physical|genetic]-D; merging Q6 and Q7 is twice as slow 
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:physical|:genetic]-()
WHERE g.name='RIN4'
RETURN p
UNION
//Q8: Path length = 5; Gene-->A--B<-[enc]-C-->SNP-->Trait
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:has_variation]->()-[:associated_with]->(:Trait)
WHERE g.name='RIN4'
RETURN p
UNION
//Q9: Path length = 5; Gene-->A--B<-[enc]-C--D-->E
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:physical|:genetic]-()-[:has_function|:participates_in|:has_observ_pheno]->()
WHERE g.name='RIN4'
RETURN p
UNION
//Q10: Path length = 5; Gene-->Protein-->Protein<-[enc]-Gene-->Gene--TO
MATCH p = (g:Gene {name:'RIN4'})-[:enc]->(:Protein)-[:ortho]->(:Protein)<-[:enc]-(:Gene)-[:physical|:genetic]-(:Gene)-[:cooc_wi]-(:TO)
WHERE g.name='RIN4'
RETURN p
UNION
//Q11: Path length = 6; Gene-->A-->B<-[enc]-Gene--Gene-->SNP-->Trait
MATCH p = (g:Gene)-->()-[:ortho]->()<-[:enc]-()-[:physical|:genetic]-()-[:has_variation]->()-[:associated_with]->(:Trait)
WHERE g.name='RIN4'
RETURN p
UNION
//Q12: Path length = 6; Gene-->A-->B-[xref]-Protein-->Enzyme--Reaction-->Pathway
MATCH p = (g:Gene)-->()-[:ortho]->()-[:xref]-()-[:is_a]-()-[:ca_by]-()-[:part_of]->(:Path)
WHERE g.name='RIN4'
RETURN p
```