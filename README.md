# Rule-Based-swahili-POS-tagger
# Rule-Based Part-of-Speech Tagger for Kiswahili

## Overview
This project implements a rule-based Part-of-Speech (POS) tagger specifically designed for Kiswahili, an agglutinative language. It leverages morphological analysis (word structure) and contextual analysis (surrounding words) to accurately assign grammatical categories to words.

## Project Aim
The objective of this project is to develop a Rule-Based Part-of-Speech Tagger for Kiswahili. Unlike English, Kiswahili is highly agglutinative, meaning a single word often contains the subject, tense, and object (e.g., hawajasafiri = "they have not traveled"). This project aims to identify the grammatical category of words using Morphological Analysis (word structure) and Contextual Analysis (surrounding words).

## Methodology
We utilize a two-step algorithm to assign tags:

### Step A: Morphological Tagging (Regex)
We apply a prioritized list of Regular Expressions to analyze the internal structure of the word. The order is crucial:

*   **Closed Classes**: We first check for specific known lists of Pronouns (mimi, wewe), Conjunctions (na, lakini), and Prepositions.

*   **Orthography**: We check for capitalization to identify Proper Nouns (NNP) and digits for Numbers (CD).

*   **Affix Analysis**: We analyze prefixes and suffixes common in Kiswahili grammar:
    *   **Prefixes**: Words starting with `ha-` (negative marker) or `ta-` (future tense) are likely verbs.
    *   **Suffixes**: Words ending in `-a` are often Verbs (kula). Words ending in `-o` or `-u` are often Adjectives (mdogo).

*   **Locatives**: Words ending in `-ni` are treated as Nouns/Locations (nyumbani).

### Step B: Contextual Correction (Brill's Logic)
After the initial tagging, we apply "Transformation Rules" to fix errors based on sentence context.

**Example**: If a word was tagged as a Noun (NN), but it follows a Preposition (IN), it is likely acting as a Verb (VB) in an infinitive sense (e.g., `kwa kula`).

## Tag Set Definitions
*   **NN**: Noun (Common)
*   **NNP**: Proper Noun (Names)
*   **VB**: Verb
*   **JJ**: Adjective
*   **RB**: Adverb
*   **PRP**: Personal Pronoun
*   **CC**: Conjunction
*   **IN**: Preposition
*   **CD**: Cardinal Number
*   **PRP$**: Possessive Pronoun

## Usage
To use the POS tagger, simply call the `pos_tag_kiswahili` function with your Kiswahili sentence:

```python
import re

# ... (define NEGATIONS, SUBJECTS, TENSES, tokenize_swahili_verb, patterns, context_rules as in the notebook)

# Example usage:
sentence = "mimi nitakula kitabu kikubwa"
tags = pos_tag_kiswahili(sentence)

for word, tag in tags:
    print(f"{word}: {tag}")
