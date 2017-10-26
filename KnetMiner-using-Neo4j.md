Everything related to the evaluation of using Neo4j as the graph database of KnetMiner...


Cypher queries representing the semantic motifs used in KnetMiner
```sql
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

`Cypher version: CYPHER 3.2, planner: COST, runtime: INTERPRETED. 5168 total db hits in 78 ms`

Same query in SPARQL:
```sql
PREFIX  odx:  <http://ondex.sourceforge.net/ondex-core#>
PREFIX  odxc: <http://www.ondex.org/ex/concept/>
PREFIX  odxcc: <http://www.ondex.org/ex/conceptClass/>
PREFIX  rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX  odxr: <http://www.ondex.org/ex/relation/>
PREFIX  odxrt: <http://www.ondex.org/ex/relationType/>

SELECT DISTINCT ?g ?a ?b ?c ?d ?e
{
  ?g a odxcc:Gene;
     odx:conceptName 'RIN4'.

  # Q1
  ?g ?pa ?a.
  ?a odx:conceptName ?aname.

  # Q3
  OPTIONAL {
    ?a odxrt:cooc_wi ?b.
    ?b a odxcc:TO.
  }

  OPTIONAL {
    # Q2
    { ?a odxrt:ortho|odxrt:h_s_s|odxrt:xref|odxrt:has_domain|odxrt:associated_with|odxrt:pub_in|odxrt:has_function|odxrt:participates_in|odxrt:cat_c|odxrt:cooc_wi|odxrt:is_a ?b }

    # Q12
    UNION {
      ?a odxrt:ortho ?b.
      ?b (odxrt:xref|^odxrt:xref)/(odxrt:is_a|^odxrt:is_a)/(odxrt:ca_by|^odxrt:ca_by)/(odxrt:part_by|^odxrt:part_of) ?c.
      ?c a odxcc:Path
    }

    UNION {
      ?a odxrt:ortho ?b.
      ?c odxrt:enc ?b.

      # Q5
      { ?c odxrt:pub_in|odxrt:has_function|odxrt:participates_in|odxrt:has_variation|odxrt:has_observ_pheno|odxrt:differentially_expressed ?d. }

      # Q6
      UNION { ?c odxrt:cooc_wi ?to. ?d a odxcc:TO }

      # Q7
      UNION { ?c odxrt:physical|^odxrt:physical|odxrt:genetic|^odxrt:genetic ?d }

      # Q8
      UNION { ?c odxrt:has_variation/odxrt:associated_with ?e. ?e a odxcc:Trait }

      UNION {
        ?c (odxrt:physical|^odxrt:physical|odxrt:genetic|^odxrt:genetic) ?d

        # Q9
        { ?d (odxrt:has_function|odxrt:participates_in|odxrt:has_observ_pheno) ?e }

        # Q10
        UNION {
          ?d odxrt:cooc_wi ?e.
          ?c a odxcc:Gene.
          ?d a odxcc:Gene.
          ?e a odxcc:TO.
        }

        # Q11
        UNION { ?d odxrt:has_variation ?e }
      } # level following c
    } # level following b 
  } # level following ?a 
}
LIMIT 1000
```

Required time is 192ms against the wheat network, with a similar hardware. 

Notes about SPARQL:
  * The compactness/readability could be improved by defining a better RDF schema. Eg, `?a odx:conceptName ?aname` is weird, at the moment it's the only way to restrict ?a to concepts, since they're not made instances of a 'Concept' (they're instances of some concept class, but it's hard to match this other condition). In a better schema, it would be: `?a a odx:Concept`
  * Although Cypher forces to linear paths and redundancy (see next point), it appears to have a slightly more compact syntax, e.g.: 
    * there is no SPARQL equivalent for unbounded direction edges like `()-[:physical]-()`. This has to be specified via `?x odxrt:physical|^odxrt:physical ?y`, where `?x ^p ?y` means: `?y p ?x`.
    * there is no equivalent for in-line type specification, e.g., `(g:Gene{name:'RIN4'})` has to be written with two patterns: `?g a odxcc:Gene; odx:conceptName 'RIN4'.` (the ';' separator tells that the subject of the second triple is still ?g).
    * variables can be omitted only in certain cases, e.g., it can here: `?gene odxrt:encodes/odxrt:xref ?pub`, however, the equivalent of: `(g)-[:encodes]->(:Protein)-[:xref]->(pub)` has to be written as `?gene odxrt:encodes ?p. ?p a odxcc:Protein. ?p odxrt:xref ?pub`.
    * Variables need to be introduced in order to specify any property for an edge, e.g., `(gene)--(prot)` has to be translated as `?gene ?p ?prot`, where ?p is unbounded (so, it can be bounded to whaterver property) and can be omitted from the results (from `SELECT`).
  * The same query above can be written with fewer unions, at the price of more redundancy, the same way it is done for Cypher. Unions and branching with nested queries tend to be preferable in SPARQL, since its syntax is less compact.
