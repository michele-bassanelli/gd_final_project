### Overview

This report focuses on the development of a Spanish grammar, with particular emphasis on the agreement between subjects, copulas, and adjectives, as well as the aspectual interpretation of adjectives when used with the copulas *ser* and *estar*. The goal was to accurately capture the nuances of subject-verb agreement, gender and number agreement, and the correct usage of copulas in relation to adjectives that convey different aspectual meanings.

### Why I Chose This Phenomenon

In Spanish, the choice between the copulas *ser* and *estar* is tied to the semantic nuances of the second element in the copula construction.

**Aspectual Distinction**: The copula *ser* is typically used to denote inherent, permanent characteristics or identity, whereas *estar* is employed to express temporary states or conditions. For instance, *Juan es presidente* (Juan is president) uses *ser* to convey Juan's permanent role or identity as president, while *Juan está cansado* (Juan is tired) uses *estar* to describe a temporary state of being tired. Clements (1988) uses the example of "ser alto” (being tall) to illustrate how *ser* can indicate a more permanent or enduring attribute as part of a process.

**Agreement Complexity**: In addition to choosing the correct copula, Spanish requires that adjectives agree with the subject in gender and number.

- María está cansada.  
  **PROPN.F.SG be.PRS.3SG tired.F.SG**  
  “María is tired.”

The adjective *cansada* agrees with *María* in both gender and number, while also reflecting the temporary state described by *estar*.

Trechera (2002) highlights this complexity by noting that "la modificación inherente a los verbos de cambio de estado físico o mental puede ser descrita con *ESTAR* pero no con *SER*" ("the inherent modification of verbs of physical or mental state change can be described with *ESTAR* but not with *SER*"). This illustrates how the choice of copula affects not just grammatical agreement but also the semantic interpretation of the adjective.

### Implementation Approach/Design

#### LFG Framework

The implementation was based on the Lexical Functional Grammar (LFG) framework, using XLE. I chose the XCOMP analysis, although traditionally PREDLINK has also been used. XCOMP was easier to implement and offers more flexibility, especially for languages with agreement complexities like Spanish. As Dalrymple et al. (2004) point out, “the XCOMP analysis is particularly effective for languages like French and Norwegian, where the postcopular complement shows agreement with the subject of the copula.”

#### Challenges in Agreement

The primary challenges in implementing this grammar involved:

- **Adjective-Noun Agreement**: Adjectives must agree with the subject noun phrase in both gender and number, which is crucial for grammatical accuracy in Spanish.

- **Aspectual Agreement**: The choice between *ser* and *estar* often depends on whether the adjective describes a permanent or temporary state. Capturing this distinction required careful management of aspectual features within the grammar.

### Implementation Details

#### Agreement Rules

1. Agreement was ensured by using tags like `(^ SUBJ GEND) = fem` and `(^ SUBJ NUM) = pl` on the adjectives, and corresponding tags in the NP like `(^ NUM) = pl` and `(^ GEND) = fem`. This rule ensures that the subject, copula, and adjectives all agree in number and gender.

2. **Aspectual Agreement** was handled by associating specific aspectual features with the copulas and ensuring that the adjectives conformed to these features:

   ```lfg
   son  CopV * (^ PRED) = 'ser<(^ XCOMP)>(^ SUBJ)'
            (^ SUBJ) = (^ XCOMP SUBJ)
            (^ TENSE) = pres
			(^ XCOMP A-ASP) =c descriptive.

   altos A * (^ PRED) = 'alto<(^ SUBJ)>'
          (^ SUBJ GEND) = masc
          (^ SUBJ NUM) = pl
          (^ ATYPE) = predic
		  (^ A-ASP) = descriptive.
   ```

#### Example Sentences

1. **Las mujeres están felices** (The women are happy):
   - The adjective *felices* agrees with the subject *las mujeres* in gender (feminine) and number (plural).
   - The copula *están* agrees with the plural subject *las mujeres*.
   - The aspectual agreement rule applies since *felices* aligns with the temporary state described by *estar*.

   **Glossed Sentence:**  
   Las mujeres están felices.  
   **DEF.F.PL woman.F.PL be.PRS.3PL happy.PL**  
   ‘The women are happy.’

2. **Juan es presidente** (Juan is president):
   - The noun *presidente* agrees with the subject *Juan* in gender (masculine) and number (singular).
   - The copula *es* agrees with the singular subject *Juan*.
   - The noun *presidente* aligns with *ser*, indicating a more permanent state.

   **Glossed Sentence:**  
   Juan es presidente.  
   **PROPN.M.SG be.PRS.3SG president.M.SG**  
   ‘Juan is president.’

### Files
- es_grammar.lfg : this file contains the rules of the grammar 
- test.lfg : this file contains a list of test sentences 

### Conclusion

The development of this Spanish grammar highlights the intricacies of agreement and aspectual interpretation in copular constructions. By leveraging the LFG framework, the grammar successfully models the necessary agreements and captures the subtle distinctions between *ser* and *estar*. The approach ensures that sentences are both syntactically correct and semantically appropriate, reflecting the complexities of Spanish grammar.

### Bibliography

Clements, J. Clancy. 1988. "The semantics and pragmatics of the Spanish construction." *Language* 64, no. 4: 779-822.

Dalrymple, Mary, Dyvik, Helge, and King, Tracy Holloway. 2004. "Copular Complements: Closed or Open?" In *The LFG 04 Conference*, Christchurch, New Zealand, 188-198.

Trechera, Rafael Marín. 2002. *El componente aspectual de la predicación*. PPU.

