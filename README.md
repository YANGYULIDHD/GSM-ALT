# GSM-ALT
This is the repository for our dataset GSM-ALT, which was used to evaluate LLMs' robustness to numerical variations in mathematical reasoning tasks.

## Dataset Summary
GSM-ALT is a dataset of 250 abstracted templates created from the questions randomly sampled from GSM8K (Grade School Math 8K) dataset \[Cobbe et al., 2021\]. 
For a question in GSM8K, we manually check it and rewrite some of the numerical values in the question text, process and final answer into variables like x, y, z, ..., to create their abstracted forms (which construct the template).
With the template, you can automatically generate multiple variants of the original questions by replacing the variables with random values, which can be used to evaluate LLMs' robustness to numerical variations in mathematical reasoning tasks.
However, to ensure the validity of the generated variants, replaced values in the template should meet certain constraints (e.g., x/2 should be a whole number if it represents the number of something). 
We manually annotate the constraints for each template.
When generating variants, you can accept only those that meet the constrains to ensure their qualities.

## Data Structure

### Data Fields
- question: the original question string in "GSM8K".
- process: the original process string to the "question" in "GSM8K". It contains multiple steps of reasoning and the final numerical answer.
- final_answer: the final answer extracted from the "process".
- abstracted_question: the abstracted question string corresponding to the 'question'.
- abstracted_process: the abstracted process string corresponding to the 'process'.
- abstracted_final_answer: the abstracted final answer corresponding to the 'final_answer'.
- constraints: the constraints string. Multiple constraints are seperated by '###'.

### Data Instance
```
{
    'question':'After receiving the $2000 stimulus check, Mr. Eithan decided to share the amount with his family. He gave 2\/5 of the amount to his wife, 2\/5 of the remaining amount to his first son, 40% of the remaining amount to his second son, and kept the remaining in their family savings account. Calculate the total amount he kept in the family's savings account.'
    'process':'The total amount of money Mr. Eithan gave to his wife is 2\/5*2000 = $<<2\/5*2000=800>>800\nAfter giving his wife $800, he remained with $2000-$800=$<<2000-800=1200>>1200\nHe gave his first son 2\/5 of the remaining amount which is 2\/5*$1200 = $<<2\/5*1200=480>>480\nThe total amount remaining after he gave his first 2\/5 of the amount is $1200-$480 = $<<1200-480=720>>720\nHe then gave his second son 40\/100*720 = $<<40\/100*720=288>>288 of the money.\nAfter giving his second son $288, the amount of money remaining that he saved in the family's saving account is $720-$288=$432\n#### 432'
    'final_answer':432
    'abstracted_question':'After receiving the $x stimulus check, Mr.Eithan decided to share the amount with his family. He gave y\/z of the amount to his wife, p\/q of the remaining amount to his first son, i% of the remaining amount to his second son, and kept the remaining in their family savings account. Calculate the total amount he kept in the family's savings account.'
    'abstracted_process':'The total amount of money Mr.Eithan gave to his wife is $(y\/z*x).\nAfter giving his wife $(y\/z*x), he remained with $<<x - y\/z*x = (1-y\/z)*x>>((1-y\/z)*x).\nHe gave his first son p\/q of the remaining amount which is $(p\/q*(1-y\/z)*x).\nThe total amount remaining after he gave his first son p\/q of the amount is $<<(1-y\/z)*x - p\/q*(1-y\/z)*x = (1-p\/q)*(1-y\/z)*x>>((1-p\/q)*(1-y\/z)*x).\nHe then gave his second son $(i\/100*(1-p\/q)*(1-y\/z)*x) of the money.\nAfter giving his second son $(i\/100*(1-p\/q)*(1-y\/z)*x), the amount of money remaining that he saved in the family's saving account is $<<(1-p\/q)*(1-y\/z)*x - i\/100*(1-p\/q)*(1-y\/z)*x = (1-i\/100)*(1-p\/q)*(1-y\/z)*x>>((1-i\/100)*(1-p\/q)*(1-y\/z)*x).\nSo, the final answer is: (1-i\/100)*(1-p\/q)*(1-y\/z)*x'
    'abstracted_final_answer':'(1-i\/100)*(1-p\/q)*(1-y\/z)*x'
    'constraints':'y<z ### p<q ### i<100'
}
```
**Note: In addition to the constraints annotated in our dataset, there is a common constraint for all templates. That is the "final_answer" should be a postive, whole number.**
