<a name="Assignment-Lucky-People"></a>

# Assignment - Lucky People[¶](#Assignment-Lucky-People)

Your assignment is to test with TorXakis the Lucky People program.

<a name="Requirements"></a>

## Requirements[¶](#Requirements)

The Lucky People program gets as input a sequence of persons on socket 777.  
A person has a sex, first name, last name, day and month of birth.  
For each person, the Lucky People program produces one output, a string on socket 777.  
The string is either True or False, and reflect whether the person is considered lucky.  
A person is lucky when either

*   the first character of the first name is equal to the first character of the last name
*   the day of birth is equal to the month of birth, or
*   the person is a Female after five Males or a Male after five Females.

The input sequence is specified as followed:

<table>

<tbody>

<tr>

<td>persons  
</td>

<td>(person '\n')*  
</td>

<td>  
</td>

</tr>

<tr>

<td>person  
</td>

<td>sex separator firstName separator lastName separator dayOfBirth separator monthOfBirth  
</td>

<td>  
</td>

</tr>

<tr>

<td>separator  
</td>

<td>'@'  
</td>

<td>  
</td>

</tr>

<tr>

<td>sex  
</td>

<td>'Male' | 'Female'  
</td>

<td>  
</td>

</tr>

<tr>

<td>firstName  
</td>

<td>name  
</td>

<td>  
</td>

</tr>

<tr>

<td>lastName  
</td>

<td>name  
</td>

<td>  
</td>

</tr>

<tr>

<td>name  
</td>

<td>[A-Z][a-z]*  
</td>

<td>  
</td>

</tr>

<tr>

<td>dayOfBirth  
</td>

<td>Int  
</td>

<td>1 <= dayOfBirth <= 31  
</td>

</tr>

<tr>

<td>monthOfBirth  
</td>

<td>Int  
</td>

<td>1 <= monthOfBirth <= 12  
</td>

</tr>

</tbody>

</table>

<a name="Examples"></a>

## Examples[¶](#Examples)

<a name="Lucky-based-on-names"></a>

### Lucky based on names[¶](#Lucky-based-on-names)

*   Male@Mickey@Mouse@13@1
*   Male@Donald@Duck@13@3
*   Male@Luuk@Laar@24@12

<a name="Lucky-based-on-birthday"></a>

### Lucky based on birthday[¶](#Lucky-based-on-birthday)

*   Female@Shakira@Ripoll@2@2
*   Male@Michael@Buble@9@9
*   Female@Imke@Laar@7@7

<a name="Lucky-based-on-sequence"></a>

### Lucky based on sequence[¶](#Lucky-based-on-sequence)

In the following sequences of persons, the persons in italics are considered lucky due to their position in the sequence

*   Male@Huey@Duck@17@10  
    Male@Dewey@Duck@17@10  
    Male@Louie@Duck@17@10  
    Male@Mickey@Mouse@13@1  
    Male@Donald@Duck@13@3  
    _Female@April@Duck@15@5_

*   Female@Beatrix@Oranje@31@1  
    Female@Maxima@Zorreguieta@17@5  
    Female@Amalia@Oranje@7@12  
    Female@Alexia@Oranje@26@6  
    Female@Ariane@Oranje@10@4  
    _Male@Willem@Oranje@27@4_

<a name="Deliverables"></a>

## Deliverables[¶](#Deliverables)

Provide a TorXakis model: a file named 'LuckyPeople.txs' containing a [model definition](ModelDefs) and [connection definition](CnectDefs).  
Provide a test report: a file containing the verdict whether 'LuckyPeople.java' satisfy the requirements and the actual trace(s) used to test the program.